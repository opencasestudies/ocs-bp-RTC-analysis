<!-- README.md is generated from README.Rmd. Please edit that file -->
OpenCaseStudies
===============

<!-- badges: start -->
[![R build
status](https://github.com/opencasestudies/ocs-bp-RTC-analysis/workflows/R-CMD-check/badge.svg)](https://github.com/opencasestudies/ocs-bp-RTC-analysis/actions)
<!-- badges: end -->

### Disclaimer

The purpose of the [Open Case
Studies](https://opencasestudies.github.io) project is **to demonstrate
the use of various data science methods, tools, and software in the
context of messy, real-world data**. A given case study does not cover
all aspects of the research process, is not claiming to be the most
appropriate way to analyze a given dataset, and should not be used in
the context of making policy decisions without external consultation
from scientific experts.

### License

This case study is part of the
[OpenCaseStudies](https://opencasestudies.github.io) project. This work
is licensed under the Creative Commons Attribution-NonCommercial 3.0
([CC BY-NC 3.0](https://creativecommons.org/licenses/by-nc/3.0/us/))
United States License.

### Citation

To cite this case study:

Wright, Carrie, and Ontiveros, Michael and Jager, Leah and Taub,
Margaret and Hicks, Stephanie. (2020).
<a href="https://github.com/opencasestudies/ocs-bp-RTC-analysis" class="uri">https://github.com/opencasestudies/ocs-bp-RTC-analysis</a>.
Influence of Multicollinearity on Measured Impact of Right-to-Carry Gun
Laws (Version v1.0.0).

### Acknowledgements

We would like to acknowledge [Daniel
Webster](https://www.jhsph.edu/faculty/directory/profile/739/daniel-webster)
for assisting in framing the major direction of the case study. We would
also like to thank [Elizabeth
Stuart](https://www.jhsph.edu/faculty/directory/profile/1792/elizabeth-a-stuart)
for reviewing the case study.

We would also like to acknowledge the [Bloomberg American Health
Initiative](https://americanhealth.jhu.edu/) for funding this work.

### Title

Influence of Multicollinearity on Measured Impact of Right-to-Carry Gun
Laws Part 2

### Motivation

The influence of the implementation of less restrictive right-to-carry
gun laws on violent crime is a historically controversial topic. One
reason for the contriversy, is concern that some earlier reports examing
this topic may have used methods that were inappropriate.

One of the major concerns is that an earlier report included multiple
demographic variables that were collinear with one another. This
resulted in different a very coefficient estimate for right-to-carry gun
law adoption than other reports that did not include colinear varaibles.
This phenomenon is called
[multicollinearity](https://en.wikipedia.org/wiki/Multicollinearity),
and it can result in abberant findings for particular explanatory
variables, despite not altering the overall predictive power of a model.

In this case study we use data peform simplified analyses similar to
those of reports on this topic to explore the influence of
multicolinearity on coeffcient estimate stability. We however, do not
recreate the previous analyses. The reports that we use as a guide for
our analysis are:

1.  John J. Donohue et al., Right‐to‐Carry Laws and Violent Crime: A
    Comprehensive Assessment Using Panel Data and a State‐Level
    Synthetic Control Analysis. *Journal of Empirical Legal Studies*,
    16,2 (2019).

2.  David B. Mustard & John Lott. Crime, Deterrence, and Right-to-Carry
    Concealed Handguns. *Coase-Sandor Institute for Law & Economics*
    Working Paper No. 41, (1996).

### Motivating question

What is the effect of multicollinearity on coefficient estimates from
linear regression models when analyzing right to carry laws and violence
rates?

### Data

In this case study, we perform analyses similar to those in
<a href="https://www.nber.org/papers/w23510.pdf" target="_blank">Donohue, et al.</a>
article and the
<a href="https://chicagounbound.uchicago.edu/cgi/viewcontent.cgi?article=1150&amp;context=law_and_economics" target="_blank">Lott and Mustard</a>
article, however **we do not try to recreate them**, instead we perform
simplified analyses to allow us to focus on multicolinearity.

Therefore we use a subset of the **explanatory variables** used by each
article including:

1.  Data about state demographics in terms of population compositions
    for age, sex, race, as well as overall population values from the US
    Census Bureau:

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>Data</th>
<th>Link</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>years 1977 to 1979</strong></td>
<td><a href="https://www2.census.gov/programs-surveys/popest/tables/1900-1980/state/asrh/">link</a></td>
</tr>
<tr class="even">
<td><strong>years 1980 to 1989</strong></td>
<td><a href="https://www2.census.gov/programs-surveys/popest/tables/1980-1990/counties/asrh/">link</a> * county data was used for this decade which also has state information</td>
</tr>
<tr class="odd">
<td><strong>years 1990 to 1999</strong></td>
<td><a href="https://www2.census.gov/programs-surveys/popest/tables/1990-2000/state/asrh/">link</a></td>
</tr>
<tr class="even">
<td><strong>years 2000 to 2010</strong></td>
<td><a href="https://www.census.gov/data/datasets/time-series/demo/popest/intercensal-2000-2010-state.html">link</a> <br> <a href="https://www2.census.gov/programs-surveys/popest/technical-documentation/file-layouts/2000-2010/intercensal/state/st-est00int-alldata.pdf" target="_blank">technical documentation</a></td>
</tr>
</tbody>
</table>

Six demographic variables are created for the
<a href="https://www.nber.org/papers/w23510.pdf" target="_blank">Donohue, et al.</a>-like
analysis and 36 were created for the
<a href="https://chicagounbound.uchicago.edu/cgi/viewcontent.cgi?article=1150&amp;context=law_and_economics" target="_blank">Lott and Mustard</a>-like
analysis.

To use this data, we also need [Federal Information Processing Standard
(FIPS) state
codes](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standard_state_code){target="\_blank",
to identify what demographic data corresponds to what state. This is
also available from the
<a href="https://www.census.gov/geographies/reference-files/2014/demo/popest/2014-geocodes-state.html" target="_blank">US Census Bureau</a>.

1.  Police staffing data, which was downloaded from the
    <a href="https://crime-data-explorer.fr.cloud.gov/downloads-and-docs" target="_blank">Federal Bureau of Investigation</a>

2.  Unemployment data, which was downloaded from the
    <a href="https://data.bls.gov/cgi-bin/dsrv?la" target="_blank">U.S. Bureau of Labor Statistics</a>.

3.  Poverty data, extracted from Table 21 from this
    <a href="https://www.census.gov/data/tables/time-series/demo/income-poverty/historical-poverty-people.html" target="_blank">US Census Bureau Poverty Data</a>

4.  Right-to-carry law data, which is available in a table in the
    <a href="https://www.nber.org/papers/w23510.pdf" target="_blank">Donohue paper</a>

Finally our outcome of interest is violent crime rates. The violent
crime data was downloaded from the
<a href="https://www.ucrdatatool.gov/Search/Crime/State/StatebyState.cfm" target="_blank">FBI uniform crime reporting system</a>

#### Learning Objectives

The skills, methods, and concepts that students will be familiar with by
the end of this case study are:

<u>**Statistical Learning Objectives:**</u>

1.  what multicollinearity is and how it can influence linear regression
    coefficients  
2.  how to look for the presence of multicollinarity and determine the
    severity
3.  the difference between multicollinearity and correlation
4.  implementation of panel regression analysis (`plm`)
5.  calcuation of VIF (`car`)

<u>**Data science Learning Objectives:**</u>

1.  data import of many different file types with special cases
    (`readr`, `readxl`, `pdftools`)
2.  joining data from multiple sources (`dplyr`)  
3.  working with character strings (`stringr`)
4.  data comparisons (`dplyr` and `janitor`)
5.  reshaping data into different formats (`tidyr`)  
6.  visualizations (`ggplot2`)
7.  perform iterative simulations (`rsample`)

#### Data import and wrangling

See [the part 1 case
study](https://github.com/opencasestudies/ocs-bp-RTC-wrangling) for the
data import and data wrangling details.

#### Data Visualization

This case study demonstrates how to make correaltion plots and scatter
plots with error bars. We also show how to add formulas and arrows to
plots. The instruction about data visualization assumes that students
have some familiarity with `ggplot2`.

### Analysis

This case study covers balanced panel regression model data analysis
with fixed effects. In doing so we provide an introduction to
longitudinal analysis in general, as well as use of the `plm` package.
We also show how to calculate [Variance inflation factor
(VIF)](https://en.wikipedia.org/wiki/Variance_inflation_factor) values
using the `car` package to quantify the severity of multicollinearity.
As another assesment of multicollinearity, we demonstrate how to perform
simulations to evaluate the stability of coefficient estimates.

### Other notes and resources

<a href="https://www.tidyverse.org/" target="_blank">Tidyverse</a>  
Please see
<a href="https://opencasestudies.github.io/ocs-bp-co2-emissions/" target="_blank">this case study</a>
for more details on using `ggplot2`  
<a href="https://www.bmj.com/about-bmj/resources-readers/publications/epidemiology-uninitiated/7-longitudinal-studies" target="_blank">Longitudinal studies</a>  
<a href="https://en.wikipedia.org/wiki/Panel_data" target="_blank">Panel data</a>  
<a href="https://en.wikipedia.org/wiki/Confidence_interval" target="_blank">Confidence intervals</a>  
<a href="https://en.wikipedia.org/wiki/Linear_regression" target="_blank">Linear regression</a>  
<a href="https://en.wikipedia.org/wiki/Panel_analysis" target="_blank">panel regression analysis</a>  
<a href="https://en.wikipedia.org/wiki/Durbin%E2%80%93Wu%E2%80%93Hausman_test" target="_blank">Hausmen test</a>
<a href="https://en.wikipedia.org/wiki/Resampling_(statistics)" target="_blank">Resampling</a>  
<a href="https://en.wikipedia.org/wiki/Variance_inflation_factor" target="_blank">Variance inflation factor (VIF)</a>  
<a href="https://en.wikipedia.org/wiki/Coefficient_of_determination" target="_blank"><span class="math inline"><em>R</em><sup>2</sup></span> coefficient of determination</a>  
<a href="https://en.wikipedia.org/wiki/Tikhonov_regularization" target="_blank">Ridge regression</a>  
[LaTeX mathematical
notation](https://www.calvin.edu/~rpruim/courses/s341/S17/from-class/MathinRmd.html)target="\_blank"}

For more information on linear regression see this
<a href="https://rafalab.github.io/dsbook/linear-models.html#linear-regression-in-the-tidyverse" target="_blank">book</a>
and this
<a href="https://opencasestudies.github.io/ocs-bp-diet/" target="_blank">case study</a>.

For more information on the different types of panel regression models
see this
[book](https://bookdown.org/ccolonescu/RPoE4/panel-data-models.html),
[here](https://www.bauer.uh.edu/rsusmel/phd/ec1-15.pdf), and
[here](https://sites.google.com/site/econometricsacademy/econometrics-models/panel-data-models).

For more information on implementing panel regession in R using the
`plm` package, see
<a href="https://cran.r-project.org/web/packages/plm/vignettes/plmPackage.html" target="_blank">here</a>
and
<a href="http://www.princeton.edu/~otorres/Panel101R.pdf" target="_blank">here</a>.

For more information on multioclinearity and VIF, see this
<a href="https://link.springer.com/content/pdf/10.1007/s11135-006-9018-6.pdf" target="_blank">article</a>.DOI
10.1007/s11135-006-9018-6

The articles used to motivate this case study are:  
<a href="https://chicagounbound.uchicago.edu/cgi/viewcontent.cgi?article=1150&amp;context=law_and_economics" target="_blank">Lott and Mustard</a>  
<a href="https://www.nber.org/papers/w23510.pdf" target="_blank">Donohue, et al.</a>  
<a href="https://en.wikipedia.org/wiki/More_Guns,_Less_Crime" target="_blank">See here for a list of studies on this topic</a>

<table>
<thead>
<tr class="header">
<th>Package</th>
<th>Use in this case study</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="https://github.com/jennybc/here_here" target="_blank">here</a></td>
<td>to easily load and save data</td>
</tr>
<tr class="even">
<td><a href="https://dplyr.tidyverse.org/" target="_blank">dplyr</a></td>
<td>to arrange/filter/select/compare specific subsets of the data</td>
</tr>
<tr class="odd">
<td><a href="https://cran.r-project.org/web/packages/magrittr/vignettes/magrittr.html" target="_blank">magrittr</a></td>
<td>to use the compound assignment pipe operator <code>%&lt;&gt;%</code></td>
</tr>
</tbody>
</table>

<a href="https://purrr.tidyverse.org/" target="_blank">purrr</a> | to
import the data in all the different excel and csv files efficiently
<a href="https://tibble.tidyverse.org/" target="_blank">tibble</a> | to
create data objects that we can manipulate with
`dplyr`/`stringr`/`tidyr`/`purrr`
<a href="https://ggplot2.tidyverse.org/" target="_blank">ggplot2</a> |
to create plots  
<a href="https://cran.r-project.org/web/packages/ggrepel/vignettes/ggrepel.html" target="_blank">ggrepel</a>
| to allow labels in figures not to overlap  
<a href="https://cran.r-project.org/web/packages/plm/vignettes/plmPackage.html" target="_blank">plm</a>
| to work with panel data fitting fixed effects and linear regression
models
<a href="https://cran.r-project.org/web/packages/broom/vignettes/broom.html" target="_blank">broom</a>
| to create nicely formatted model output  
<a href="https://github.com/ggobi/ggally" target="_blank">GGally</a> |
to extend ggplot2 functionality to easily create more complex plots  
<a href="https://www.rdocumentation.org/packages/ggcorrplot/versions/0.1.3" target="_blank">ggcorrplot</a>
| to easily visualize a correlation matrix  
<a href="https://rsample.tidymodels.org" target="_blank">rsample</a> |
to split our sample for the simulation analysis  
<a href="https://rstudio.github.io/DT/" target="_blank">DT</a> | to
create interactive and searchable tables  
<a href="https://cran.r-project.org/web/packages/car/vignettes/embedding.pdf" target="_blank">car</a>
| to calculate VIF values on linear model output
<a href="https://stringr.tidyverse.org/articles/stringr.html" target="_blank">stringr</a>
| to manipulate the character strings within the data  
<a href="https://cran.r-project.org/web/packages/cowplot/vignettes/introduction.html" target="_blank">cowplot</a>
| to allow plots to be combined
<a href="https://cran.r-project.org/web/packages/latex2exp/vignettes/using-latex2exp.html" target="_blank">latex2exp</a>
| to convert latex math formulas to R’s plotmath expressions

#### For users

There is a [`Makefile`](Makefile) in this folder that allows you to type
`make` to knit the case study contained in the `index.Rmd` to
`index.html` and it will also knit the [`README.Rmd`](README.Rmd) to a
markdown file (`README.md`).

#### For instructors

If instructors want more details about the data import and wrangling for
the data used in this analysis, start with [this case
study](https://github.com/opencasestudies/ocs-bp-RTC-wrangling).

#### Target audience

For individuals or classes with some familiarity with regression and
`ggplot2`. See this
<a href="https://opencasestudies.github.io/ocs-bp-diet/" target="_blank">case study</a>
for an introduction to regression.

#### Suggested homework

Ask students to remove one or more of the demographic variables with
high VIF values from the Lott-like panel data and perform the panel
linear regression analysis again, as well as cacluate the VIF values.

Ask the students to discuss how this possibly changed the results.
