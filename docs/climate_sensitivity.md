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

## Equilibrium climate sensitivity
Go~


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





