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
- The atmosphere aabsorbs all longwave radiation from the surface.
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
D_{s} = (1-\epsilon)(1-\epsilon)D_{2} + (1-\epsilon)D_{1} + \epsilon \sigma T_{1}^{4} = (1-\epsilon)\epsilon \sigma T_{2}^{4} + \epsilon \sigma T_{1}^{4}
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
\sigma T_{1}^{4}
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

## Homework assignment 2 (due xxx)

1. Given an observational vertical temperature profile ($T_s$=288 K, $T_1$=275 K, $T_2$=230 K, and OLR=238.5 $\mbox{W/m}^2$, could you find the value for $\epsilon$?

2. What is the radiative forcing $R$ in an isothermal atmosphere? Please use the two-layer model to support your guess.

3. Assume we add greenhouse gases into the atmopshere with observed temperatures, that is:$\Delta \epsilon = 0.02\times \epsilon$. What is the radiative forcing? Be sure you understand the sign.

## Final project 2

Could you construct a n-layer model, for n>4? Please discuss:

1. The upward and downward longwave beams, and the level of emission.

2. What are the atmospheric temperatures in each layer? And what is the sufrave temperature?

3. If you add greenhouse gases, what are the temperatures change?

4. What is the radiative forcing in each layer? How does it change when the emmisivity increases?
 

