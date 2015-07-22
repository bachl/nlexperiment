






## Mapping parameters
NetLogo identifiers may include some ASCII characters (?=*!<>:#+/%$^'&-)
that makes the R part of data manipulation rather uncomfortable. 
The following example is using Ant model (Wilensky 1997) to show 
**how to use nice names in R and map them to NetLogo variables**.


```r
experiment <- nl_experiment(
  model_file = file.path(nl_netlogo_path(), 
                         "models/Sample Models/Biology/Ants.nlogo"), 
  max_ticks = 150,
  step_measures = measures(
    pile1 = "sum [food] of patches with [pcolor = cyan]",  
    pile2 = "sum [food] of patches with [pcolor = sky]",  
    pile3 = "sum [food] of patches with [pcolor = blue]"  
  ),
  param_values = list(
    population = 125,
    diffusion_rate = c(50, 60),
    evaporation_rate = c(5, 10, 15)
  ),
  mapping = c(
    diffusion_rate = "diffusion-rate",
    evaporation_rate = "evaporation-rate"
    ),
  random_seed = 1,
  export_view = TRUE
)
```

Run experiment

```r
results <- nl_run(experiment)
```

Show views

```r
nl_show_view(results)
```

![](img/README-p5ShowViews-1.png) ![](img/README-p5ShowViews-2.png) ![](img/README-p5ShowViews-3.png) ![](img/README-p5ShowViews-4.png) ![](img/README-p5ShowViews-5.png) ![](img/README-p5ShowViews-6.png) 

Show remaining food by parameter space and food piles

```r
library(tidyr)
dat <- nl_get_step_result(results)
dat <- tidyr::gather(dat, pile, value, pile1, pile2, pile3)

library(ggplot2)
ggplot(dat, aes(x = tick, y = value, color = pile) ) +
  geom_line() +
  facet_grid(diffusion_rate ~ evaporation_rate)
```

![](img/README-p5plot-1.png) 
