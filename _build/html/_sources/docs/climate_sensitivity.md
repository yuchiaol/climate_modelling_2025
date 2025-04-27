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
We start with the radiative forcing in a RCE model. Recall that the instantaneous radiative forcing is the quick change in the TOA energy budget before the climate system begins to adjust. 

```{figure} /_static/lecture_specific/lecture1_figures/hansen_1997_rf.png
:scale: 50%
```

If we consider the forcing as doubling CO2, we could write the radiative forcing as:
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

[Sherwood et al. (2020)](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2019RG000678) summarize the radiative forcing:
```{figure} /_static/lecture_specific/lecture1_figures/sherwood_erf.jpg
:scale: 30%
```


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

DeltaR_test = 4.  # Radiative forcing in W/m2
DeltaT0 = DeltaR_test / lambda_0
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

lambda0 = DeltaR / ECS_nofeedback
print('lambda0')
print(lambda0)

```
## Climate feedback
I guess that the feedback concept is inherited from electronic engineering. 
For example, Professor Gu-Yeon Wei's lecture 18 introduced the feedback concept [(here)](https://in.ncu.edu.tw/~ncume_ee/harvard-es154/lect_18_feedback.pdf):

```{figure} /_static/lecture_specific/lecture1_figures/feedback0.png
:scale: 50%
```

In climate science, we can think of "Source" as forcing or input added to the system, A as the whole process in the system, $\beta$ (gain) as the amplifier or deamplifier, and the load is the output.

We use $f$ to quantify the effect and strength of $\beta$ (gain). If it is positive ($f>0$), we call it amplifying feedback. If it is negative ($f<0$), we call it dampling feedback. 

Can you give examples for positive and negative feedbacks in the atmosphere or climate system?

In Arctic climate system, the feedbacks could be complex. In [Goosse et al. (2018)](https://www.nature.com/articles/s41467-018-04173-0), they summarized some feedbacks in the Arctic:

```{figure} /_static/lecture_specific/lecture1_figures/arctic_feedback.png
:scale: 30%
```

Let's work on how to quantify the effect of feedbacks. We strat with the temperature change considered in a system without feedback:
```{math}
:label: my_label68
\Delta T_{0} = \frac{\Delta R}{\lambda_{0}}
```

We assume that a positive feedback (e.g., water vapor feedback) is included and taking action. So its effect is to "enhance" the temperature change positively with a fraction of $f$:

```{math}
:label: my_label69
f\lambda_{0}\Delta T_{0}
```

```{note}
```{math}
:label: my_label70
\mbox{why  } -\infty < f \le 1 \mbox{ ???}
```

With infinity loops, we take the sume of each feedback effect:
```{math}
:label: my_label71
f\lambda_{0}\Delta T_{0} + f^{2}\lambda_{0}\Delta T_{0} + \cdots = (f+f^{2}+\cdots)\lambda_{0}\Delta T_{0} = \lambda_{0}\Delta T_{0}\sum_{n=1}^{\infty}f^{n}
```
 
Sum the original response to forcing $\Delta R = \lambda_{0}\Delta T_{0}$,
```{math}
:label: my_label72
\lambda_{0}\Delta T_{0} + \lambda_{0}\Delta T_{0}\sum_{n=1}^{\infty}f^{n} = \lambda_{0}\Delta T_{0}\sum_{n=0}^{\infty}f^{n} = \frac{1}{1-f}\lambda_{0}\Delta T_{0}
```

So the final response is
```{math}
:label: my_label73
\lambda_{0}\Delta T = \frac{1}{1-f}\lambda_{0}\Delta T_{0}
```
```{math}
:label: my_label74
\rightarrow \Delta T = \frac{1}{1-f}\Delta T_{0}
```

We define the gain as:
```{math}
:label: my_label75
g = \frac{1}{1-f} = \frac{\Delta T}{\Delta T_{0}}
``` 

What does this mean? How about a negative feedback?

If the system has N individual feedbacks, the total effect can be summed:
```{math}
:label: my_label76
f = f_{1} + f_{2} + \cdots f_{N} = \sum_{i=1}^{N}f_{i}
```

The ECS now becomes:
```{math}
:label: my_label77
\Delta T_{2xCO2} = \frac{1}{1-\sum_{i=1}^{N}f_{i}}\Delta T_{0} = \frac{1}{1-\sum_{i=1}^{N}f_{i}}\frac{\Delta R}{\lambda_{0}}
```

We can define feedback parameter in the unit of W/m$^2$/K as:
```{math}
:label: my_label78
\lambda_{i} = f_{i}\lambda_{0}
```

Then:
```{math}
:label: my_label79
\Delta T_{2xCO2} = \frac{\Delta R}{\lambda_{0}-\sum_{i=1}^{N}\lambda_{i}}
```

So the total feedback (parameter) could be decompose into:
```{math}
:label: my_label80
\lambda = \lambda_{0} - \sum_{i=1}^{N}\lambda_{i}
```

```{note}
$\lambda_{0}=3.3$ W/m$^2$/K is the Planck feedback parameter. This is not a real feedback (right?), and simply as a consequence of a warmer world emits more longwave radiation than a colder world.
```

This formulation is very useful, becasue we can qunatitatively compare the importance of each feedback. Below is an example from my paper:
```{figure} /_static/lecture_specific/lecture1_figures/my_feedback0.png
:scale: 70%
```

Now let's add a water vapor feedback into our RCE model and calculate ECS and feedback parameter.

