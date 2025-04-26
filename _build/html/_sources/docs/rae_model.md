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

(rae_model)=

# Radiative Advective Equilibrium (RAE)

## Climate regimes
RCE model is powerful to understand the most important feature of atmospheric vertical thermal profile. However, outside the tropical region, the the pathway of energy balance can be different from the RCE paradigm. 

To maintain energy balance, three types of energy transfer can be considered:

- advection
- radiation
- surfce turbulent fluxes

We can see below figure from [Hartmann 2016](https://www.atmos.washington.edu/~dennis/gpc.html) that characterizes the way to reach energy balance depends on latitudes.

```{figure} /_static/lecture_specific/lecture1_figures/Fig6_1_hartmann.png
:scale: 50%
```

- in low latitudes, the radiative cooling is mostly balanced by surface turbulent fluxes (LE+SH). This corresponds to RCE (why?).
- in high latitudes, the radiative cooling is balanced by advection from lower latitudes. This corresponds to RAE.
- in midlatitudes, all three processes are important. We refer it to radiative convective advective equilibrium (RCAE).

So, the way how energy reaches balance is another representation of low, middle, and high latitude climates.

To quantify what we discuss above, we use the framework of vertically integrated, zonal-mean, annual-mean moist static energy (MSE) budget.
```{math}
:label: my_label89
<[\frac{\partial m}{\partial t}]> + <[\frac{\partial vm}{\partial y}]> = [R_{a}] + [LH] + [SH],
```
where m is the moist static energy (MSE), [$\cdot$] is zonal mean, <$\cdot$> is vertical integration, $R_a$ is the difference of TOA and surface radiative fluxes, $LH$ and $SH$ are latent and sensible heat fluxes respectively.


```{note}
Moist static energy [(MSE)](https://glossary.ametsoc.org/wiki/Moist_static_energy) can be defined as:
```{math}
:label: my_label90
m = c_{p}T + gz + Lq
```

For simplicity, we drop notations [$\cdot$] and <$\cdot$> and divide both side by the radiative fluxes to obtain a nondimensional equation, following [Miyawaki et al. (2022)](https://journals.ametsoc.org/view/journals/clim/35/3/JCLI-D-21-0440.1.xml):

```{math}
:label: my_label91
\underbrace{\frac{\partial_{t}m+\partial_{y}(vm)}{R_{a}}}_{R_{1}} = 1 + \underbrace{\frac{LH+SH}{R_{a}}}_{R_2},
```
where $R_1$ and $R_2$ are two key nondimensional variables.

- RCE regime: the $R_a$ is mostly balanced by $LE+SH$ $\rightarrow R_{2} \approx -1$ $\rightarrow R_1$ is small. ($R_{1}<0.1$)
- RAE regime: the $R_a$ is balanced by advection, so $R_2$ is small or $R_1$ is large . ($R_{1}>0.9$)
- RCAE regime: $R_1$ is moderate, but complicated!!! ($0.1<R_{1}<0.9$)

The figure from [Miyawaki et al. (2022)](https://journals.ametsoc.org/view/journals/clim/35/3/JCLI-D-21-0440.1.xml)below shows the $R_1$ as a function of latitude with climate regime colored.

```{figure} /_static/lecture_specific/lecture1_figures/miyawaki_2022_figure2a.png
:scale: 70%
```

We also see strong seasonality of $R_1$: 
```{figure} /_static/lecture_specific/lecture1_figures/miyawaki_2022_figure3a.png
:scale: 110%
```

## Two-layer RAE model

Let's play around with a simpler model to emulate RAE. [Payne et al. (2015)](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1002/2015GL065889) proposed a two-layer model to characterize RCE equilibrium. I found this model very intersting and informative. The model looks like below:

```{figure} /_static/lecture_specific/lecture1_figures/rae_two_layer.png
:scale: 50%
```

The surface represents 1000 hPa and the atmosphere represents 500 hPa. $F_{S}$ is the shorwave radiation absorbed by the surface, $F_A{}$ is the sum of atmospheric heat rransport and shortwave radiation absorbed by the atmosphere, $F_{C}$ is the convective heat transport.

Energy balance for 
- the surface: $F_{S} + \epsilon \sigma T_{a}^{4} = F_{C} + \sigma T_{s}^{4}$
- the atmosphere: $F_{A} + F_{C} + \epsilon \sigma T_{s}^{4} = 2\epsilon \sigma T_{a}^{4}$

To trigger the convection, we assume that $T_{s} - T_{a}$ have to exceed a critical lapse rate $\Delta_{T}$ defined as:
```{math}
:label: my_label92
\Delta_{T} = T_{s} - T_{a}
```

```{math}
:label: my_label93
\frac{d\Delta_{T}}{dT_{s}} = -0.6 \mbox{ K/K}
```

So we can relate $\Delta_T$ to $T_s$ or derive $\Delta_T$ as a function of $T_s$. Does this make sense? How does this relate to RCE?

In high-latitude, as discussed above, we assume convective heat transport is very weak, that is $F_{C}=0$. So the energy balance eqaution becomes:
```{math}
:label: my_label94
F_{S} + \epsilon \sigma T_{a}^{4} = \sigma T_{s}^{4}
```
```{math}
:label: my_label95
F_{A} + \epsilon \sigma T_{s}^{4} = 2\epsilon \sigma T_{a}^{4}
```

We can solve for $T_s$ and $T_a$:
```{math}
:label: my_label96
\sigma T_{s}^{4} = \frac{2F_{S}+F_{A}}{(2-\epsilon)}
```
```{math}
:label: my_label97
\sigma T_{a}^{4} = \frac{\epsilon F_{S} + F_{A}}{\epsilon(2-\epsilon)}
```



## Homework assignment X (due xxx)
1. In the pure radiative model, can you remove the effect of ozone?
2. In the pure radiative model, can you remove the effects of ozone and water vapor?
3. In the RCE model, chosse an initial condition with temperature reaching radiative equilibrium.
4. In the RCE model, chosse an isothermal initial condition with temperature 360 K and 170 K. Can you plot the temperature evolution at surface, 800 hPa, 500 hPa, 200 hPa, and 100 hPa, with time? 





