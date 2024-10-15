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

(zero_dim_model)=

# Zero-dimensional Energy Balance Model

## Global energy budget
We start with the observed global energy budget, which is not too difficult to understand but is rather essential to many aspects of climate dynamics and modeling studies.

```{figure} /_static/lecture_specific/lecture1_figures/energy_budget000.png
:scale: 40%
```

For the shortwave:
- Incoming shortwave/solar radiation is 341 W/m$^2$. 
- Reflected shortwave radiation is 102 W/m$^2$.
- The global mean albedo is 102 m$^2$ / 341 m$^2$ = 0.299 ~= 0.3.
- Clouds absorption is 79 m$^2$, roughly the same as atmospheric absorption 78 m$^2$.
- Surface receives 161 m$^2$, and reflects 23 m$^2$ (why smaller than cloud reflection?).
- The roles of ozone (O$_3$) and water vapor (H$_{2}$O)?

For the longwave:
- Surface emits 396 m$^2$ (blackbody with temperture 288K?).
- Emission to space is only 239 m$^2$ (greenhouse effect).
- Atmospheric window allos 22 m$^2$ to the space directly.
- Atmosphere emits downward to the surface 333 m$^2$.

For others:
- Evapotranspiration emits 80 m$^2$ to the atmosphere (latent heat).
- Thermals process emits 17 m$^2$ to the atmosphere (sensible heat?).

Net radiation absorbed by the surface:
- 161 m$^2$ (shortwave) - 17 m$^2$ (sensible heat) - 80 (latent heat) - 396 (longwave) + 333 (longwave from the atmosphere) = 1 m$^2$ ~= 0.9 m$^2$

```{note}
Think about the roles of clouds and water vapor. 
```

## Calculate the energy budget
We assume a budget for an idealised global-mean atmosphere-ocean system:

```{math}
:label: my_label1
\frac{dE}{dt} = \mbox{energy flux into the system} - \mbox{energy flux out of the system}
```

- $E$ is the heat or energy content of the system, or called enthalpy. 
- We will consider the energy per surface area, therefore each term has units $\mbox{W/m}^2$. 
- While $E$ is the sum of internal energy in all reservoirs (e.g., atmosphere, ocean, land, cryosphere, biosphere), the internal exchange of energy betweem them cannot be present here.

```{note}
[Enthalpy](https://en.wikipedia.org/wiki/Enthalpy) (H) is the sum of a system's internal energy and the product of its pressure and volume:

$$
H = U + pV
$$

```

For the "energy flux into the system", we assume it to be the net incoming solar radiation at the top-of-atmosphere (TOA):

```{math}
:label: my_label2
Q = S_{0}\frac{A_{cross}}{A_{surface}} = \frac{S_{0}}{4} = 341.3 \mbox{W/m}^2
```

- $S_{0}$ is the solar constant: 1365.2 $\mbox{W/m}^2$.
- $A_{cross}$ is the area of illumination: $\pi r_{p}^2$
- $A_{surface}$ is surface area of the Earth: $4\pi r_{p}^2$
- $r_{p}$ is the Earth's radius: 6371 km

```{figure} /_static/lecture_specific/lecture1_figures/sphere_cross.png
:scale: 60%
Figure 2.2 from Dennis L. Hartmann's textbook.
```

For the "energy flux out of the system", there are two components:

1. The amount of incoming solar radiation will be reflected back to the space $\alpha Q$.
- $\alpha$ is the planetary albedo: reflected solar flux / incoming solar flux $\approx 0.3$.
- $\alpha Q = 341.3*0.3 = 101.9 \mbox{W/m}^2$.

2. The outgoing or terrestrial longwave radiative (OLR) flux.
- We assume the Earth behaves like a blackbody radiator with the emission temperature $T_e$.
- Following the Stefan-Bolzmann law: $\sigma T_{e}^4$
- Stefan-Boltzmann constant: $\sigma = 5.67\times 10^{-8} \mbox{W/m}^2/\mbox{K}^{4}$ 
- $T_{e} = \sqrt[^4]{\frac{238.5}{\sigma}} = ?$ You should get a very cold temperature...(what happens here?)

```{note}
We could try to solve this puzzling by including the greenhouse effect as:

$$
\mbox{OLR} = \tau \sigma T_{s}^{4}
$$

where $\tau$ is called the transmissivity of the atmosphere.

We can use the observed value to estimate $\tau$:

$$
\tau = \frac{\mbox{OLR}}{\sigma T_{s}^{4}} = \frac{238.5}{5.67\times 10^{-8}\cdot 288^4} \approx 0.61
$$

```

Now let's put everything together:

```{math}
:label: my_label3
\frac{dE}{dt} = \underbrace{Q}_{\mbox{energy flux into the system}} - \underbrace{(\alpha Q + \tau \sigma T_{s}^{4})}_{\mbox{energy flux out of the system}}
```

```{math}
:label: my_label4
\Rightarrow \frac{dE}{dt} = (Q-\alpha Q) - \tau \sigma T_{s}^{4} = (1-\alpha)Q - \tau \sigma T_{s}^{4}
```

We will call $(1-\alpha)Q$ the absorbed shorwave radiation (ASR).

## Equilibrium state
Assume that the Earth system is in energy balance, that is:

```{math}
:label: my_label5
\underbrace{Q}_{\mbox{energy flux into the system}} = \underbrace{(\alpha Q + \tau \sigma T_{s}^{4})}_{\mbox{energy flux out of the system}}
```

or

```{math}
:label: my_label6
\mbox{ASR} = \mbox{OLR} 
```

```{math}
:label: my_label7
\Rightarrow (1-\alpha)Q = \tau \sigma T_{s}^{4}
```

We would like to solve the surface temperature in this equilibrium state, so arrange the above equation to get:
```{math}
:label: my_label8
T_{eq} = \sqrt[^4]{\frac{(1-\alpha)Q}{\tau \sigma}}
```

Let's solve $T_{eq}$ as a function of $\alpha$ and $\tau$. 

I wrote a very bad (but easy to understand) Python code below to calcualte $T_{eq}$ with $\alpha$ and $\tau$ varied (can you improve it?):

```{code-cell} ipython3
import numpy as np

nx = 20
Q = 341.3 # W / m^2   
sigma = 5.67e-8 # W / m^2 / K^4
alpha = np.linspace(0,1,nx)
tau = np.linspace(0.1,1,nx)

Teq_obs = ((1-0.3)*Q/0.61/sigma)**0.25

Teq = np.zeros((nx,nx))
for JJ in range(nx):
    for II in range(nx):
        Teq[JJ,II] = ((1-alpha[JJ])*Q/tau[II]/sigma)**0.25
```

Can you make below figure?

```{figure} /_static/lecture_specific/lecture1_figures/Teq_test1.png
:scale: 60%
```



# not below