```{code-cell} ipython3
#  actual specific humidity
q = rcm.subprocess['Radiation'].specific_humidity
#  saturation specific humidity (a function of temperature and pressure)
qsat = climlab.utils.thermo.qsat(rcm.Tatm, rcm.lev)
#  Relative humidity
rh = q/qsat

#  Plot relative humidity in percent
fig,ax = plt.subplots()
ax.plot(q*1000, rcm.lev, 'b-')
ax.invert_yaxis()
ax.grid()
ax.set_ylabel('Pressure (hPa)')
ax.set_xlabel('Specific humidity (g/kg)', color='b')
ax.tick_params('x', colors='b')
ax2 = ax.twiny()
ax2.plot(rh*100., rcm.lev, 'r-')
ax2.set_xlabel('Relative humidity (%)', color='r')
ax2.tick_params('x', colors='r')

```

We consider the relative humidity somewhat fixed, while saturation specific humidity varied with temperature changes.

```{code-cell} ipython3
rcm_2xCO2_h2o = climlab.process_like(rcm_2xCO2)
rcm_2xCO2_h2o.name = 'RCE Model (2xCO2 equilibrium with H2O feedback)'

for n in range(2000):
    # At every timestep
    # we calculate the new saturation specific humidity for the new temperature
    #  and change the water vapor in the radiation model
    #  so that relative humidity is always the same
    qsat = climlab.utils.thermo.qsat(rcm_2xCO2_h2o.Tatm, rcm_2xCO2_h2o.lev)
    rcm_2xCO2_h2o.subprocess['Radiation'].specific_humidity[:] = rh * qsat
    rcm_2xCO2_h2o.step_forward()

# Check for energy balance
print(rcm_2xCO2_h2o.ASR - rcm_2xCO2_h2o.OLR)

skew = make_skewT()
add_profile(skew, rcm)
add_profile(skew, rcm_2xCO2_strat)
add_profile(skew, rcm_2xCO2_eq)
add_profile(skew, rcm_2xCO2_h2o)

```

```{code-cell}ipython
ECS = rcm_2xCO2_h2o.Ts - rcm.Ts
print('ECS')
print(ECS)

g = ECS / ECS_nofeedback
print('gain')
print(g)

lambda_net = DeltaR / ECS
print('lambda_net')
print(lambda_net)

print('lambda0')
print(lambda0)

lambda_h2o = lambda0 - lambda_net
print('lambda_h2o')
print(lambda_h2o)

```

```{math}
:label: my_label81
\lambda_{H2O} = \lambda_{0} - \lambda = 3.3 - 1.43 \approx 1.87
```

"For every 1 degree of surface warming, the increased water vapor greenhouse effect provides an additional 1.86 W m$^{-2}$ of radiative forcing."


[Flato et al. (2013)](https://www.ipcc.ch/report/ar5/wg1/) in IPCC AR5 reported the climate feedbacks:
```{figure} /_static/lecture_specific/lecture1_figures/flato_feedbacks.png
:scale: 50%
```

[Sherwood et al. (2020)](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2019RG000678) summarize the climate feedbacks:
```{figure} /_static/lecture_specific/lecture1_figures/sherwood_feedbacks.jpg
:scale: 40%
```

## The physical model - putting together

We review Section 2.2 of [Sherwood et al. (2020)](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2019RG000678) to put everything together. This is the conventional model for feedback-forcing theory.

```{math}
:label: my_label82
\Delta N = \Delta F + \Delta R + V,
``` 
where $\Delta N$ is the net downward radiation imbalance at TOA, $\Delta F$ is the radiative forcing, and $\Delta R$ is the radiative response due to direct or indirect changes in temperature, and $V$ is the unrelated processes.

Assume that the radiative response $\Delta R$ is proportional to first order of the forced change in global mean surface air temperature ($\Delta T_s$, using Taylor expansion), we can get:

```{math}
:label: my_label83
\Delta N = \Delta F + \lambda\Delta T_{s} + V.
```

```{note}
What is the sign of $\lambda$? Can the system reach to an equilibrium state if $\lambda$ is positive?
```

Consider the climate system reaches an equilibrium state over a sufficient long time, so we can neglect $\Delta N$ and $V$.

```{math}
:label: my_label84
\Delta T_{s} = -\frac{\Delta F}{\lambda}.
```

So the climate sensitivity of doubling CO2 can be expressed as below:

```{math}
:label: my_label85
S_{2xCO2} = -\frac{\Delta F_{2xCO2}}{\lambda}
```

The feedback $\lambda$ can be decomposed into each feedback process:
```{math}
:label: my_label86
\lambda = \sum_{i}^{6?} {\lambda_{i}}
```

- Feebacks can change strength in different climate state --> state dependence $\Delta \lambda_{state}$
- $\Delta N$ can not only depend on global-mean surface air temperature but also the geographic distribution or pattern --> pattern effect $\Delta \lambda_{pattern}$

We can modify the model as:
```{math}
:label: my_label87
\Delta N = \Delta F + (\lambda - \Delta \lambda)\Delta T_{s},
```
where
```{math}
:label: my_label88
\Delta \lambda = \Delta \lambda_{state} + \Delta \lambda_{pattern} = \frac{\partial \lambda}{\partial T_{s}}\Delta T_{s} + \frac{\partial \lambda}{\partial T_{s}^{'}(x)}\Delta T_{s}^{'}(x)
```

How to calculate $\Delta \lambda_{state}$ and $\Delta \lambda_{pattern}$ is another long story.

## Radiative kernel analysis


## Homework assignment X (due xxx)
1. Please change adj_lapse_rate = 9.8. This is the dry adiabatic lapse rate. Calculate the water feedback parameter in this scenario.




