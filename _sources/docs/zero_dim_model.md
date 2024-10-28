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

Below is the newer version of the global energy budget from Dennis L. Hartmann's [textbook](https://www.atmos.washington.edu/~dennis/gpc.html):
```{figure} /_static/lecture_specific/lecture1_figures/Fig-2-4.png
:scale: 40%
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
- We assume the Earth behaves like a blackbody radiator with the <span style="color:red">emission temperature $T_e$</span>.
- Following the [Stefan-Bolzmann law](https://en.wikipedia.org/wiki/Stefan%E2%80%93Boltzmann_law): $\sigma T_{e}^4$
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

## Time-dependent energy balance model
In the real word and for most climate modelling system, the energy-balance state of a climate can be broken if some disturbances or process changes in the system (e.g., greenhouse gases). The system is no longer in equilibrium, but it may adjust to a new equibrium state (if not, what will happen?). So we are now back to Equation (4).

We relate the enthalpy $E$ to the global-mean surface temperature $T_{s}$ with a constant $C$:
```{math}
:label: my_label9
E = CT_{s}
```
where $C$ is the effective heat capacity of atmosphere and ocean combined. 

```{note}
Assuming $C$ as a constant is somewhat tricky and highly-simplified. It is apparently not reasonable to treat the atmosphere, ocean, cryopshere together as one value $C$. However, to start with this zero-dimensional simple model, a constant $C$ may be suffient.

We can assume that most of the heat capacity comes from the upper ocean (reasonable, right?), then 

$$
C = c_{w}\rho_{w}H
$$

where
- $c_w$ is the specific heat of water = $4\times 10^{3}$ $\mbox{J/kg/K}$.
- $\rho$ is the density of water = $10^3$ $\mbox{kg/m}^3$.
- $H$ is the effective depth of sea water $\approx$ 100 m.

We will have the effective heat capacity $4.0\times 10^{8}$ $\mbox{J/m}^{2}\mbox{/K}$.

