---
jupytext:
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

(ecs_feedback)=

# Climate sensitivity and feedback

## Radiative forcing
We start with the radiative forcing in a RCE model. Recall that the radiative forcing is the quick change in the TOA energy budget before the climate system begins to adjust. If we consider the forcing as doubling CO2, we could write the radiative forcing as:
```{math}
:label: my_label61
\Delta R = (ASR_{2xCO2}-OLR_{2xCO2}) - (ASR_{1xCO2}-OLR_{1xCO2}).
```

What is happening after we add more CO2 in the atmosphere?

- The atmosphere is less efficient at radiating energy away to space (reducing emissivity, right?).
- OLR will decrease
- The climate system will begin gaining energy.

Now let's build a RCE model and analyze its radiative forcing.

```{code-cell} ipython3
#  This code is used just to create the skew-T plot of global, annual mean air temperature
%matplotlib inline
import numpy as np
import matplotlib.pyplot as plt
import xarray as xr
from metpy.plots import SkewT
import climlab

#plt.style.use('dark_background')

#  This code is used just to create the skew-T plot of global, annual mean air temperature
#ncep_url = "http://www.esrl.noaa.gov/psd/thredds/dodsC/Datasets/ncep.reanalysis.derived/"
ncep_url = "/Users/yuchiaol_ntuas/Desktop/ebooks/data/"
#time_coder = xr.coders.CFDatetimeCoder(use_cftime=True)
ncep_air = xr.open_dataset( ncep_url + "air.mon.1981-2010.ltm.nc", use_cftime=True)
#  Take global, annual average 
coslat = np.cos(np.deg2rad(ncep_air.lat))
weight = coslat / coslat.mean(dim='lat')
Tglobal = (ncep_air.air * weight).mean(dim=('lat','lon','time'))

print(Tglobal)

#  Resuable function to plot the temperature data on a Skew-T chart
def make_skewT():
    fig = plt.figure(figsize=(9, 9))
    skew = SkewT(fig, rotation=30)
    skew.plot(Tglobal.level, Tglobal, color='black', linestyle='-', linewidth=2, label='Observations')
    skew.ax.set_ylim(1050, 10)
    skew.ax.set_xlim(-90, 45)
    # Add the relevant special lines
    skew.plot_dry_adiabats(linewidth=0.5)
    skew.plot_moist_adiabats(linewidth=0.5)
    #skew.plot_mixing_lines()
    skew.ax.legend()
    skew.ax.set_xlabel('Temperature (degC)', fontsize=14)
    skew.ax.set_ylabel('Pressure (hPa)', fontsize=14)
    return skew

#  and a function to add extra profiles to this chart
def add_profile(skew, model, linestyle='-', color=None):
    line = skew.plot(model.lev, model.Tatm - climlab.constants.tempCtoK,
             label=model.name, linewidth=2)[0]
    skew.plot(1000, model.Ts - climlab.constants.tempCtoK, 'o', 
              markersize=8, color=line.get_color())
    skew.ax.legend()

```

```{code-cell} ipython3
# Get the water vapor data from CESM output
#cesm_data_path = "http://thredds.atmos.albany.edu:8080/thredds/dodsC/CESMA/"
cesm_data_path = "/Users/yuchiaol_ntuas/Desktop/ebooks/data/"
atm_control = xr.open_dataset(cesm_data_path + "cpl_1850_f19.cam.h0.nc")
# Take global, annual average of the specific humidity
weight_factor = atm_control.gw / atm_control.gw.mean(dim='lat')
Qglobal = (atm_control.Q * weight_factor).mean(dim=('lat','lon','time'))

#  Make a model on same vertical domain as the GCM
mystate = climlab.column_state(lev=Qglobal.lev, water_depth=2.5)
#  Build the radiation model -- just like we already did
rad = climlab.radiation.RRTMG(name='Radiation',
                              state=mystate, 
                              specific_humidity=Qglobal.values,
                              timestep = climlab.constants.seconds_per_day,
                              albedo = 0.25,  # surface albedo, tuned to give reasonable ASR for reference cloud-free model
                             )
#  Now create the convection model
conv = climlab.convection.ConvectiveAdjustment(name='Convection',
                                               state=mystate,
                                               adj_lapse_rate=6.5,
                                               timestep=rad.timestep,
                                              )
#  Here is where we build the model by coupling together the two components
rcm = climlab.couple([rad, conv], name='Radiative-Convective Model')

print(rcm)

print(rcm.subprocess['Radiation'].absorber_vmr['CO2'])

print(rcm.integrate_years(5))

print(rcm.ASR - rcm.OLR)

```


Now we make another RCE model with CO2 doubled!

```{code-cell} ipython3
# Make an exact clone with same temperatures
rcm_2xCO2 = climlab.process_like(rcm)
rcm_2xCO2.name = 'Radiative-Convective Model (2xCO2 initial)'

#  Check to see that we indeed have the same CO2 amount
print(rcm_2xCO2.subprocess['Radiation'].absorber_vmr['CO2'])

#  Now double it!
rcm_2xCO2.subprocess['Radiation'].absorber_vmr['CO2'] *= 2

print(rcm_2xCO2.subprocess['Radiation'].absorber_vmr['CO2'])

```

Let's quickly chech the instantaneous radiative forcing:
```{code-cell} ipython3
rcm_2xCO2.compute_diagnostics()

print(rcm_2xCO2.ASR - rcm.ASR)

print(rcm_2xCO2.OLR - rcm.OLR)

DeltaR_instant = (rcm_2xCO2.ASR - rcm_2xCO2.OLR) - (rcm.ASR - rcm.OLR)

print(DeltaR_instant)

```
What is the instantaneous radiative forcing in our two/three-layer model?

