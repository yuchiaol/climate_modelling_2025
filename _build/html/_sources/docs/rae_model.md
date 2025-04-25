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



## Homework assignment X (due xxx)
1. In the pure radiative model, can you remove the effect of ozone?
2. In the pure radiative model, can you remove the effects of ozone and water vapor?
3. In the RCE model, chosse an initial condition with temperature reaching radiative equilibrium.
4. In the RCE model, chosse an isothermal initial condition with temperature 360 K and 170 K. Can you plot the temperature evolution at surface, 800 hPa, 500 hPa, 200 hPa, and 100 hPa, with time? 