```

Then Equation (4) becomes:
```{math}
:label: my_label10
C\frac{dT_s}{dt} = (1-\alpha)Q - \tau \sigma T_{s}^{4}
```

Now we can solve this equation analytically!!! We adopt the concept of linear perturbation to the system:
```{math}
:label: my_label11
T_{s} = T_{eq} + T_{s}'\\ T_{s}' \ll T_{eq}
```

Using Taylor series expansion, we can approximate
```{math}
:label: my_label12
\tau \sigma T_{s}^{4} = \tau \sigma (T_{eq}+T_{s}')^{4} \approx \tau \sigma(T_{eq}^{4}+4T_{eq}^{3}T_{s}')
```

The energy balance equation can be then written as:
```{math}
:label: my_label13
\Rightarrow C\frac{d(T_{eq}+T_{s}')}{dt} = (1-\alpha)Q - \tau \sigma (T_{eq}+T_{s}')^{4} = (1-\alpha)Q - \tau \sigma (T_{eq}^{4}+4T_{eq}^{3}T_{s}')
```

Recall that $T_{eq}=\sqrt[4]{\frac{(1-\alpha)Q}{\tau \sigma}}$, such that:
```{math}
:label: my_label14
\Rightarrow C\frac{d(T_{eq}+T_{s}')}{dt} = (1-\alpha)Q - \tau \sigma (\frac{(1-\alpha)Q}{\tau \sigma}+4T_{eq}^{3}T_{s}') = -4\tau \sigma T_{eq}^{3}T_{s}'
```

We define a feedback parameter:
```{math}
:label: my_label15
\lambda_{0} = 4\tau \sigma T_{eq}^{3}
```
```{math}
:label: my_label16
\Rightarrow C\frac{dT_{s}'}{dt} = -\lambda_{0} T_{s}'
```

We next define a characteristic timescale:
```{math}
:label: my_label17
t^{*} \equiv \frac{C}{\lambda_{0}}
```

The the equation becomes:
```{math}
:label: my_label18
\frac{dT_{s}'}{dt} = - \frac{T_{s}'}{t^{*}} \Rightarrow \frac{dT_{s}'}{T_{s}'} = - \frac{dt}{t^{*}}
```

Take integral on both sides with a given initial condition $T_{s}'(0)$:
```{math}
:label: my_label19
\int_{T_{s}'(0)}^{T_{s}'(t)}\frac{dT_{s}'}{T_{s}'} = \int_{0}^{t}-\frac{dt}{t^{*}} 
```
```{math}
:label: my_label20
\Rightarrow \ln \frac{T_{s}'(t)}{T_{s}'(0)} = -\frac{t}{t^{*}}
```

```{math}
:label: my_label21
\Rightarrow T_{s}'(t) = T_{s}'(0)e^{-\frac{t}{t^{*}}}
```

Above are all mathematics, we now discuss the parameters and outcome.

1. The analytical solution:
```{figure} /_static/lecture_specific/lecture1_figures/T_solution.png
:scale: 90%
```

If we consider a global warming scenario, that is $\tau=0.57$ and $\alpha=0.32$, then we have the solutions:
```{figure} /_static/lecture_specific/lecture1_figures/T_solution2.png
:scale: 90%
```


2. The Planck feeback, $\lambda_{0}$:
- The strength of Planck feedback can be quantified by $\lambda_{0}$, which we usually call the Planck feedback parameter (more correctly -$\lambda_{0}$ to denote it is a negative feedback).

- If we use the code from Professor Brian Rose, we can get its value 3.31 W/m$^2$/K. This value does not vary too much in climate model simulations with different scenarios.
```{code-cell} ipython3
OLRobserved = 238.5  # in W/m2
sigma = 5.67E-8  # S-B constant
Tsobserved = 288.  # global average surface temperature
tau = OLRobserved / sigma / Tsobserved**4  # solve for tuned value of transmissivity

lambda_0 = 4 * sigma * tau * Tsobserved**3
#  This is an example of formatted text output in Python
print( 'lambda_0 = {:.2f} W m-2 K-1'.format(lambda_0)  )
```

- refer to as no-feedback climate response parameter.

- Since $\lambda_{0} = 4\tau \sigma T_{eq}^{3}$ and $T_{eq} = \sqrt[^4]{\frac{(1-\alpha)Q}{\tau \sigma}}$, we can make a plot for $\lambda_{0}$ as a function of $\tau$ and $\alpha$:
```{figure} /_static/lecture_specific/lecture1_figures/lambda_zero.png
:scale: 90%
``` 

3. The characteristic timescale $t^{*}$:
- $t^{*}$ depends on effective heat capacity and radiative feedback process.
- We can calculate its value by $\frac{C}{\lambda_{0}}=\frac{4.0\times10^{8}}{3.31}\approx 120845921$ second $\approx 3.8$ year.
- This means this climate system requires about 4 years to reach a quasi-equilibrium state based on the e-folding time criterion.
- not too long, right?
- We can make a plot to show its dependency on $H$ and $\lambda_{0}$:

```{figure} /_static/lecture_specific/lecture1_figures/T_star.png
:scale: 90%
```


## Homework assignment 1 (due xxx)
1. If the outgoing longwave radiation is 238.5 $\mbox{W/m}^2$ as observed and we purely consider the Earth as a blackbody, what is the corresponding surface air temperature $T_{s}$?

2. Can you make the contour plot for $T_{eq}$ as a function of $\alpha$ and $\tau$? If global warming occurs, what is the possible changes in $\alpha$ and $\tau$, and the resultant $T_{eq}$?

3. Can you use numerical method to solve Equation (10)? Please make a plot for the numerical solution together with the analytical solution included. Please also submit your code.

## Final project 1
When global warming occurs, high temperature could melt sea ice or glaciers that greatly reduce the albedo. In other words, the planetary albedo can be considered as a function of temperature, rather than a fixed value as we discuss above.

One widely-used simple albedo parameterization is:

```{math}
:label: my_label22
\alpha(T) = \alpha_{i}=0.7, \mbox{ when } T\le T_{i} = 260 K
```

```{math}
:label: my_label23
\alpha(T) = \alpha_{0} = 0.289, \mbox{ when } T\ge T_{0} = 293 K
```

```{math}
:label: my_label24
\alpha(T) = \alpha_{0} + (\alpha_{i}-\alpha_{0})\frac{(T-T_{0})^{2}}{(T_{i}-T_{0})^{2}}, \mbox{ when } T_{i}=260 K\lt T \lt T_{0}= 293 K
```

1. Discuss why we choose $\alpha_{i}=0.7$ and $T_{i}=260 K$. What does this condition in a climate system mean? 
2. Discuss why we choose $\alpha_{0} = 0.289$ and $T_{0}=293 K$. What does this condition in a climate system mean?
3. Can you calcuate the solution for $T_{eq}$ anallytically, when implementing $\alpha(T)$ in Equation (10)?
4. Can you calcualte the numerical solution for $T_{eq}$ anallytically, when implementing $\alpha(T)$ in Equation (10)?
5. Please find the relationship between $T_{eq}$ and $\alpha(T)$.
6. Feel free to add or discuss anything else?





