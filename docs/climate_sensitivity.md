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

(rce_model)=

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
ncep_url = "http://www.esrl.noaa.gov/psd/thredds/dodsC/Datasets/ncep.reanalysis.derived/"
time_coder = xr.coders.CFDatetimeCoder(use_cftime=True)
ncep_air = xr.open_dataset( ncep_url + "pressure/air.mon.1981-2010.ltm.nc", decode_times=time_coder)
#  Take global, annual average 
coslat = np.cos(np.deg2rad(ncep_air.lat))
weight = coslat / coslat.mean(dim='lat')
Tglobal = (ncep_air.air * weight).mean(dim=('lat','lon','time'))

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

# Get the water vapor data from CESM output
cesm_data_path = "http://thredds.atmos.albany.edu:8080/thredds/dodsC/CESMA/"
atm_control = xr.open_dataset(cesm_data_path + "cpl_1850_f19/concatenated/cpl_1850_f19.cam.h0.nc")
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

rcm

rcm.subprocess['Radiation'].absorber_vmr['CO2']

rcm.integrate_years(5)

rcm.ASR - rcm.OLR

```

```{code-cell} ipython3
xxx
```

## Radiative equilibrium model

Before we go into RCE model, we recap the important feature of considering only radiative process. We increase the complexity of radiative model, rather than just grey radiation, by using the [rapid radiative transfer model](http://rtweb.aer.com/rrtm_frame.html) in climlab.

We load the water vapor profile from CESM control simulation.

```{code-cell} ipython3
# Load the model output as we have done before
cesm_data_path = "/Users/yuchiaol_ntuas/Desktop/ebooks/local_ncu_climate_modelling/book_test2/docs/data/cpl_1850_f19.cam.h0.nc"
atm_control = xr.open_dataset(cesm_data_path)

# The specific humidity is stored in the variable called Q in this dataset:
print(atm_control.Q)
print(atm_control.gw)


# Take global, annual average of the specific humidity
weight_factor = atm_control.gw / atm_control.gw.mean(dim='lat')

Qglobal = (atm_control.Q * weight_factor).mean(dim=('lat','lon','time'))
# Take a look at what we just calculated ... it should be one-dimensional (vertical levels)
print(Qglobal)

fig, ax = plt.subplots()
#  Multiply Qglobal by 1000 to put in units of grams water vapor per kg of air
ax.plot(Qglobal*1000., Qglobal.lev)
ax.invert_yaxis()
ax.set_ylabel('Pressure (hPa)')
ax.set_xlabel('Specific humidity (g/kg)')
ax.grid()

```

Now we construct a single column model.

```{code-cell} ipython3
#  Make a model on same vertical domain as the GCM
mystate = climlab.column_state(lev=Qglobal.lev, water_depth=2.5)
print(mystate)

radmodel = climlab.radiation.RRTMG(name='Radiation (all gases)',  # give our model a name!
                              state=mystate,   # give our model an initial condition!
                              specific_humidity=Qglobal.values,  # tell the model how much water vapor there is
                              albedo = 0.25,  # this the SURFACE shortwave albedo
                              timestep = climlab.constants.seconds_per_day,  # set the timestep to one day (measured in seconds)
                             )

print(radmodel)

#  Here's the state dictionary we already created
print('========================================')
print('=======radmodel.state=======')
print(radmodel.state)

#  Here are the pressure levels in hPa
print('========================================')
print('=======radmodel.lev=======')
print(radmodel.lev)

print('========================================')
print('=======radmodel.absorber_vmr=======')
print(radmodel.absorber_vmr)


#  specific humidity in kg/kg, on the same pressure axis
print('========================================')
print('=======radmodel.specific_humidity=======')
print(radmodel.specific_humidity)

```

Let's have a look at the input parameters of RRTMG model, we can see a lot of parameters for cloud processes:
```{code-cell} ipython3
for item in radmodel.input:
    print(item)
```

Let's integrate the model one timestep:
```{code-cell} ipython3
print('========================================')
print('=======radmodel.Ts=======')
print(radmodel.Ts)

print('========================================')
print('=======radmodel.Tatm=======')
print(radmodel.Tatm)

# a single step forward
radmodel.step_forward()

print('========================================')
print('=======radmodel.Ts=======')
print(radmodel.Ts)

