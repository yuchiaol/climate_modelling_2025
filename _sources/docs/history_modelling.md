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

# The History of Climate Modeling

In this lecture, we briefly go over the history of climate modeling. I follow the timeline organized by [a CarbonBrief online article](https://www.carbonbrief.org/timeline-history-climate-modelling/)
and use materials from articles written by [Dr. Yi-Hsuan Chen](https://pb.ps-taiwan.org/modules/news/article.php?storyid=648) and [Dr. Yen-Ting Hwang](https://pb.ps-taiwan.org/modules/news/article.php?storyid=647). 

## What is climate modeling?
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

A minimal definition of a climate model by Professor Brian E. J. Rose:
> A representation of the exchange of energy between the Earth system and space, and its effects on average surface temperature.

What is the average? And the planetary energy budget is the to all climate modeling.


## 1896 - Svante August Arrhenius 

- Wikipedia link: [here](https://en.wikipedia.org/wiki/Svante_Arrhenius)
- On the Influence of Carbonic Acid in the Air upon the Temperature of the Ground ([here](https://www.rsc.org/images/Arrhenius1896_tcm18-173546.pdf))
- Fourier (1827)
- Pouillet (1838)
- Tyndail (1865)
- Langley (1884, 1890)

In particular, Arrhenius' Table VII impressed me most becasue it shows polar amplified temperature increase:

```{figure} /_static/lecture_specific/lecture1_figures/arrhenius_table.png
:scale: 85%
```

## 1904 - Vilhelm Bjerknes
- Wikipedia link: [here](https://en.wikipedia.org/wiki/Vilhelm_Bjerknes)
- [Das Problem der Wettervorhersage, betrachtet von Standpunkt der Mechanik und Physik](https://books.google.com.tw/books/about/Das_Problem_der_Wettervorhersage_betrach.html?id=Pib0HAAACAAJ&redir_esc=y)
- “Atmospheric changes could be calculated from a set of seven “primitive equations”. CarbonBrief
- Ask ChatGPT:
```{figure} /_static/lecture_specific/lecture1_figures/chatgpt_preimitive_eqns.png
:scale: 50%
```
- [Jacob Bjerknes](https://en.wikipedia.org/wiki/Jacob_Bjerknes) - ENSO


## 1922 - Lewis Fry Richardson 
- Began climate modelling using numerical methods.
- [Weather Prediction by Numerical Process](https://archive.org/details/weatherpredictio00richrich/weatherpredictio00richrich/page/n7/mode/2up?view=theater)
- It took him six weeks to produce an eight-hour forecast.
- “Weather Forecasting Factory” by Stephen Conlin (1986, [here](https://www.emetsoc.org/resources/rff/)):
```{figure} /_static/lecture_specific/lecture1_figures/weather_factory.png
:scale: 75%
```

## 1938 - Guy Callendar
- He used a 1D radiative transfer model to show warming by rising CO2 levels.
- He did all the calculations by hand.
- [The artificial production of carbon dioxide and its influence on temperature](https://www.met.reading.ac.uk/~ed/callendar_1938.pdf)
- A later paper (Hawkins and Jones, 2013) reproduced Guy Callendar's results and Ed Hawkins' [article](https://www.climate-lab-book.ac.uk/2013/75-years-after-callendar/):
```{figure} /_static/lecture_specific/lecture1_figures/rising_temperature.png
:scale: 95%
```

## 1946 - John von Neumann
- He proposed that new computers, such as the ENIAC at the University of Pennsylvania, can be used to forecast weather. 

## 1950 - ENIAC
- [Electronic Numerical Integrator and Computer](https://en.wikipedia.org/wiki/ENIAC)
- completed in 1945, put to work on December 10, 1945, and retired at 11:45 p.m. on October 2, 1955
```{figure} /_static/lecture_specific/lecture1_figures/eniac_women.png
:scale: 50%
```

## 1953 - Gilbert Plass 
- The warming influence of human-caused CO2 emissions
- Time magazine: [here](https://time.com/archive/6825643/science-invisible-blanket/)

## 1954 - JNWPU and BESK
- Joint Numerical Weather Prediction Unit (JNWPU) July 1, 1954 (Jule Gregory Charney?)
- Binary Electronic Sequence Calculator (BESK) - December, 1954 (Carl-Gustav Rossby)

## 1955 - Joseph Smagorinsky
- General Circulation Research Section under both von Neumann and Charney.
- to create a 3D general circulation model (GCM) of the global atmosphere based on “primitive equations”.
- the General Circulation Research Laboratory in 1959.
- the Geophysical Fluid Dynamics Laboratory (GFDL) in 1963.
- a paper later by Norman Phillips 1956 - the first GCM: [here](https://empslocal.ex.ac.uk/people/staff/gv219/classics.d/Phillips56.pdf) and [here](https://celebrating200years.noaa.gov/breakthroughs/climate_model/welcome.html#model), a 2-layer, hemispheric, quasi-geostrophic computer model.

## 1956 - Mikhail Budyko 
- [The Heat Balance of the Earth’s Surface](https://www.cia.gov/readingroom/docs/CIA-RDP81-01043R002500010003-6.pdf)
- He used a simple energy-balance model, and calculated the Earth’s average global temperature by balancing incoming solar energy with outgoing thermal energy.

```{figure} /_static/lecture_specific/lecture1_figures/budyko_plot.png
:scale: 50%
```
## 1956 - Syukuro Manabe
- Smagorinsky invited Syukuro Manabe from the University of Tokyo to join his lab at the US Weather Bureau.
- They work together to gradually add complexity to the models.

## 1956 - Norman Phillips 
- a paper by Norman Phillips 1956 - the first GCM: [here](https://empslocal.ex.ac.uk/people/staff/gv219/classics.d/Phillips56.pdf) and [here](https://celebrating200years.noaa.gov/breakthroughs/climate_model/welcome.html#model), a 2-layer, hemispheric, quasi-geostrophic computer model.

## 1963 - Fritz Möller 
- [On the Influence of Changes in the CO2 Concentration in Air on the Radiation Balance of the Earth's Surface and on the Climate](https://www.patarnott.com/SimpleScience/docs/MOLLER_1963_CO2_NotForcingClimate.pdf)
- To disprove Gilbert Plass’s influential 1953 paper on the warming influence of human-caused CO2 emissions.
- He ignored certain physical processes such as atmospheric convection, but encouraged other researchers (Manabe and Wetherald in 1967) to develop more reliable climate models.

## 1964 - Akio Arakawa 
- [Computational design for long-term numerical integration of the equations of fluid motion: Two-dimensional incompressible flow. Part I](https://people.bath.ac.uk/ensdasr/PAPERS/arakawa_1966.pdf)
- Mintz-Arakawa Model can run longer!!!
- Additional adevection term to represent grid-scale subgrid-scale interactions.

## 1964 - NCAR
- Warren Washington and Akira Kasahara 
- NCAR as a leading climate modelling centre from the 1960s onwards.

## 1966 - National Academy of Science
- [Weather and Climate Modification: Problems and Prospects](https://nap.nationalacademies.org/read/21330/chapter/1#iii)
- the modellers need more powerful, faster computers

## 1967 - Kirk Bryan 
- the first ocean general circulation model
- [A numerical investigation of the oceanic general circulation](https://onlinelibrary.wiley.com/doi/abs/10.1111/j.2153-3490.1967.tb01459.x)

## 1967 - Manabe and Wetherald
```{figure} /_static/lecture_specific/lecture1_figures/mw_1976.png
:scale: 50%
```
- [Thermal Equilibrium of the Atmosphere with a Given Distribution of Relative Humidity](https://journals.ametsoc.org/view/journals/atsc/24/3/1520-0469_1967_024_0241_teotaw_2_0_co_2.xml)
- They produced the first credible prediction, using a 1D radiative-convective model, of what would happen to the atmosphere if CO2 levels were changed.
- “what will happen to the global average temperature if the radiative transfer of energy between the surface and the troposphere is altered by an increase in CO2 levels?” CarbonBrief.
- “they want to know what the potential feedbacks from water vapour and clouds might be, which they discover strongly influence the CO2 effect.” CarbonBrief
- “They estimate the effect of doubling CO2 levels – a metric which later becomes known as “climate sensitivity” – and settle on a value of 2.4C.” CarbonBrief
- A post by Professor John Mitchell: [here](https://www.carbonbrief.org/prof-john-mitchell-how-a-1967-study-greatly-influenced-climate-change-science/)
- An interview with Syukuro Manabe: [here](https://www.carbonbrief.org/the-carbon-brief-interview-syukuro-manabe/)
```{figure} /_static/lecture_specific/lecture1_figures/mw_1976_flowchart.png
:scale: 50%
```

## 1969 - William D Sellers
- [A Global Climatic Model Based on the Energy Balance of the Earth-Atmosphere System](https://journals.ametsoc.org/view/journals/apme/8/3/1520-0450_1969_008_0392_agcmbo_2_0_co_2.xml)
- I think Sellers' model is a prototype of (moist) energy balance model.
```{figure} /_static/lecture_specific/lecture1_figures/seller_ebm.png
:scale: 50%
```

## 1969 - Nimbus3
- NASA's Nimbus3 satellite was launched April 14, 1969 [(here)](https://nssdc.gsfc.nasa.gov/nmc/spacecraft/display.action?id=1969-037A).
- The era of climate scientists relying on satellite data to test and validate their models has begun.
```{figure} /_static/lecture_specific/lecture1_figures/nimbus3_1969.png
:scale: 50%
```

## 1969 - Manabe and Bryan
- [Climate calculations with a combined ocean-atmosphere model](https://www.gfdl.noaa.gov/bibliography/related_files/sm6903.pdf)
- the first “coupled” atmosphere-ocean GCM
- The computing time issue: it takes 1,100 hours (about 46 days) to do just one run of the model.
- Simplified land and ocean domains.
```{figure} /_static/lecture_specific/lecture1_figures/coupled_gcm.png
:scale: 50%
```

## 1970 - two meetings on human's impact on climate
- Study of Critical Environmental Problems (SCEP) [here](https://mitpress.mit.edu/9780262690270/mans-impact-on-the-global-environment/)
- Inadvertent Climate Modification: Report of the Study of Man’s Impact on Climate” (SMIC) [here](https://mitpress.mit.edu/9780262690331/inadvertent-climate-modification/)
- "Global climate models are presented as being “indispensable” for researching human-caused climate change." CarbonBrief

## 1970 - NOAA
- President Richard Nixon: "serve a national need for better protection of life and property from natural hazards…for a better understanding of the total environment".

## 1972 - the United Nations Conference on the Human Environment
- the UN Environment Program.
- Human-caused climate change is now turn to the radar of politicians.

## 1972 - UK Met Office 
- The first UK coupled GCM, developed since 1963.
- They used the UK-based ATLAS computer, led by George Corby.
- Five-layer atmospheric GCM
- [A general circulation model of the atmosphere suitable forlong period integrations](https://rmets.onlinelibrary.wiley.com/doi/epdf/10.1002/qj.49709841808)
- [A numerical experiment using a general circulation modelof the atmosphere](https://rmets.onlinelibrary.wiley.com/doi/epdf/10.1002/qj.49709941903)

## 1975 - Manabe and Wetherald 
- Another seminal paper for climate modeling: [The Effects of Doubling the CO2 Concentration on the climate of a General Circulation Model](https://journals.ametsoc.org/view/journals/atsc/32/1/1520-0469_1975_032_0003_teodtc_2_0_co_2.xml)
- For the first time the effects of doubling atmospheric CO2 levels.
- polar amplification
- climate sensitivity of 2.9C
```{figure} /_static/lecture_specific/lecture1_figures/mb1_1975.png
:scale: 70%
```
```{figure} /_static/lecture_specific/lecture1_figures/mw2_1975.png
:scale: 70%
```

## 1975 - Manabe, Bryan, Spelman
- the first coupled atmosphere-ocean GCM (AOGCM)
- [A Global Ocean-Atmosphere Climate Model. Part I. The Atmospheric Circulation](https://journals.ametsoc.org/view/journals/phoc/5/1/1520-0485_1975_005_0003_agoacm_2_0_co_2.xml)
```{figure} /_static/lecture_specific/lecture1_figures/manabe_topography.png
:scale: 70%
```

## 1975 - National Academy of Sciences
- the US Committee for the Global Atmospheric Research Program (GARP) 
- [Understanding Climatic Change: A Program for Action](https://archive.org/stream/understandingcli00unit#page/n5/mode/2up)

## 1975 - Wally Broecker
- first coined the term "global warming".
- [Climatic Change: Are We on the Brink of a Pronounced Global Warming?](https://www.science.org/doi/epdf/10.1126/science.189.4201.460)
```{figure} /_static/lecture_specific/lecture1_figures/global_warming111.png
:scale: 110%
```
## 1977 - Arakawa and Lamb
- [Computational Design of the Basic Dynamical Processes of the UCLA General Circulation Model](https://www.sciencedirect.com/science/article/abs/pii/B9780124608177500094)
- [Methods in Computational Physics: Advances in Research and Applications](https://www.sciencedirect.com/bookseries/methods-in-computational-physics-advances-in-research-and-applications)
```{figure} /_static/lecture_specific/lecture1_figures/arakawa1977.png
:scale: 110%
```

## 1979 - The Charney Report
- [Carbon Dioxide and Climate: A Scientific Assessment](https://geosci.uchicago.edu/~archer/warming_papers/charney.1979.report.pdf)
- A retired representative from the Mobil oil company.
- heat from the atmosphere could be temporarily absorbed by the oceans
- climate sensitivity of 2-4.5C

## 1979 - Newell and Dopplick 
- [Questions concerning the possible influence of anthropogenic CO2 on atmospheric temperature](https://doi.org/10.1175/1520-0450(1979)018<0822:QCTPIO>2.0.CO;2)
```{figure} /_static/lecture_specific/lecture1_figures/controversy1979.png
:scale: 60%
```

## 1980 - World Climate Research Programme (WCRP)
- to determine the predictability of climate and to determine the effect of human activities on climate
- [https://www.wcrp-climate.org/](https://www.wcrp-climate.org/)
```{figure} /_static/lecture_specific/lecture1_figures/wcrp.png
:scale: 60%
```

## 1985 - Department of Energy
- [Projecting the climatic effects of increasing carbon dioxide](https://www.osti.gov/biblio/5885458)
- [report pdf file](https://www.osti.gov/servlets/purl/5885458)
- Carbon dioxide-climate controversy

## 1988 - Hansen’s three scenarios
- [Global climate changes as forecast by Goddard Institute for Space Studies three-dimensional model](https://agupubs.onlinelibrary.wiley.com/doi/abs/10.1029/JD093iD08p09341)
- US Senate hearing
```{figure} /_static/lecture_specific/lecture1_figures/hansen_3_1988.png
:scale: 40%
```
## 1988 - IPCC
- The United Nations Environment Programme (UNEP) and the World Meteorological Organization (WMO) establish the Intergovernmental Panel on Climate Change (IPCC).
- “provide the world with a clear scientific view on the current state of knowledge in climate change and its potential environmental and socio-economic impacts”
- First IPCC chair is Bert Bolin.
- website: [here](https://www.ipcc.ch/)

## 1989 - AMIP
- [The Atmospheric Model Intercomparison Project](https://doi.org/10.1175/1520-0477(1992)073<1962:ATAMIP>2.0.CO;2)

## 1989 - New NCAR and GFDL AOGCMs
- NCAR AOGCM: [Washington and Meehl (1989)](https://link.springer.com/article/10.1007/BF00207397)
```{figure} /_static/lecture_specific/lecture1_figures/wm_1989_scheme.png
:scale: 40%
```
```{figure} /_static/lecture_specific/lecture1_figures/wm_1989_dsat.png
:scale: 40%
```
- GFDL AOGCM: [Stouffer et al. (1989)](https://www.nature.com/articles/342660a0)
```{figure} /_static/lecture_specific/lecture1_figures/sm_1989_dsat.png
:scale: 40%
```

## 1990 - Met Office Hadley Centre
- Margaret Thatcher, the UK prime minister, formally opens the Met Office’s Hadley Centre for Climate Prediction and Research in Bracknell, Berkshire. 
- The first Hadley Centre coupled model boasts “11 atmospheric levels, 17 ocean levels and 2.5° × 3.75° resolution”. 

## 1990 - First IPCC report
- August 27, 1990 — August 30, 1990.
- Report: [here](https://www.ipcc.ch/assessment-report/ar1/)
- about 0.3C warming per decade
```{figure} /_static/lecture_specific/lecture1_figures/IPCC1.png
:scale: 100%
```

## 1990 - Robert D. Cess
- uncertantiy from cloud is large.
- [Intercomparison and Interpretation of Climate Feedback Processes in 19 Atmospheric General Circulation Models](https://pubs.giss.nasa.gov/docs/1990/1990_Cess_ce03000u.pdf)
- [Cloud-Radiative Forcing and Climate: Resultsfrom te Earth Radiation Budget Experiment](https://www.science.org/doi/epdf/10.1126/science.243.4887.57)
```{figure} /_static/lecture_specific/lecture1_figures/albedo_lw.png
:scale: 60%
```
```{figure} /_static/lecture_specific/lecture1_figures/cloud_forcing.png
:scale: 60%
```

## 1991 - Eruption of Mount Pinatubo 
- [Potential climate impact of Mount Pinatubo eruption](https://pubs.giss.nasa.gov/docs/1992/1992_Hansen_ha00800v.pdf)

## 1992 - Kevin Trenberth
- [Climate System Modeling](https://books.google.co.uk/books?id=EDClFW7JWrQC&dq=%22Climate+System+Modeling%22+Cambridge+Univ.+Press+1992+trenberth&source=gbs_navlinks_s)

## 1995 - CMIP launched
- The Coupled Model Intercomparison Project (CMIP)
- 18 models from 14 modelling groups are included
- official website: [here](https://www.wcrp-climate.org/wgcm-cmip)

## 1995 - John Mitchell
- [Climate response to increasing levels of greenhouse gases and sulphate aerosols](https://www.nature.com/articles/376501a0)
- based on Rasool and Schneiderr (1971): [Atmospheric Carbon Dioxide and Aerosols: Effects of Large Increases on Global Climate](https://www.science.org/doi/10.1126/science.173.3992.138)
```{figure} /_static/lecture_specific/lecture1_figures/ghg_aer_ts.png
:scale: 40%
```
```{figure} /_static/lecture_specific/lecture1_figures/ghg_aer_maps.png
:scale: 50%
```

## 1995 - Draft of IPCC’s second assessment report
- Human activity is a likely cause of the warming of the global atmosphere
- Dr Richard S Lindzen from MIT, however, was unconvinced: the model estimate of natural variability being correct?

## 1995 - US Congressional Hearing 
 “climate models and projections of potential impacts of global climate change”.
- chared by Dana Rohrabacher
- He asks: “Are we so certain about the future climate changes that we should take action that will change the lives of millions of our own citizens at a cost of untold billions of dollars?”
```{figure} /_static/lecture_specific/lecture1_figures/us_congress.png
:scale: 50%
```
## 1996 - Ben Santer’s fingerprint study 
- [A search for human influences on the thermal structure of the atmosphere](https://www.nature.com/articles/382039a0)
- attached by climate sceptics: [here](https://www.realclimate.org/index.php/archives/2010/02/close-encounters-of-the-absurd-kind/)
```{figure} /_static/lecture_specific/lecture1_figures/fingerprint.png
:scale: 50%
```

## 2000 - IPCC’s Special Report on Emissions Scenarios
- [SERS](https://www.ipcc.ch/report/emissions-scenarios/?idp=0)
- The IPCC team is led by Professor [Nebojsa Nakicenovic](https://www.carbonbrief.org/the-carbon-brief-interview-prof-nebojsa-nakicenovic/)
- [Representative Concentration Pathways (RCPs)](https://sedac.ciesin.columbia.edu/ddc/ar5_scenario_process/RCPs.html) in time for the IPCC’s fifth assessment report in 2014.
```{figure} /_static/lecture_specific/lecture1_figures/rcp.png
:scale: 40%
```

## 2000 - Peter Cox
- The first “fully coupled three-dimensional carbon-climate model”.
- [Acceleration of global warming due to carbon-cycle feedbacks in a coupled climate model](https://www.nature.com/articles/35041539)
```{figure} /_static/lecture_specific/lecture1_figures/carbon_ts.png
:scale: 50%
```
```{figure} /_static/lecture_specific/lecture1_figures/temp_ts.png
:scale: 60%
```

## 2004 - Peter Stott and Myles Allen
- [Human contribution to the European heatwave of 2003](https://www.nature.com/articles/nature03089)
- It was very likely that human influence at least doubled the chances of heatwave occurring 
```{figure} /_static/lecture_specific/lecture1_figures/attribution.png
:scale: 60%
```

## 2007 - Santer vs Douglass
- [A comparison of tropical temperature trends with model predictions](https://ncwatch.typepad.com/media/files/DOUGLASPAPER.pdf)
```{figure} /_static/lecture_specific/lecture1_figures/douglass.png
:scale: 60%
```

- [Consistency of modelled and observed temperature trends inthe tropical troposphere](https://www.researchgate.net/publication/323160419_Consistency_of_Modeled_and_Observed_Temperature_Trends_in_the_Tropical_Troposphere)
```{figure} /_static/lecture_specific/lecture1_figures/santer_2008.png
:scale: 60%
```

- Climategate affair in 2009.

## 2008 - Tim Lenton
- [Tipping elements in the Earth’s climate system](https://www.pnas.org/doi/epdf/10.1073/pnas.0705414105)
```{figure} /_static/lecture_specific/lecture1_figures/tipping.png
:scale: 40%
```

## 2008 - Veerabhadran Ramanathan and Greg Carmichael
- [Global and regional climate changes due to black carbon](https://www.nature.com/articles/ngeo156)

## 2008 - CMIP5

## 2013 - IPCC’s fifth assessment report
- [Report here](https://www.ipcc.ch/assessment-report/ar5/)
```{figure} /_static/lecture_specific/lecture1_figures/ipcc_ar5.png
:scale: 60%
```

## 2017 - Medhaug et al.
- [Reconciling controversies about the ‘global warming hiatus’](https://www.nature.com/articles/nature22315)

## 2021 - IPCC’s sixth assessment report
- [Report here](https://www.ipcc.ch/assessment-report/ar6/)
```{figure} /_static/lecture_specific/lecture1_figures/ipcc_ar6.png
:scale: 40%
```

## 2021 - The Nobel Prize in Physics
- Syukuro Manabe
- Klaus Hasslmann
```{figure} /_static/lecture_specific/lecture1_figures/nobel_2021.png
:scale: 40%
```

```{figure} /_static/lecture_specific/lecture1_figures/ipcc_history.png
:scale: 40%
```

## 2024 - Google's NeuralGCM
- [NeuralGCM](https://neuralgcm.readthedocs.io/en/latest/trained_models.html)
- [Neural general circulation models for weather and climate](https://www.nature.com/articles/s41586-024-07744-y)
```{figure} /_static/lecture_specific/lecture1_figures/neuralgcm_structure.png
:scale: 40%
```
```{figure} /_static/lecture_specific/lecture1_figures/amip_neuralgcm.png
:scale: 60%
```

## future from now?
- high-resolution climate modeling?
- AI approach?

