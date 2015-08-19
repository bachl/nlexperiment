


[![](https://travis-ci.org/bergant/nlexperiment.svg)](https://travis-ci.org/bergant/nlexperiment)

----
# __[<font color="red">nl</font><font color = "#444444">experiment</font>](http://bergant.github.io/nlexperiment/)__

####Define and run NetLogo experiments in R


Provides functions to 
define __NetLogo[^1] experiments__ with parameter sets, measures and 
related simulation options in __concise__ structure. 
The cycle of experiment definition, simulations, data analysis, visualisations and
parameter fitting can be easily turned 
into __readable__ and __reproducible__ documents.

[^1]: Wilensky, U. (1999). NetLogo. http://ccl.northwestern.edu/netlogo/. Center for Connected Learning and Computer-Based Modeling, Northwestern University, Evanston, IL.

__RNetLogo__[^2] is used as an interface to NetLogo environment.

[^2]: Jan C. Thiele (2014). R Marries NetLogo: Introduction to the RNetLogo Package. Journal
of Statistical Software, 58(2), 1-41. URL http://www.jstatsoft.org/v58/i02/.

## Documentation and Examples
See __[project website](http://bergant.github.io/nlexperiment/)__ for more information
or go through __[examples](http://bergant.github.io/nlexperiment/tutorial.html)__ to 
learn how to create experiments.


## Installation


```r
install.packages("RNetLogo")
devtools::install_github("bergant/nlexperiment")
```

## Example
Simple Experiment with NetLogo Fire model[^3]:

[^3]: Wilensky, U. (1997). 
[NetLogo Fire model](http://ccl.northwestern.edu/netlogo/models/Fire). 
Center for Connected Learning and Computer-Based Modeling, 
Northwestern University, Evanston, IL.


```r
library(nlexperiment)
nl_netlogo_path("c:/Program Files (x86)/NetLogo 5.2.0") 
```

Define the experiment:

```r
experiment <- nl_experiment(
  model_file = "models/Sample Models/Earth Science/Fire.nlogo", 
  while_condition = "any? turtles",
  param_values = list(density = c(57, 59, 61)),
  random_seed = 1,
  step_measures = measures(
    percent_burned = "(burned-trees / initial-trees) * 100"
  )
)
```

Run the experiment:


```r
result <- nl_run(experiment)  
```

Plot the results:

```r
nl_show_step(result, x = "step_id", y = "percent_burned", 
             x_param = "density")
```

![](img/README-fireStepPlotRM-1.png) 


See more [examples](http://bergant.github.io/nlexperiment/tutorial.html) 
on the [project website](http://bergant.github.io/nlexperiment/).

