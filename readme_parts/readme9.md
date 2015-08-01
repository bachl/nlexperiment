





## Best-fit criteria function
Using best-fit criteria is done with the same attributes.
As in Thiele, Kurth & Grimm (2014) we define cost function which
is 0 when criteria are met and > 0 outside acceptable ranges:


```r
cond_cost_function <- function(value, minval, maxval) {
  # squared relative deviation if value outside accepted range
  ifelse(
    minval > value  | value > maxval,
    ret <- ((mean(minval,maxval) - value) / mean(minval,maxval))^2,
    0
  )
}
```

_Note: the function can use a vector in `value` attribute_

The rest is analogous to the previous example except `eval_mutate`
defines only one value:


```r
experiment <- nl_experiment( 
  model_file = 
    system.file("netlogo_models/SM2_Hoopoes.nlogo", package = "nlexperiment"),
  
  setup_commands = c("setup", "repeat 24 [go]"), # 2 years of warming-up
  go_command = "repeat 12 [go]",                 # iteration is per year
  iterations = 20,                               # run for 20 years
  repetitions = 10,                              # repeat simulation 10 times
  
  param_values = list(                           # "full factor design"
    scout_prob = seq(from = 0.00, to = 0.50, by = 0.05),
    survival_prob = seq(from = 0.950, to = 1.000, by = 0.005)
  ),
  mapping = c(                                   # map NetLogo variables
    scout_prob = "scout-prob", 
    survival_prob = "survival-prob"
  ),
  
  step_measures = measures(                      # NetLogo reporters per step
    abund = "month-11-count",             
    alpha = "month-11-alpha",                 
    patches_count = "count patches"
  ),

  eval_criteria = criteria(                      # evaluation per each run
    abundance = mean(step$abund),
    variation = sd(step$abund),
    vacancy = mean(step$alpha / step$patches_count)
  ),
  
  eval_aggregate_fun = mean,                     # mean value (10 repetitions)
  
  eval_mutate = criteria(                        # add categorical values
    cost = 
      cond_cost_function(abundance, 115, 135) +
      cond_cost_function(variation, 10, 15) +
      cond_cost_function(vacancy, 0.15, 0.30)
  )  
)
```


Run experiment:

```r
result <- nl_run(experiment, parallel = TRUE) 
# get the data (criteria)
dat <- nl_get_result(result, type = "criteria") 
```


Plot evaluation criteria on the model parameter space:

```r
library(ggplot2)
ggplot(dat , aes(x = factor(scout_prob), y = factor(survival_prob), fill = pmin(cost, 30))) +
  geom_tile() + theme_minimal() + labs(x = "scout_prob", y= "survival_prob") +
  coord_fixed()
```

![](img/README-p9SplotParameterSpace-1.png) 