Now we briefly calculate the adjusted radiative forcing. Recall that the adjusted radiative forcing is that the stratosphere is adjusted while the troposphere does not.

```{code-cell} ipython3
# print out troposphere
print(rcm.lev[13:])

rcm_2xCO2_strat = climlab.process_like(rcm_2xCO2)
rcm_2xCO2_strat.name = 'Radiative-Convective Model (2xCO2 stratosphere-adjusted)'
for n in range(1000):
    rcm_2xCO2_strat.step_forward()
# hold tropospheric and surface temperatures fixed
    rcm_2xCO2_strat.Tatm[13:] = rcm.Tatm[13:]
    rcm_2xCO2_strat.Ts[:] = rcm.Ts[:]

DeltaR = (rcm_2xCO2_strat.ASR - rcm_2xCO2_strat.OLR) - (rcm.ASR - rcm.OLR)
print(DeltaR)

```

Radiative forcing gives us some insights about how the changes of forcing could tranfer energy to the system, and temperature might change. But not the final temperature reaching equilibrium.

## Equilibrium climate sensitivity (ECS) without feedback
We define the ECS as "The global mean surface warming necessary to balance the planetary energy budget after a doubling of atmospheric CO2".

If we assume incoming shortwave radiation does not change (e.g., the planetary albedo does not change) after doubling CO2, the climate system has to adjust to new equilibrium temperature. This new temperature must balance the radiative forcing. So, we can get:
```{math}
:label: my_label62
\Delta R = OLR_{f} - OLR_{2xCO2},
```
where $OLR_{f}$ is the equilibrated $OLR$ associated with the new qeuilibrium temperature.

We know that from our zero-dimensional energy-balance model, we derive the OLR sensitivity to temperature change:
```{math}
:label: my_label63
\lambda_{0} = 4\tau\sigma T_{eq}^{3} \approx 3.3 \mbox{ W/m$^2$/K}
```

So we can write:
```{math}
:label: my_label64
OLR_{f} \approx OLR_{2xCO2} + \lambda_{0}\Delta T_{0},
```

The energy balance gives us:
```{math}
:label: my_label65
\Delta R = OLR_{f} - OLR_{2xCO2} \approx OLR_{2xCO2} + \lambda_{0}\Delta T_{0} - OLR_{2xCO2} = \lambda_{0}\Delta T_{0}
```
```{math}
:label: my_label66
\rightarrow \Delta T_{0} = \frac{\Delta R}{\lambda_{0}}
```

Assume the actual radiative forcing after doubling CO2 is about 4 W/m$^2$/K, we can get:
```{math}
:label: my_label67
\Delta T_{0} = \frac{\Delta R}{\lambda_{0}} = \frac{4}{3.3} \approx 1.2 \mbox{ K}
```

This is the ECS without feedback using zero-dimensional model!!!

```{code-cell} ipython3
OLRobserved = 238.5  # in W/m2
sigma = 5.67E-8  # S-B constant
Tsobserved = 288.  # global average surface temperature
tau = OLRobserved / sigma / Tsobserved**4  # solve for tuned value of transmissivity

lambda_0 = 4 * sigma * tau * Tsobserved**3

DeltaR = 4.  # Radiative forcing in W/m2
DeltaT0 = DeltaR / lambda_0
print( 'The Equilibrium Climate Sensitivity in the absence of feedback is {:.1f} K.'.format(DeltaT0))
```

Now we use RCE models to estimate ECS.
```{code-cell} ipython3
rcm_2xCO2_eq = climlab.process_like(rcm_2xCO2_strat)
rcm_2xCO2_eq.name = 'Radiative-Convective Model (2xCO2 equilibrium)'
rcm_2xCO2_eq.integrate_years(5)

print('rcm_2xCO2_eq.ASR - rcm_2xCO2_eq.OLR')
print(rcm_2xCO2_eq.ASR - rcm_2xCO2_eq.OLR)

skew = make_skewT()
add_profile(skew, rcm_2xCO2_strat)
add_profile(skew, rcm_2xCO2_eq)
add_profile(skew, rcm)

ECS_nofeedback = rcm_2xCO2_eq.Ts - rcm.Ts
print('ECS with no feedback')
print(ECS_nofeedback)

print('rcm_2xCO2_eq.OLR - rcm.OLR')
print(rcm_2xCO2_eq.OLR - rcm.OLR)
print('rcm_2xCO2_eq.ASR - rcm.ASR')
print(rcm_2xCO2_eq.ASR - rcm.ASR)

```




## Homework assignment 4 (due xxx)
1. In the pure radiative model, can you remove the effect of ozone?
2. In the pure radiative model, can you remove the effects of ozone and water vapor?
3. In the RCE model, chosse an initial condition with temperature reaching radiative equilibrium.
4. In the RCE model, chosse an isothermal initial condition with temperature 360 K and 170 K. Can you plot the temperature evolution at surface, 800 hPa, 500 hPa, 200 hPa, and 100 hPa, with time? 

## Final project 4
The key parameter 'adj_lapse_rate' determines the RCE equilibrium, but setting it as a constant may be somewhat idealized. Can you replace it with:
- adj_lapse_rate = 9.8. This is the dry adiabatic lapse rate.
- adj_lapse_rate = 'pseudoadiabat'. This follows the blue moist adiabats on the skew-T diagrams.
- more realistic lapse-rate from observations? For example from sounding?





