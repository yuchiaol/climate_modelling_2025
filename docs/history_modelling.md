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

(history_modelling)=

# The History of Climate Modelling

In this lecture, we briefly go over the history of climate modelling. I follow the timeline organized by [a CarbonBrief online article](https://www.carbonbrief.org/timeline-history-climate-modelling/)
and use materials written by [Dr. Yi-Hsuan Chen](https://pb.ps-taiwan.org/modules/news/article.php?storyid=648) and [Dr. Yen-Ting Hwang](https://pb.ps-taiwan.org/modules/news/article.php?storyid=647). 

## What is climate modelling?
Ask ChatGPT:

> Climate modeling is the use of mathematical models to simulate and understand the Earth's climate system. These models are built using equations that represent the physical, chemical, and biological processes that govern climate dynamics. By incorporating key elements such as the atmosphere, oceans, land surface, ice, and human influences, climate models help scientists study past, present, and future climate conditions.

> Key Components of Climate Models 
> 1. Atmospheric Dynamics: Movements of air, heat, and moisture through processes like wind, convection, and cloud formation.
> 2. Ocean Dynamics: Currents, heat exchange, and the movement of salinity within the ocean.
> 3. Land Surface: The effects of vegetation, soil moisture, and albedo (reflectivity) on climate.
> 4. Cryosphere: The role of ice sheets, glaciers, and sea ice in reflecting sunlight and regulating heat.
> 5. Radiative Transfer: The movement of solar and terrestrial radiation, influencing temperature and weather patterns.
> 6. Greenhouse Gas Emissions and Aerosols: The role of CO2, methane, and other pollutants in warming or cooling the atmosphere.

> Types of Climate Models
> 1. Simple Climate Models: Often used for specific questions, these models simplify the system, focusing on just a few elements like temperature and carbon cycle dynamics.
> 2. General Circulation Models (GCMs): Complex three-dimensional models that simulate the entire climate system, including interactions between the atmosphere, oceans, land surface, and ice.
> 3. Earth System Models (ESMs): Advanced GCMs that also include processes like the carbon cycle, biogeochemistry, and the role of vegetation and ecosystems.

> Purposes of Climate Modeling
> 1. Understanding Climate Change: Models help predict how the Earth's climate will respond to increasing greenhouse gas concentrations, enabling scientists to project future temperature rise, sea-level changes, and extreme weather events.
> 2. Paleoclimate Studies: By modeling past climates, scientists can understand how the Earth’s climate has changed over millennia and how it might behave in the future.
> 3. Scenario Planning: Climate models are used to test different policy scenarios, such as reducing carbon emissions, and their potential impacts on global and regional climates.
> 4. Short-Term Forecasting: Seasonal and decadal climate models can predict patterns like El Niño, monsoons, or droughts.

> Limitations of Climate Models
> 1. Resolution: Models often have a coarse resolution, making it difficult to simulate fine-scale processes like local weather patterns or regional topography.
> 2. Uncertainty: There is inherent uncertainty in projections due to unknown future human activities, natural variability, and incomplete understanding of certain climate processes.
> 3. Complex Interactions: The climate system is highly complex, and not all interactions—especially those involving clouds, aerosols, and ocean circulation—are fully understood.

```{note}
George E. P. Box (statistician): Essentially, all models are wrong, but some are useful.
```

## 1896 - Svante August Arrhenius 

- Wikipedia link: [here](https://en.wikipedia.org/wiki/Svante_Arrhenius)
- On the Influence of Carbonic Acid in the Air upon the Temperature of the Ground ([here](https://www.rsc.org/images/Arrhenius1896_tcm18-173546.pdf))
- Fourier (1827)
- Pouillet (1838)
- Tyndail (1865)
- Langley (1884, 1890)

In Particular, Arrhenius' Table VII impressed me most becasue it shows polar amplified temperature increase:

```{figure} /_static/lecture_specific/lecture1_figures/arrhenius_table.png
:scale: 85%
```


## 1904 - Vilhelm Bjerknes

## 1922 - Lewis Fry Richardson 



## NumPy

Fundamental matrix and array processing capabilities are provided by the
excellent [NumPy](http://www.numpy.org/) library.

For example, let\'s build some arrays

```{code-cell} ipython3
import numpy as np                     # Load the library

a = np.linspace(-np.pi, np.pi, 100)    # Create even grid from -π to π
b = np.cos(a)                          # Apply cosine to each element of a
c = np.sin(a)                          # Apply sin to each element of a
```

Now let\'s take the inner product

```{code-cell} ipython3
b @ c
```

The number you see here might vary slightly due to floating point arithmetic
but it\'s essentially zero.

As with other standard NumPy operations, this inner product calls into highly
optimized machine code.

It is as efficient as carefully hand-coded FORTRAN or C.




## SciPy

The [SciPy](http://www.scipy.org) library is built on top of NumPy and
provides additional functionality.

(tuple_unpacking_example)=
For example, let\'s calculate $\int_{-2}^2 \phi(z) dz$ where $\phi$ is
the standard normal density.

```{code-cell} ipython3
from scipy.stats import norm
from scipy.integrate import quad

ϕ = norm()
value, error = quad(ϕ.pdf, -2, 2)  # Integrate using Gaussian quadrature
value
```

SciPy includes many of the standard routines used in

-   [linear algebra](http://docs.scipy.org/doc/scipy/reference/linalg.html)
-   [integration](http://docs.scipy.org/doc/scipy/reference/integrate.html)
-   [interpolation](http://docs.scipy.org/doc/scipy/reference/interpolate.html)
-   [optimization](http://docs.scipy.org/doc/scipy/reference/optimize.html)
-   [distributions and random number generation](http://docs.scipy.org/doc/scipy/reference/stats.html)
-   [signal processing](http://docs.scipy.org/doc/scipy/reference/signal.html)

See them all [here](http://docs.scipy.org/doc/scipy/reference/index.html).

## Graphics

The most popular and comprehensive Python library for creating figures
and graphs is [Matplotlib](http://matplotlib.org/), with functionality
including

-   plots, histograms, contour images, 3D graphs, bar charts etc.
-   output in many formats (PDF, PNG, EPS, etc.)
-   LaTeX integration

Example 2D plot with embedded LaTeX annotations

```{figure} /_static/lecture_specific/about_py/qs.png
:scale: 55%
```

Example contour plot

```{figure} /_static/lecture_specific/about_py/bn_density1.png
:scale: 55%
```

Example 3D plot

```{figure} /_static/lecture_specific/about_py/career_vf.png
:scale: 80%
```

More examples can be found in the [Matplotlib thumbnail
gallery](http://matplotlib.org/gallery.html).

Other graphics libraries include

-   [Plotly](https://plot.ly/python/)
-   [Bokeh](http://bokeh.pydata.org/en/latest/)

## Symbolic Algebra

It\'s useful to be able to manipulate symbolic expressions, as in
Mathematica or Maple.

The [SymPy](http://www.sympy.org/) library provides this functionality
from within the Python shell.

```{code-cell} ipython3
from sympy import Symbol

x, y = Symbol('x'), Symbol('y')  # Treat 'x' and 'y' as algebraic symbols
x + x + x + y
```

We can manipulate expressions

```{code-cell} ipython3
expression = (x + y)**2
expression.expand()
```

solve polynomials

```{code-cell} ipython3
from sympy import solve

solve(x**2 + x + 2)
```

and calculate limits, derivatives and integrals

```{code-cell} ipython3
from sympy import limit, sin, diff

limit(1 / x, x, 0)
```

```{code-cell} ipython3
limit(sin(x) / x, x, 0)
```

```{code-cell} ipython3
diff(sin(x), x)
```

The beauty of importing this functionality into Python is that we are
working within a fully fledged programming language.

We can easily create tables of derivatives, generate LaTeX output, add
that output to figures and so on.

## Pandas

One of the most popular libraries for working with data is
[pandas](http://pandas.pydata.org/).

Pandas is fast, efficient, flexible and well designed.

Here\'s a simple example, using some dummy data generated with Numpy\'s
excellent `random` functionality.

```{code-cell} ipython3
import pandas as pd
np.random.seed(1234)

data = np.random.randn(5, 2)  # 5x2 matrix of N(0, 1) random draws
dates = pd.date_range('28/12/2010', periods=5)

df = pd.DataFrame(data, columns=('price', 'weight'), index=dates)
print(df)
```

```{code-cell} ipython3
df.mean()
```

## Further Reading


These lectures were originally taken from a longer and more complete lecture
series on Python programming hosted by [QuantEcon](https://quantecon.org).

The [full set of lectures](https://python-programming.quantecon.org/) might be
useful as the next step of your study.