climlab.to_xarray(radmodel.diagnostics)

climlab.to_xarray(radmodel.LW_flux_up)

print('========================================')
print('=======radmodel.ASR - radmodel.OLR=======')
print(radmodel.ASR - radmodel.OLR)

```

The model does not reach equilibrium, right?

Now let's integrate the model to equilibrium:
```{code-cell} ipython3
while np.abs(radmodel.ASR - radmodel.OLR) > 0.01:
    radmodel.step_forward()

#  Check the energy budget again
print('========================================')
print('=======radmodel.ASR - radmodel.OLR=======')
print(radmodel.ASR - radmodel.OLR)

def add_profile(skew, model, linestyle='-', color=None):
    line = skew.plot(model.lev, model.Tatm - climlab.constants.tempCtoK,
             label=model.name, linewidth=2)[0]
    skew.plot(1000, model.Ts - climlab.constants.tempCtoK, 'o', 
              markersize=8, color=line.get_color())
    skew.ax.legend()

skew = make_skewT()
add_profile(skew, radmodel)
skew.ax.set_title('Pure radiative equilibrium', fontsize=18);

```

What do you think about the temperature profile compared to the observational one?

In Manabe's 1964 [paper](https://www.gfdl.noaa.gov/bibliography/related_files/sm6401.pdf):

```{figure} /_static/lecture_specific/lecture1_figures/manabe_1964_jump.png
:scale: 80%
```
 
## Remove water vapor
Let's remove water vapor:

```{code-cell} ipython3
# Make an exact clone of our existing model
radmodel_noH2O = climlab.process_like(radmodel)
radmodel_noH2O.name = 'Radiation (no H2O)'

print('========================================')
print('=======radmodel_noH2O=======')
print(radmodel_noH2O)

#  Here is the water vapor profile we started with
print('========================================')
print('=======radmodel_noH2O.specific_humidity=======')
print(radmodel_noH2O.specific_humidity)

radmodel_noH2O.specific_humidity *= 0.

print('========================================')
print('=======radmodel_noH2O.specific_humidity=======')
print(radmodel_noH2O.specific_humidity)

#  it's useful to take a single step first before starting the while loop
#   because the diagnostics won't get updated 
#  (and thus show the effects of removing water vapor)
#  until we take a step forward

radmodel_noH2O.step_forward()
while np.abs(radmodel_noH2O.ASR - radmodel_noH2O.OLR) > 0.01:
    radmodel_noH2O.step_forward()

print('========================================')
print('=======radmodel_noH2O.ASR - radmodel_noH2O.OLR=======')
print(radmodel_noH2O.ASR - radmodel_noH2O.OLR)

skew = make_skewT()
for model in [radmodel, radmodel_noH2O]:
    add_profile(skew, model)

```

We can remove ozone and ozone and water vapor combined. Can you make the same plot?

```{figure} /_static/lecture_specific/lecture1_figures/re_no_absorbers.png
:scale: 50%
```

## Radiative convective equilibrium model
The idea is to copule the convective model to the radiative model. If the lapse rate exceeds the critical lapse rate, then an instantaneous adjustment will be performed to mix temperatures to the critical lapse rate while conserving the energy.

The crtical lapse is generally chosen to be 6.5 K / km. Let's have a look at how Manabe discussed [the critical lapse rate](https://www.gfdl.noaa.gov/bibliography/related_files/sm6401.pdf):

```{figure} /_static/lecture_specific/lecture1_figures/manabe_critical_lapse_rate.png
:scale: 50%
```

This is how we parameterize it and construct our RCE model in climlab:
```{code-cell} ipython3
#  Make a model on same vertical domain as the GCM
mystate = climlab.column_state(lev=Qglobal.lev, water_depth=2.5)

#  Build the radiation model -- just like we already did
rad = climlab.radiation.RRTMG(name='Radiation (net)',
                              state=mystate, 
                              specific_humidity=Qglobal.values,
                              timestep = climlab.constants.seconds_per_day,
                              albedo = 0.25,  # surface albedo, tuned to give reasonable ASR for reference cloud-free model
                             )
#  Now create the convection model
conv = climlab.convection.ConvectiveAdjustment(name='Convection',
                                               state=mystate,
                                               adj_lapse_rate=6.5, # this is the key parameter! We'll discuss below
                                               timestep=rad.timestep,  # same timestep!
                                              )
