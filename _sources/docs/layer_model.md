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

(layer_model)=

# Layered Energy Balance Model

The zero-dimensional energy balance model discussed in previous lecture does not contain vertical distribution of the atmosphere, which is important in understanding climate change. We will introduce the simple two-layer model, simple three-layer model, and n-layer model. These models will pave a road for understanding Manabe's radiative-convective equilibrium that will be discussed in the next lecture. Before these, let's start with radiation process in the atmosphere.

## Atmospheric absorption spectra
The most important results, using [Planck's law](https://en.wikipedia.org/wiki/Planck%27s_law) from atmospheric radiation studies are probably can be shown in the figure from Dennis L. Hartmann's [textbook](https://www.atmos.washington.edu/~dennis/gpc.html) (his Figure 3.1):
 

```{figure} /_static/lecture_specific/lecture1_figures/Chapter-3-1.png
:scale: 10%
```

- We call longer wavelengths “infrared”, and shorter wavelengths “ultraviolet”.
- The Earth emission temperature is 255 K.
- The Sun emission temperature is 6000 K with peak wavelength $\lambda_{peak, Sun}$ 0.6 $\mu m$.
- We know that soloar spectrum peaks at 0.6 micrometers. By [Wien's displacement law](https://en.wikipedia.org/wiki/Wien%27s_displacement_law), we can get:

```{math}
:label: my_label25
T_{e}\times \lambda_{peak} = b \Rightarrow 6000\times 0.6 = 255\times \lambda_{peak, Earth} \Rightarrow \lambda_{peak, Earth} = 14 \mu m
```

- Let's quickly review the air gas composition looking at Hartmann's table1.1: 

```{figure} /_static/lecture_specific/lecture1_figures/gas_table1.png
:scale: 50%
```

- The abropstion of these gases, Figure 3.4 from Hartmann's text book:

```{figure} /_static/lecture_specific/lecture1_figures/Fig-3-4.png
:scale: 5%
```

- The molecular structure of these gases, Figure 3.3 from Hartmann's text book:

```{figure} /_static/lecture_specific/lecture1_figures/Fig-3-3.png
:scale: 40%
```


## Model with one-layer atmosphere and one-layer surface (one-layer model)

```{figure} /_static/lecture_specific/lecture1_figures/one_atm_one_surf.png
:scale: 40%
```

We assume:

- A single-layer atmosphere with temperature $T_{a}$.
- The atmosphere is transparent to shortwave radiation.
- The surface albedo is $\alpha$.
- The atmosphere absorbs all longwave radiation from the surface.
- The atmosphere and surface are blackbodies.
- The atmosphere radiates equally upward and downward.

We consider surface energy balance:

```{math}
:label: my_label26
(1-\alpha)Q + \sigma T_{a}^{4} = \sigma T_{s}^{4}
```

```{note}
back radiation: $\sigma T_{a}^{4}$
```

We consider atmosphere energy balance:
```{math}
:label: my_label27
\sigma T_{s}^{4} = 2\sigma T_{a}^{4}
```

So we can get the result that surface temperature is alway higher than the atmospheric temperature:
```{math}
:label: my_label28
T_{s} = 2^{0.25}T_{a} \approx 1.2T_{a}
```

If we assume $T_{a}$ = $T_{e}$ = 255 K (the emission temperature, how do we get this value?), we can $T_{s}$ = 303 K. In reality, the surface temperature is about 288 K, lower than 303 K. Why?

Let's play around with varying $Q$ and $\alpha$:

```{figure} /_static/lecture_specific/lecture1_figures/one_layer_surface_T.png
:scale: 90%
```

## Model with two-layer atmosphere and one-layer surface (two-layer model)

The surface temperature is too warm in the one-layer model. How could we improve the model to better capture the real surface air temperature? 

Many aspects can be considered (can you think about some?). We start by considering the vertical structure and longwave absorption by the atmosphere(s).

We keep most of the assumptions made for one-layer model, but modify the model as below:
- include one more atmospheric layer.
- each atmospehric layer absorbs a fraction of longwave radiation. This fraction is $\epsilon$ and we call it absorptivity.
- the reamaining $1-\epsilon$ readiation will pass through the layer.
- we adopt the [Kirchhoff's law of thermal radiation](https://en.wikipedia.org/wiki/Kirchhoff%27s_law_of_thermal_radiation): "For an arbitrary body emitting and absorbing thermal radiation in thermodynamic equilibrium, the emissivity function is equal to the absorptivity function.". That is absorptivity is equal to emissivity.
- consider the two longwave beams: upward (U) and downward (D) beams.

```{note}
This is a grey gas-type model. Grey meands that the emission and absorption do not depend on the wavelength.
```

Let's look at the model:

```{figure} /_static/lecture_specific/lecture1_figures/two_layer_surface_T.png
:scale: 40%
```

### Upward beam
```{math}
:label: my_label29
U_{s} = \sigma T_{s}^{4}
```

```{math}
:label: my_label30
U_{1} = (1-\epsilon)U_{s} + \epsilon \sigma T_{1}^{4} = (1-\epsilon)\sigma T_{s}^{4} + \epsilon \sigma T_{1}^{4}
```

```{math}
:label: my_label31
U_{2} &=& (1-\epsilon)U_{1} + \epsilon \sigma T_{2}^{4} = (1-\epsilon)((1-\epsilon)U_{s} + \epsilon \sigma T_{1}^{4}) + \epsilon \sigma T_{2}^{4} \\ 
&=& \underbrace{(1-\epsilon)^{2}\sigma T_{s}^{4}}_{\mbox{from surface}} + \underbrace{\epsilon (1-\epsilon) \sigma T_{1}^{4}}_{\mbox{from atmospheric layer 1}} + \underbrace{\epsilon \sigma T_{2}^{4}}_{\mbox{from atmospheric layer 2}}
```

### Downward beam
```{math}
:label: my_label32
D_{2} = 0
```

```{math}
:label: my_label33
D_{1} = (1-\epsilon)D_{2} + \epsilon \sigma T_{2}^{4} = \epsilon \sigma T_{2}^{4}
```

```{math}
:label: my_label34
D_{s} = (1-\epsilon)D_{1} + \epsilon \sigma T_{1}^{4} = (1-\epsilon)\epsilon \sigma T_{2}^{4} + \epsilon \sigma T_{1}^{4}
```


- If $\epsilon = 1$, the atmopsheres absorb everything:
```{math}
:label: my_label35
U_{s} = \sigma T_{s}^{4}\\
U_{1} = \sigma T_{1}^{4}\\
U_{2} = \sigma T_{2}^{4}
```

```{math}
:label: my_label36
D_{2} = 0 \\
D_{1} = \sigma T_{2}^{4}\\
D_{s} = \sigma T_{1}^{4}
```


- If $\epsilon = 0$, the atmopsheres absorb nothing:
```{math}
:label: my_label37
U_{s} = \sigma T_{s}^{4}\\
U_{1} = \sigma T_{s}^{4}\\
U_{2} = \sigma T_{s}^{4}
```

```{math}
:label: my_label38
D_{2} = 0\\
D_{1} = 0\\
D_{s} = 0
```

Let's solve temperatures considering an energy balance state!

### The atmospheric layer2
```{math}
:label: my_label39
U_{2} + D_{1} = D_{2} + U_{1}
```

```{math}
:label: my_label40
\Rightarrow (1-\epsilon)^{2}\sigma T_{s}^{4} + \epsilon (1-\epsilon)\sigma T_{1}^{4} + \epsilon \sigma T_{2}^{4} +  \epsilon \sigma T_{2}^{4} = 0 + (1-\epsilon)\sigma T_{s}^{4} + \epsilon \sigma T_{1}^{4}
```

```{math}
:label: my_label41
\Rightarrow [2\epsilon]\sigma T_{2}^{4}\\
+ [-\epsilon^{2}]\sigma T_{1}^{4}\\
+ [\epsilon^{2}-\epsilon]\sigma T_{s}^{4}\\
```

### The atmospheric layer1
```{math}
:label: my_label42
U_{1} + D_{s} = D_{1} + U_{s}
```

```{math}
:label: my_label43
\Rightarrow (1-\epsilon)\sigma T_{s}^{4} + \epsilon \sigma T_{1}^{4} + (1-\epsilon)\epsilon \sigma T_{2}^{4} + \epsilon \sigma T_{1}^{4} = \sigma T_{s}^{4} + \epsilon \sigma T_{2}^{4}
```

```{math}
:label: my_label44
\Rightarrow [-\epsilon^{2}]\sigma T_{2}^{4}\\
[2\epsilon]\sigma T_{1}^{4}\\
[-\epsilon]\sigma T_{s}^{4}
```

### The surface
```{math}
:label: my_label45
U_{s} + \alpha Q = D_{s} + Q
```

```{math}
:label: my_label46
\Rightarrow \sigma T_{s}^{4} - (1-\epsilon)\epsilon \sigma T_{2}^{4} - \epsilon \sigma T_{1}^{4} = (1- \alpha)Q
```

```{math}
:label: my_label47
\Rightarrow [\epsilon^{2}-\epsilon]\sigma T_{2}^{4}\\
[-\epsilon]\sigma T_{1}^{4}\\
[1]\sigma T_{s}^{4}
```

Now we can write the above equations in a matrix form:
```{math}
:label: my_label48
\underbrace{\begin{bmatrix}
2\epsilon & -\epsilon^{2} & \epsilon^{2} - \epsilon \\
\epsilon^{2} & -2\epsilon & \epsilon \\
\epsilon^{2}-\epsilon & \epsilon & 1 
\end{bmatrix}}_{A}
\begin{bmatrix}
\sigma T_{2}^{4}\\
\sigma T_{1}^{4}\\
\sigma T_{s}^{4}
\end{bmatrix} = 
\begin{bmatrix}
0\\
0\\
(1-\alpha)Q = \sigma T_{e}^{4}
\end{bmatrix}
```

It is a bit tedious to calculate the inverse of the matrix. We can use online tool [(here)](https://www.symbolab.com/solver/linear-algebra-calculator) to calculate it:
```{math}
:label: my_label49
A^{-1} = 
\begin{bmatrix}
-\frac{1}{\epsilon(\epsilon-2)} & -\frac{1}{-\epsilon+2} & \frac{1}{-\epsilon+2} \\
\frac{1}{-\epsilon+2} & \frac{2+\epsilon(\epsilon+1)^{2}}{\epsilon(\epsilon+2)(\epsilon=2)} & \frac{\epsilon+1}{-\epsilon+2}\\
\frac{1}{-\epsilon+2} &  -\frac{\epsilon+1}{-\epsilon+2}& \frac{\epsilon+2}{-\epsilon+2}  
\end{bmatrix}
```

So we can get:
```{math}
:label: my_label50
\begin{bmatrix}
T_{2}^4 \\
T_{1}^4 \\
T_{s}^4 
\end{bmatrix}=
\begin{bmatrix}
-\frac{T_{e}^4}{\epsilon-2}\\
\frac{-\epsilon T_{e}^4 - T_{e}^4}{\epsilon-2}\\
\frac{-\epsilon T_{e}^4 - 2T_{e}^4}{\epsilon-2}
\end{bmatrix}
\Rightarrow 
\begin{bmatrix}
T_{2} \\
T_{1} \\
T_{s} 
\end{bmatrix}=
\begin{bmatrix}
\sqrt[4]{\frac{1}{2-\epsilon}}T_{e}\\
\sqrt[4]{\frac{1+\epsilon}{2-\epsilon}}T_{e}\\
\sqrt[4]{\frac{2+\epsilon}{2-\epsilon}}T_{e}
\end{bmatrix}
```

Assume $\epsilon = 0.586$ (why?) and $T_{e}=255$ K (why?), we can have
```{math}
:label: my_label51
\begin{bmatrix}
T_{2} \\
T_{1} \\
T_{s}
\end{bmatrix}=
\begin{bmatrix}
233.8 \mbox{K} \\
262.3 \mbox{K} \\
296.4 \mbox{K} 
\end{bmatrix}
```

From observations:
```{math}
:label: my_label52
\begin{bmatrix}
T_{2} \\
T_{1} \\
T_{s}
\end{bmatrix}=
\begin{bmatrix}
230.0 \mbox{K} \\
275.0 \mbox{K} \\
288.0 \mbox{K}
\end{bmatrix}
```

```{note}
The radiative equilibrium solution for the two-layer model is substantially warmer at the surface and colder in the lower troposphere than the real atmosphere.
```

If we fix $T_{e} = 255$ K:
```{figure} /_static/lecture_specific/lecture1_figures/two_layer_T.png
:scale: 90%
```

If we fix $\epsilon=0.586$:
```{figure} /_static/lecture_specific/lecture1_figures/two_layer_T2.png
:scale: 90%
```

```{figure} /_static/lecture_specific/lecture1_figures/two_layer_T1.png
:scale: 90%
```

```{figure} /_static/lecture_specific/lecture1_figures/two_layer_TS.png
:scale: 90%
```

## Level of emission
From above results, we can decompose total OLR to the contributions from each layer:
```{math}
:label: my_label53
\begin{eqnarray}
OLR_{s} &=& (1-\epsilon)^{2}\sigma T_{s}^{4}\\
OLR_{1} &=& \epsilon(1-\epsilon)\sigma T_{1}^{4}\\
OLR_{2} &=& \epsilon\sigma T_{2}^{4}
\end{eqnarray}
```

If we assume $T_s$=288 K, $T_1$=275 K, $T_2$=230 K, and $\epsilon=0.586$, then we can get:
```{figure} /_static/lecture_specific/lecture1_figures/level_emission_T.png
:scale: 90%
```

## Radiative forcing
We define the radiative forcing ($F_{R}$) as the change in TOA radiative flux after adding absorbers (not being adjusted by other processes, such as feedbacks). In orther words, the radiative flux change due to changes in atmospheric composition.

```{math}
:label: my_label54
F_{R} = -\Delta OLR,
```
where negative value means radiative emission to the space, so the Earth is losing energy, vice versa.

In the two-layer model, the only component that can change due to adding absorbers is OLR. In a more realistic scenasio, the radiative forcing invovles with stratospheric adjustment. Here's a schematic from Hansen's 1997 classic [paper](https://agupubs.onlinelibrary.wiley.com/doi/10.1029/96JD03436):

```{figure} /_static/lecture_specific/lecture1_figures/hansen_1997_rf.png
:scale: 50%
```

- instantaneous radiative forcing
- adjusted radiative forcing
- effective radiative forcing (???)

Let's go back to our two-layer model with level of emission (Equation (53)). If we perturb the emmisivity:
```{math}
:label: my_label55
\epsilon \rightarrow \epsilon + \Delta \epsilon
```

The atmospheric temperatures are assumed unchanged, so we can derive:
```{math}
:label: my_label56
\begin{eqnarray}
OLR_{s} &=& (1-\epsilon-\Delta \epsilon)^{2}\sigma T_{s}^{4}\\
OLR_{1} &=& (\epsilon+\Delta \epsilon)(1-\epsilon-\Delta \epsilon)\sigma T_{1}^{4}\\
OLR_{2} &=& (\epsilon+\Delta \epsilon)\sigma T_{2}^{4}
\end{eqnarray}
```

We subtract each term from original value to get "changes" of OLR:
```{math}
:label: my_label57
\begin{eqnarray}
\Delta OLR_{s} &=& ((1-\epsilon-\Delta \epsilon)^{2} - (1-\epsilon)^{2})\sigma T_{s}^{4}\\
\Delta OLR_{1} &=& ((\epsilon+\Delta \epsilon)(1-\epsilon-\Delta \epsilon) - \epsilon(1-\epsilon))\sigma T_{1}^{4}\\
\Delta OLR_{2} &=& (\epsilon+\Delta \epsilon-\epsilon)\sigma T_{2}^{4}
\end{eqnarray}
```

```{math}
:label: my_label58
\Rightarrow
\begin{eqnarray}
\Delta OLR_{s} &=& ((-2(1-\epsilon)\Delta \epsilon + \Delta \epsilon^{2})\sigma T_{s}^{4}\\
\Delta OLR_{1} &=& (\Delta \epsilon - 2\epsilon\Delta \epsilon - \Delta \epsilon^{2})\sigma T_{1}^{4}\\
\Delta OLR_{2} &=& (\Delta \epsilon)\sigma T_{2}^{4}
\end{eqnarray}
```

If we ignore high-order term:
```{math}
:label: my_label59
\Rightarrow
\begin{eqnarray}
\Delta OLR_{s} &=& ((-2(1-\epsilon)\Delta \epsilon)\sigma T_{s}^{4}\\
\Delta OLR_{1} &=& (\Delta \epsilon - 2\epsilon\Delta \epsilon)\sigma T_{1}^{4}\\
\Delta OLR_{2} &=& (\Delta \epsilon)\sigma T_{2}^{4}
\end{eqnarray}
```

What do these results mean?

The radiative forcing then becomes:
```{math}
:label: my_label60
\begin{eqnarray}
F_{R} &=& -\Delta OLR_{s} - \Delta OLR_{1} - \Delta OLR_{2} \\
&=&-\Delta \epsilon (-2(1-\epsilon)\sigma T_{s}^{4} + (1-2\epsilon)\sigma T_{1}^{4} + \sigma T_{2}^{4})
\end{eqnarray}
```

Will $F_R$ be negative or positive? It is complicated and depending on the vertical temperature profile.

If we fix temepratures from observations:
```{figure} /_static/lecture_specific/lecture1_figures/level_demission.png
:scale: 90%
```

If we include nonlinear terms:
```{figure} /_static/lecture_specific/lecture1_figures/level_demission_nonlinear.png
:scale: 90%
```

Below is the intantaneous radiative forcing estimated by a rapid radiative transfer model (RRTM) from Previdi et al. (2020):
```{figure} /_static/lecture_specific/lecture1_figures/instant_rf.png
:scale: 100%
```

## 30-layer model
We can construct a 30-layer model by hand, but it could be challenging and not feasible. Therefore, it would be useful, and sometimes helpful, to konw [climlab](https://climlab.readthedocs.io/en/latest/), developed by Professor Brian E. J. Rose. Let's start with a few examples: [here](https://brian-rose.github.io/ClimateLaboratoryBook/courseware/grey-radiation-climlab.html). We will see how to construct a 30-layer model, which seems not feasible for solving the equations analytically.


```{note}
You can find more materials and notes prepared by Professor Brian E. J. Rose: [here](https://brian-rose.github.io/ClimateLaboratoryBook/home.html). They are awesome!!!
```

We now follow an example from Professor Rose's notes to demonstrate the results of 30-layer grey-gas model.

We first look at the observed temperature profile, which we need to specify later in the 30-layer model. 

```{code-cell} ipython3
import numpy as np
import matplotlib.pyplot as plt
import xarray as xr
import climlab
from scipy.optimize import brentq

plt.style.use('dark_background')

# read data from ncep reanalysis data
url = 'http://apdrc.soest.hawaii.edu:80/dods/public_data/Reanalysis_Data/NCEP/NCEP/clima/pressure/air'
air = xr.open_dataset(url)
# The name of the vertical axis is different than the NOAA ESRL version..
ncep_air = air.rename({'lev': 'level'})
print('ncep_air')
print(ncep_air)

#  Take global, annual average and convert to Kelvin
weight = np.cos(np.deg2rad(ncep_air.lat)) / np.cos(np.deg2rad(ncep_air.lat)).mean(dim='lat')
Tglobal = (ncep_air.air * weight).mean(dim=('lat','lon','time'))
Tglobal += climlab.constants.tempCtoK
print('Tglobal')
print(Tglobal)


#  A handy re-usable routine for making a plot of the temperature profiles
#  We will plot temperatures with respect to log(pressure) to get a height-like coordinate

def zstar(lev):
    return -np.log(lev / climlab.constants.ps)

def plot_soundings(result_list, name_list, plot_obs=True, fixed_range=True):
    color_cycle=['r', 'g', 'b', 'y']
    # col is either a column model object or a list of column model objects
    #if isinstance(state_list, climlab.Process):
    #    # make a list with a single item
    #    collist = [collist]
    fig, ax = plt.subplots(figsize=(9,9))
    if plot_obs:
        ax.plot(Tglobal, zstar(Tglobal.level), color='w', label='Observed')    
    for i, state in enumerate(result_list):
        Tatm = state['Tatm']
        lev = Tatm.domain.axes['lev'].points
        Ts = state['Ts']
        ax.plot(Tatm, zstar(lev), color=color_cycle[i], label=name_list[i])
        ax.plot(Ts, 0, 'o', markersize=12, color=color_cycle[i])
    #ax.invert_yaxis()
    yticks = np.array([1000., 750., 500., 250., 100., 50., 20., 10., 5.])
    ax.set_yticks(-np.log(yticks/1000.))
    ax.set_yticklabels(yticks)
    ax.set_xlabel('Temperature (K)', fontsize=14)
    ax.set_ylabel('Pressure (hPa)', fontsize=14)
    ax.grid()
    ax.legend()
    if fixed_range:
        ax.set_xlim([200, 300])
        ax.set_ylim(zstar(np.array([1000., 5.])))
    #ax2 = ax.twinx()
    
    return ax

plot_soundings([],[] );

```

Now we construct our 30-layer model using the observed temperature profile.


```{code-cell} ipython3
#  initialize a grey radiation model with 30 levels
col = climlab.GreyRadiationModel()
print('col')
print(col)

# interpolate to 30 evenly spaced pressure levels
lev = col.lev
Tinterp = np.interp(lev, np.flipud(Tglobal.level), np.flipud(Tglobal))
print('Tinterp')
print(Tinterp)
#  Need to 'flipud' because the interpolation routine 
#  needs the pressure data to be in increasing order

# Initialize model with observed temperatures
col.Ts[:] = Tglobal[0]
col.Tatm[:] = Tinterp

# This should look just like the observations
result_list = [col.state]
name_list = ['Observed, interpolated']
plot_soundings(result_list, name_list);

col.compute_diagnostics()
print('col.OLR')
print(col.OLR)

# Need to tune absorptivity to get OLR = 238.5
epsarray = np.linspace(0.01, 0.1, 100)
OLRarray = np.zeros_like(epsarray)

for i in range(epsarray.size):
    col.subprocess['LW'].absorptivity = epsarray[i]
    col.compute_diagnostics()
    OLRarray[i] = col.OLR

def OLRanom(eps):
    col.subprocess['LW'].absorptivity = eps
    col.compute_diagnostics()
    return col.OLR - 238.5

# Use numerical root-finding to get the equilibria
# brentq is a root-finding function
#  Need to give it a function and two end-points
#  It will look for a zero of the function between those end-points
eps = brentq(OLRanom, 0.01, 0.1)
print('eps')
print(eps)

col.subprocess.LW.absorptivity = eps
print('col.subprocess.LW.absorptivity')
print(col.subprocess.LW.absorptivity)

col.compute_diagnostics()
print('col.OLR')
print(col.OLR)

```

We now calculate the radiative forcing.

```{code-cell} ipython3
#  clone our model using a built-in climlab function
col2 = climlab.process_like(col)
print('col2')
print(col2)

col2.subprocess['LW'].absorptivity *= 1.02
print(col2.subprocess['LW'].absorptivity)

#  Radiative forcing by definition is the change in TOA radiative flux,
# HOLDING THE TEMPERATURES FIXED.
print('col2.Ts - col.Ts')
print(col2.Ts - col.Ts)

col2.compute_diagnostics()
print('col2.OLR')
print(col2.OLR)

RF = -(col2.OLR - col.OLR)
print( 'The radiative forcing is %.2f W/m2.' %RF)

```

Finally, we calculate the radiative equilibrium state:
```{code-cell} ipython3
re = climlab.process_like(col)
#  To get to equilibrium, we just time-step the model forward long enough
re.integrate_years(1.)

#  Check for energy balance
print( 'The net downward radiative flux at TOA is %.4f W/m2.' %(re.ASR - re.OLR))

result_list.append(re.state)
name_list.append('Radiative equilibrium (grey gas)')
plot_soundings(result_list, name_list)

```

What do you find?


## Homework assignment 2 (due xxx)

1. Given an observational vertical temperature profile ($T_s$=288 K, $T_1$=275 K, $T_2$=230 K, and OLR=238.5 $\mbox{W/m}^2$, could you find the value for $\epsilon$?

2. What is the radiative forcing $R$ in an isothermal atmosphere? Please use the two-layer model to support your guess.

3. Assume we add greenhouse gases into the atmopshere with observed temperatures, that is:$\Delta \epsilon = 0.02\times \epsilon$. What is the radiative forcing? Be sure you understand the sign.

4. Can you add convective adjustment and ozone into your 30-layer model?


## Final project 2

Could you construct a n-layer model, for n>4? Please discuss:

1. The upward and downward longwave beams, and the level of emission.

2. What are the atmospheric temperatures in each layer? And what is the sufrave temperature?

3. If you add greenhouse gases, what are the temperatures change?

4. What is the radiative forcing in each layer? How does it change when the emmisivity increases?
 
## Final project 3
Could you include seasonal and latitudinal variation of ozone in your 30-layer model? Please discuss the aspects discussed above.






