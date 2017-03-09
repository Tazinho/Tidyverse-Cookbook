
# Factors

**Task:** Create a factor.


```r
#   ____________________________________________________________________________
factor(letters)
#>  [1] a b c d e f g h i j k l m n o p q r s t u v w x y z
#> Levels: a b c d e f g h i j k l m n o p q r s t u v w x y z
```

**Task:** Check if an atomic vector is a factor.


```r
#   ____________________________________________________________________________
is.factor(factor(letters))
#> [1] TRUE
```

**Task:** Create an ordered factor.


```r
#   ____________________________________________________________________________
factor(letters, ordered = TRUE)
#>  [1] a b c d e f g h i j k l m n o p q r s t u v w x y z
#> 26 Levels: a < b < c < d < e < f < g < h < i < j < k < l < m < n < ... < z
```

**Task:** Check if a factor is ordered.


```r
#   ____________________________________________________________________________
is.ordered(factor(letters))
#> [1] FALSE
```

**Task:** Add a factor level.


```r
#   ____________________________________________________________________________
a <- factor(letters[1:5])
levels(a) <- c(levels(a), "new_level")
a
#> [1] a b c d e
#> Levels: a b c d e new_level
```

**Task:** Add a value (that is not a level of the factor).


```r
#   ____________________________________________________________________________
a <- factor(letters[1:5])
a <- factor(c(as.character(a), "new_level"))
a
#> [1] a         b         c         d         e         new_level
#> Levels: a b c d e new_level
```

**Task:** Combine two factors.


```r
#   ____________________________________________________________________________
a <- factor(letters[1:5])
b <- factor(letters[3:7])
factor(c(as.character(a), as.character(b)))
#>  [1] a b c d e c d e f g
#> Levels: a b c d e f g
```

**Task:** Sort the levels of a factor by another variable.

**Task:** Reorder the levels of a factor by another variable.

**Task:** Reverse the order of factor levels.

**Task:** Change specific levels of a factor by name, while preserving the order.


```r
a <- factor(letters[1:5])
forcats::fct_recode(a, g= "a", h = "b")
#> [1] g h c d e
#> Levels: g h c d e
# Note that the folowing will not work, since f is also the argument name of the 
# data: forcats::fct_recode(a, f= "a", g = "b")
```

**Task:** Delete a factor level.


```r
a <- factor(letters[1:5])
forcats::fct_recode(a, NULL= "a", h = "b")
#> [1] <NA> h    c    d    e   
#> Levels: h c d e
```

**Task:** Move specific factor levels in front of all others.

**Task:** Count factors/strings.


```r
a <- factor(rep(letters[1:5], 5:1))
forcats::fct_count(a)
#> # A tibble: 5 × 2
#>        f     n
#>   <fctr> <int>
#> 1      a     5
#> 2      b     4
#> 3      c     3
#> 4      d     2
#> 5      e     1
```

**Task:** Join/bind/combine/compose/unite rare/common factor levels into one.


```r
a <- rep(letters[1:5], 5:1)
forcats::fct_lump(a, n = 2)
#>  [1] a     a     a     a     a     b     b     b     b     Other Other
#> [12] Other Other Other Other
#> Levels: a b Other
forcats::fct_lump(a, n = -2)
#>  [1] Other Other Other Other Other Other Other Other Other Other Other
#> [12] Other d     d     e    
#> Levels: d e Other
```

## Resources

* [forcats 0.1 on RStudio blog](https://blog.rstudio.org/2016/08/31/forcats-0-1-0/)
