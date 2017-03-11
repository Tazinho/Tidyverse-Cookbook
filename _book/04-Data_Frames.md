
# (PART) Acting on common data structures {-} 

# Data frames and tibbles


## Formatting
**Task:** Change column names to lower case.


```r
stringr::str_to_lower(names(iris))
#> [1] "sepal.length" "sepal.width"  "petal.length" "petal.width" 
#> [5] "species"
#   ____________________________________________________________________________
tolower(names(iris))
#> [1] "sepal.length" "sepal.width"  "petal.length" "petal.width" 
#> [5] "species"
```

**Task:** Change column names to snake_case.


```r
library(magrittr)
c("Var 1", "Var-2", "Var.3", "Var4") %>%
  stringr::str_replace_all("\\s|-|\\.", "_") %>% 
  stringr::str_to_lower()
#> [1] "var_1" "var_2" "var_3" "var4"
```

**Task:** Change column names from camelCase or CamelCase to snake_case and reversed.

## Basic operations and related stuff

**Task:** Select a column and return it as vector (not data.frame).


```r
iris[1,] %>% .$Species
#> [1] setosa
#> Levels: setosa versicolor virginica
#   ____________________________________________________________________________
iris[1,][["Species"]]
#> [1] setosa
#> Levels: setosa versicolor virginica
```

**Task:** Select columns from (and return) data.frame.


```r
iris %>% select(Sepal.Width, Species)
#   ____________________________________________________________________________
iris[, c("Sepal.Width", "Species"), drop = FALSE]
```

**Task:** Filter/subset a data.frame.


```r
iris %>% filter(Species == "versicolor" & Sepal.Width < 3)
#   ____________________________________________________________________________
iris[iris[, "Species"] == "versicolor" & iris[, "Sepal.Width"] < 3]
```

**Task:** Mutate/transform/calculate new columns of a data.frame.


```r
iris %>% mutate(a = Sepal.Length, b = 2 * a)
#   ____________________________________________________________________________
iris[["a"]] <- iris[["Sepal.Length"]]
iris[["b"]] <- 2 * iris[["a"]]
```

**Task:** Arrange/order a data.frame.


```r
dplyr::arrange(iris, dplyr::desc(Sepal.Length))
#   ____________________________________________________________________________
iris[order(iris["Sepal.Length", ], decreasing = TRUE), ]
```

**Task:** Rename colums


```r
iris %>%
  dplyr::rename(a = Species,
                b = Sepal.Length)
# or purrr::set_names()
# or dplyr::select()
#   ____________________________________________________________________________
# Use names() or setNames()
```

**Task:** Use some helpers to select columns based on naming patterns


```r
iris[1,] %>%
  dplyr::select(dplyr::starts_with("S",
                                   ignore.case = TRUE,
                                   vars = dplyr::current_vars()))
#>   Sepal.Length Sepal.Width Species
#> 1          5.1         3.5  setosa
iris[1,] %>% dplyr::select(dplyr::ends_with("h"))
#>   Sepal.Length Sepal.Width Petal.Length Petal.Width
#> 1          5.1         3.5          1.4         0.2
iris[1,] %>% dplyr::select(dplyr::contains("."))
#>   Sepal.Length Sepal.Width Petal.Length Petal.Width
#> 1          5.1         3.5          1.4         0.2
iris[1,] %>% dplyr::select(dplyr::matches("."))
#>   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
#> 1          5.1         3.5          1.4         0.2  setosa
purrr::set_names(iris[1,], paste0("a", 1:5)) %>%
  dplyr::select(dplyr::num_range(prefix = "a", range = 2:4, width = 1))
#>    a2  a3  a4
#> 1 3.5 1.4 0.2
iris[1, ] %>% dplyr::select(dplyr::one_of("Sepal.Width", "Species"))
#>   Sepal.Width Species
#> 1         3.5  setosa
#   ____________________________________________________________________________
```

**Task:** Split a column (number) into one or more new columns.


```r
iris %>% dplyr::transmute(Sepal.Length = as.character(Sepal.Length)) %>%
  tidyr::separate(col = Sepal.Length, 
                  into = c("Sep1", "Sep2"),
                  sep = "\\.",
                  remove = FALSE) %>% 
  head(1)
```

**Task:** Combine/unite one or more columns into one.

