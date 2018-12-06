


# (PART) Applications {-} 

# Models

**Task:** Extract coefficients from a model.


```r
library(magrittr)
lm(Sepal.Width ~ Species, data = iris) %>% 
  broom::tidy(.)
#>                term estimate  std.error statistic       p.value
#> 1       (Intercept)    3.428 0.04803910 71.358540 5.707614e-116
#> 2 Speciesversicolor   -0.658 0.06793755 -9.685366  1.832489e-17
#> 3  Speciesvirginica   -0.454 0.06793755 -6.682608  4.538957e-10
#   ____________________________________________________________________________
```

**Task:** Extract residuals from a model.


```r
lm(Sepal.Width ~ Species, data = iris) %>% 
  broom::augment(.) %>% head()
#>   Sepal.Width Species .fitted   .se.fit .resid .hat    .sigma      .cooksd
#> 1         3.5  setosa   3.428 0.0480391  0.072 0.02 0.3407959 0.0003118616
#> 2         3.0  setosa   3.428 0.0480391 -0.428 0.02 0.3389658 0.0110200713
#> 3         3.2  setosa   3.428 0.0480391 -0.228 0.02 0.3403157 0.0031272785
#> 4         3.1  setosa   3.428 0.0480391 -0.328 0.02 0.3397443 0.0064720901
#> 5         3.6  setosa   3.428 0.0480391  0.172 0.02 0.3405456 0.0017797285
#> 6         3.9  setosa   3.428 0.0480391  0.472 0.02 0.3385573 0.0134023471
#>   .std.resid
#> 1  0.2141113
#> 2 -1.2727728
#> 3 -0.6780191
#> 4 -0.9753959
#> 5  0.5114881
#> 6  1.4036185
#   ____________________________________________________________________________
```

**Task:** Extract measures of fit from a model.


```r
lm(Sepal.Width ~ Species, data = iris) %>% 
  broom::glance()
#>   r.squared adj.r.squared     sigma statistic      p.value df   logLik
#> 1 0.4007828     0.3926302 0.3396877  49.16004 4.492017e-17  3 -49.3663
#>        AIC      BIC deviance df.residual
#> 1 106.7326 118.7751   16.962         147
#   ____________________________________________________________________________
```

## Resources