#  Here is where we build the model by coupling together the two components
rcm = climlab.couple([rad, conv], name='Radiative-Convective Model')

print(rcm)

```

```{code-cell} ipython3
#  Some imports needed to make and display animations
from IPython.display import HTML
from matplotlib import animation

def get_tendencies(model):
    '''Pack all the subprocess tendencies into xarray.Datasets
    and convert to units of K / day'''
    tendencies_atm = xr.Dataset()
    tendencies_sfc = xr.Dataset()
    for name, proc, top_proc in climlab.utils.walk.walk_processes(model, topname='Total', topdown=False):
        tendencies_atm[name] = proc.tendencies['Tatm'].to_xarray()
        tendencies_sfc[name] = proc.tendencies['Ts'].to_xarray()
    for tend in [tendencies_atm, tendencies_sfc]:
        #  convert to K / day
        tend *= climlab.constants.seconds_per_day
    return tendencies_atm, tendencies_sfc

def initial_figure(model):
    fig = plt.figure(figsize=(14,6))
    lines = []
    
    skew = SkewT(fig, subplot=(1,2,1), rotation=30)
    #  plot the observations
    skew.plot(Tglobal.level, Tglobal, color='black', linestyle='-', linewidth=2, label='Observations')    
    lines.append(skew.plot(model.lev, model.Tatm - climlab.constants.tempCtoK, 
              linestyle='-', linewidth=2, color='C0', label='RC model (all gases)')[0])
    skew.ax.legend()
    skew.ax.set_ylim(1050, 10)
    skew.ax.set_xlim(-60, 75)
    # Add the relevant special lines
    skew.plot_dry_adiabats(linewidth=0.5)
    skew.plot_moist_adiabats(linewidth=0.5)
    skew.ax.set_xlabel('Temperature ($^\circ$C)', fontsize=14)
    skew.ax.set_ylabel('Pressure (hPa)', fontsize=14)
    lines.append(skew.plot(1000, model.Ts - climlab.constants.tempCtoK, 'o', 
                  markersize=8, color='C0', )[0])

    ax = fig.add_subplot(1,2,2, sharey=skew.ax)
    ax.set_ylim(1050, 10)
    ax.set_xlim(-8,8)
    ax.grid()
    ax.set_xlabel('Temperature tendency ($^\circ$C day$^{-1}$)', fontsize=14)

    color_cycle=['g','b','r','y','k']
    #color_cycle=['y', 'r', 'b', 'g', 'k']
    tendencies_atm, tendencies_sfc = get_tendencies(rcm)
    for i, name in enumerate(tendencies_atm.data_vars):
        lines.append(ax.plot(tendencies_atm[name], model.lev, label=name, color=color_cycle[i])[0])
    for i, name in enumerate(tendencies_sfc.data_vars):
        lines.append(ax.plot(tendencies_sfc[name], 1000, 'o', markersize=8, color=color_cycle[i])[0])
    ax.legend(loc='center right');
    lines.append(skew.ax.text(-100, 50, 'Day {}'.format(int(model.time['days_elapsed'])), fontsize=12)) 
    return fig, lines

def animate(day, model, lines):
    lines[0].set_xdata(np.array(model.Tatm)-climlab.constants.tempCtoK)
    lines[1].set_xdata(np.array(model.Ts)-climlab.constants.tempCtoK)
    #lines[2].set_xdata(np.array(model.q)*1E3)
    tendencies_atm, tendencies_sfc = get_tendencies(model)
    for i, name in enumerate(tendencies_atm.data_vars):
        lines[2+i].set_xdata(tendencies_atm[name])
    for i, name in enumerate(tendencies_sfc.data_vars):
        lines[2+5+i].set_xdata(tendencies_sfc[name])
    lines[-1].set_text('Day {}'.format(int(model.time['days_elapsed'])))
    # This is kind of a hack, but without it the initial frame doesn't appear
    if day != 0:
        model.step_forward()
    return lines

```

We use an isothermal initial condition:
```{code-cell} ipython3
#  Start from isothermal state
rcm.state.Tatm[:] = rcm.state.Ts

print('========================================')
print('=======rcm.state.Tatm=======')
print(rcm.state.Tatm)

#  Call the diagnostics once for initial plotting
rcm.compute_diagnostics()