```r
iris[1,] %>% tidyr::unite(new_col, Sepal.Width, Species, sep = "", remove = FALSE)
#>   Sepal.Length   new_col Sepal.Width Petal.Length Petal.Width Species
#> 1          5.1 3.5setosa         3.5          1.4         0.2  setosa
iris[1,] %>% dplyr::mutate(new_col = stringr::str_c(Sepal.Width, Species))
#>   Sepal.Length Sepal.Width Petal.Length Petal.Width Species   new_col
#> 1          5.1         3.5          1.4         0.2  setosa 3.5setosa
#   ____________________________________________________________________________
```


**Task:** Group and summarise a data.frame (#annonymous function).


```r
iris %>% dplyr::group_by(Species) %>% 
  dplyr::summarise(n_rows = n(),
                   dists = n_distinct(Sepal.Width, Sepal.Length),
                   blub = (function(x) mean(x))(Sepal.Width),
                   m = mean(Sepal.Length))
#> # A tibble: 3 × 5
#>      Species n_rows dists  blub     m
#>       <fctr>  <int> <int> <dbl> <dbl>
#> 1     setosa     50    39 3.428 5.006
#> 2 versicolor     50    44 2.770 5.936
#> 3  virginica     50    44 2.974 6.588
```

**Task:** Summarise grouped data by a statistic that returns more than one value.

**Task:** Add summary data directly to the summarised data frame.

## Common use cases

**Task:** Mutate/Change/reorder columns depending on the row number (for example the last row).


```r
iris %>% head(1) %>% dplyr::select(Species, dplyr::everything())
#>   Species Sepal.Length Sepal.Width Petal.Length Petal.Width
#> 1  setosa          5.1         3.5          1.4         0.2
#   ____________________________________________________________________________
setNames(iris[1, ], c("Species", setdiff(names(iris), "Species")))
#>   Species Sepal.Length Sepal.Width Petal.Length Petal.Width
#> 1     5.1          3.5         1.4          0.2      setosa
```

**Task:** Mutate columns depending/conditional on other colums.


```r
# use dplyr::case_when() or dplyr::if_else()
#   ____________________________________________________________________________
# use ifelse
```

**Task** Add an id column.


```r
iris[1:2, ] %>% 
  dplyr::mutate(id = seq_len(n()))
#>   Sepal.Length Sepal.Width Petal.Length Petal.Width Species id
#> 1          5.1         3.5          1.4         0.2  setosa  1
#> 2          4.9         3.0          1.4         0.2  setosa  2
#   ____________________________________________________________________________
df <- data.frame(a = 1:2)
df[["id"]] <- seq_len(nrow(df))
```

**Task** Add a unique identifier regarding some columns (not in order).


```r
iris %>% 
  dplyr::mutate(id = as.integer(factor(stringr::str_c(Species, Sepal.Width))))
#   ____________________________________________________________________________
iris[["id"]] <- as.integer(factor(paste0(iris[["Species"]], iris[["Sepal.Width"]])))
```

**Task** Add a unique identifier regarding all (or some) columns, in order off appearing unique rows.


```r
iris %>% dplyr::mutate(id = iris %>% 
                         purrr::pmap_chr(stringr::str_c, sep = "\t") %>% 
                         factor %>% 
                         forcats::fct_inorder(ordered = TRUE) %>% 
                         as.integer)
#   ____________________________________________________________________________
```

**Task** Work on more than two tables (`Reduce()`)

**Task** Convert rownames of a data frame into first column.

## Wide and Long data

**Task:** Gather/Melt from wide into long format (two columns).


```r
data.frame(abc = sample(letters[1:3], 6, replace = TRUE), 
           r1 = rnorm(6),
           r2 = rnorm(6),
           stringsAsFactors = FALSE) %>% 
  tidyr::gather(key = r, value = random, r1, r2) %>% 
  head(4)
#>   abc  r     random
#> 1   a r1  0.8652943
#> 2   a r1  0.1213840
#> 3   b r1  0.4436479
#> 4   b r1 -1.3253144
#   ____________________________________________________________________________
```

**Task:** Reshape/Spread from long into wide format (two columns).


```r
data.frame(abc = rep(letters[1:4], times = 2),
           r = rep(c("r1", "r2"), each = 4),
           random = rnorm(8),
           stringsAsFactors = FALSE) %>% 
  tidyr::spread(key = abc, value = random) %>% 
  head(1)
#>    r          a        b         c         d
#> 1 r1 -0.1065103 1.711224 0.0521267 -1.395813
#   ____________________________________________________________________________
```

## Joins

**Task:** Join on two columns.

## Tibbles

## Resources

* https://www.r-bloggers.com/lesser-known-dplyr-tricks/