#  Plot initial data
fig, lines = initial_figure(rcm)

```

Below we follow Professor Brian Rose to create animations to illustrate the adjustment of RCE.
```{code-cell} ipython3
#  This is where we make a loop over many timesteps and create an animation in the notebook
ani = animation.FuncAnimation(fig, animate, 150, fargs=(rcm, lines))
HTML(ani.to_html5_video())

```

If we start from radiative equilibrium state:
```{code-cell} ipython3
#  Here we take JUST THE RADIATION COMPONENT of the full model and run it out to (near) equilibrium
#  This is just to get the initial condition for our animation
for n in range(1000):
    rcm.subprocess['Radiation (net)'].step_forward()
#  Call the diagnostics once for initial plotting
rcm.compute_diagnostics()
#  Plot initial data
fig, lines = initial_figure(rcm)
```

```{code-cell} ipython3
#  This is where we make a loop over many timesteps and create an animation in the notebook
ani = animation.FuncAnimation(fig, animate, 100, fargs=(rcm, lines))
HTML(ani.to_html5_video())

```

```{note}
Convection processes are fast compared to radiative processes.
```

We now compare the results with those in radiative equilibrium.

```{code-cell} ipython3
#  Make a model on same vertical domain as the GCM
mystate = climlab.column_state(lev=Qglobal.lev, water_depth=2.5)

rad = climlab.radiation.RRTMG(name='all gases',
                              state=mystate, 
                              specific_humidity=Qglobal.values,
                              timestep = climlab.constants.seconds_per_day,
                              albedo = 0.25,  # tuned to give reasonable ASR for reference cloud-free model
                             )

#  remove ozone
rad_noO3 = climlab.process_like(rad)
rad_noO3.absorber_vmr['O3'] *= 0.
rad_noO3.name = 'no O3'

#  remove water vapor
rad_noH2O = climlab.process_like(rad)
rad_noH2O.specific_humidity *= 0.
rad_noH2O.name = 'no H2O'

#  remove both
rad_noO3_noH2O = climlab.process_like(rad_noO3)
rad_noO3_noH2O.specific_humidity *= 0.
rad_noO3_noH2O.name = 'no O3, no H2O'

#  put all models together in a list
rad_models = [rad, rad_noO3, rad_noH2O, rad_noO3_noH2O]

rc_models = []
for r in rad_models:
    newrad = climlab.process_like(r)
    conv = climlab.convection.ConvectiveAdjustment(name='Convective Adjustment',
                                               state=newrad.state,
                                               adj_lapse_rate=6.5,  # the key parameter in the convection model!
                                               timestep=newrad.timestep,)
    rc = newrad + conv
    rc.name = newrad.name
    rc_models.append(rc)

for model in rad_models:
    for n in range(100):
        model.step_forward()
    while (np.abs(model.ASR-model.OLR)>0.01):
        model.step_forward()    
    
for model in rc_models:
    for n in range(100):
        model.step_forward()
    while (np.abs(model.ASR-model.OLR)>0.01):
        model.step_forward()

skew = make_skewT()
for model in rad_models:
    add_profile(skew, model)
skew.ax.set_title('Pure radiative equilibrium', fontsize=18);

skew2 = make_skewT()
for model in rc_models:
    add_profile(skew2, model)
skew2.ax.set_title('Radiative-convective equilibrium', fontsize=18);


```

There are a lot to discuss. We could start with 1) the vertical structure, and 2) tropopause height.


## Homework assignment 3 (due xxx)
1. In the pure radiative model, can you remove the effect of ozone?
2. In the pure radiative model, can you remove the effects of ozone and water vapor?
3. In the RCE model, chosse an initial condition with temperature reaching radiative equilibrium.
4. In the RCE model, chosse an isothermal initial condition with temperature 360 K and 170 K. Can you plot the temperature evolution at surface, 800 hPa, 500 hPa, 200 hPa, and 100 hPa, with time? 

## Final project 4
The key parameter 'adj_lapse_rate' determines the RCE equilibrium, but setting it as a constant may be somewhat idealized. Can you replace it with:
- adj_lapse_rate = 9.8. This is the dry adiabatic lapse rate.
- adj_lapse_rate = 'pseudoadiabat'. This follows the blue moist adiabats on the skew-T diagrams.
- more realistic lapse-rate from observations? For example from sounding?





