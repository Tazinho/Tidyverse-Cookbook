
# (PART) Acting on common data structures {-} 

# Data frames and tibbles


## Formatting
**Task:** Change column names to lower case.

**BASE:**

```r
tolower(names(iris))
#> [1] "sepal.length" "sepal.width"  "petal.length" "petal.width" 
#> [5] "species"
```

**Task:** Change column names from camelCase or CamelCase to snake_case and reversed.

## Basic operations and related stuff

**Task:** Select columns from a data.frame.

**TDVS:** `iris %>% select(Sepal.Width, Species)`

**BASE:** `iris[, c("Sepal.Width", "Species")]`

**Task:** Filter/Subset a data.frame.

**TDVS:** `iris %>% filter(Species == "versicolor" & Sepal.Width < 3)`

**BASE:** `iris[iris[, "Species"] == "versicolor" & iris[, "Sepal.Width"] < 3]`

**Task:** Mutate/transform/calculate new columns of a data.frame.

### Arrange/order a data.frame.
#### TDVS

```r
dplyr::arrange(iris, dplyr::desc(Sepal.Length))
#>     Sepal.Length Sepal.Width Petal.Length Petal.Width    Species
#> 1            7.9         3.8          6.4         2.0  virginica
#> 2            7.7         3.8          6.7         2.2  virginica
#> 3            7.7         2.6          6.9         2.3  virginica
#> 4            7.7         2.8          6.7         2.0  virginica
#> 5            7.7         3.0          6.1         2.3  virginica
#> 6            7.6         3.0          6.6         2.1  virginica
#> 7            7.4         2.8          6.1         1.9  virginica
#> 8            7.3         2.9          6.3         1.8  virginica
#> 9            7.2         3.6          6.1         2.5  virginica
#> 10           7.2         3.2          6.0         1.8  virginica
#> 11           7.2         3.0          5.8         1.6  virginica
#> 12           7.1         3.0          5.9         2.1  virginica
#> 13           7.0         3.2          4.7         1.4 versicolor
#> 14           6.9         3.1          4.9         1.5 versicolor
#> 15           6.9         3.2          5.7         2.3  virginica
#> 16           6.9         3.1          5.4         2.1  virginica
#> 17           6.9         3.1          5.1         2.3  virginica
#> 18           6.8         2.8          4.8         1.4 versicolor
#> 19           6.8         3.0          5.5         2.1  virginica
#> 20           6.8         3.2          5.9         2.3  virginica
#> 21           6.7         3.1          4.4         1.4 versicolor
#> 22           6.7         3.0          5.0         1.7 versicolor
#> 23           6.7         3.1          4.7         1.5 versicolor
#> 24           6.7         2.5          5.8         1.8  virginica
#> 25           6.7         3.3          5.7         2.1  virginica
#> 26           6.7         3.1          5.6         2.4  virginica
#> 27           6.7         3.3          5.7         2.5  virginica
#> 28           6.7         3.0          5.2         2.3  virginica
#> 29           6.6         2.9          4.6         1.3 versicolor
#> 30           6.6         3.0          4.4         1.4 versicolor
#> 31           6.5         2.8          4.6         1.5 versicolor
#> 32           6.5         3.0          5.8         2.2  virginica
#> 33           6.5         3.2          5.1         2.0  virginica
#> 34           6.5         3.0          5.5         1.8  virginica
#> 35           6.5         3.0          5.2         2.0  virginica
#> 36           6.4         3.2          4.5         1.5 versicolor
#> 37           6.4         2.9          4.3         1.3 versicolor
#> 38           6.4         2.7          5.3         1.9  virginica
#> 39           6.4         3.2          5.3         2.3  virginica
#> 40           6.4         2.8          5.6         2.1  virginica
#> 41           6.4         2.8          5.6         2.2  virginica
#> 42           6.4         3.1          5.5         1.8  virginica
#> 43           6.3         3.3          4.7         1.6 versicolor
#> 44           6.3         2.5          4.9         1.5 versicolor
#> 45           6.3         2.3          4.4         1.3 versicolor
#> 46           6.3         3.3          6.0         2.5  virginica
#> 47           6.3         2.9          5.6         1.8  virginica
#> 48           6.3         2.7          4.9         1.8  virginica
#> 49           6.3         2.8          5.1         1.5  virginica
#> 50           6.3         3.4          5.6         2.4  virginica
#> 51           6.3         2.5          5.0         1.9  virginica
#> 52           6.2         2.2          4.5         1.5 versicolor
#> 53           6.2         2.9          4.3         1.3 versicolor
#> 54           6.2         2.8          4.8         1.8  virginica
#> 55           6.2         3.4          5.4         2.3  virginica
#> 56           6.1         2.9          4.7         1.4 versicolor
#> 57           6.1         2.8          4.0         1.3 versicolor
#> 58           6.1         2.8          4.7         1.2 versicolor
#> 59           6.1         3.0          4.6         1.4 versicolor
#> 60           6.1         3.0          4.9         1.8  virginica
#> 61           6.1         2.6          5.6         1.4  virginica
#> 62           6.0         2.2          4.0         1.0 versicolor
#> 63           6.0         2.9          4.5         1.5 versicolor
#> 64           6.0         2.7          5.1         1.6 versicolor
#> 65           6.0         3.4          4.5         1.6 versicolor
#> 66           6.0         2.2          5.0         1.5  virginica
#> 67           6.0         3.0          4.8         1.8  virginica
#> 68           5.9         3.0          4.2         1.5 versicolor
#> 69           5.9         3.2          4.8         1.8 versicolor
#> 70           5.9         3.0          5.1         1.8  virginica
#> 71           5.8         4.0          1.2         0.2     setosa
#> 72           5.8         2.7          4.1         1.0 versicolor
#> 73           5.8         2.7          3.9         1.2 versicolor
#> 74           5.8         2.6          4.0         1.2 versicolor
#> 75           5.8         2.7          5.1         1.9  virginica
#> 76           5.8         2.8          5.1         2.4  virginica
#> 77           5.8         2.7          5.1         1.9  virginica
#> 78           5.7         4.4          1.5         0.4     setosa
#> 79           5.7         3.8          1.7         0.3     setosa
#> 80           5.7         2.8          4.5         1.3 versicolor
#> 81           5.7         2.6          3.5         1.0 versicolor
#> 82           5.7         3.0          4.2         1.2 versicolor
#> 83           5.7         2.9          4.2         1.3 versicolor
#> 84           5.7         2.8          4.1         1.3 versicolor
#> 85           5.7         2.5          5.0         2.0  virginica
#> 86           5.6         2.9          3.6         1.3 versicolor
#> 87           5.6         3.0          4.5         1.5 versicolor
#> 88           5.6         2.5          3.9         1.1 versicolor
#> 89           5.6         3.0          4.1         1.3 versicolor
#> 90           5.6         2.7          4.2         1.3 versicolor
#> 91           5.6         2.8          4.9         2.0  virginica
#> 92           5.5         4.2          1.4         0.2     setosa
#> 93           5.5         3.5          1.3         0.2     setosa
#> 94           5.5         2.3          4.0         1.3 versicolor
#> 95           5.5         2.4          3.8         1.1 versicolor
#> 96           5.5         2.4          3.7         1.0 versicolor
#> 97           5.5         2.5          4.0         1.3 versicolor
#> 98           5.5         2.6          4.4         1.2 versicolor
#> 99           5.4         3.9          1.7         0.4     setosa
#> 100          5.4         3.7          1.5         0.2     setosa
#> 101          5.4         3.9          1.3         0.4     setosa
#> 102          5.4         3.4          1.7         0.2     setosa
#> 103          5.4         3.4          1.5         0.4     setosa
#> 104          5.4         3.0          4.5         1.5 versicolor
#> 105          5.3         3.7          1.5         0.2     setosa
#> 106          5.2         3.5          1.5         0.2     setosa
#> 107          5.2         3.4          1.4         0.2     setosa
#> 108          5.2         4.1          1.5         0.1     setosa
#> 109          5.2         2.7          3.9         1.4 versicolor
#> 110          5.1         3.5          1.4         0.2     setosa
#> 111          5.1         3.5          1.4         0.3     setosa
#> 112          5.1         3.8          1.5         0.3     setosa
#> 113          5.1         3.7          1.5         0.4     setosa
#> 114          5.1         3.3          1.7         0.5     setosa
#> 115          5.1         3.4          1.5         0.2     setosa
#> 116          5.1         3.8          1.9         0.4     setosa
#> 117          5.1         3.8          1.6         0.2     setosa
#> 118          5.1         2.5          3.0         1.1 versicolor
#> 119          5.0         3.6          1.4         0.2     setosa
#> 120          5.0         3.4          1.5         0.2     setosa
#> 121          5.0         3.0          1.6         0.2     setosa
#> 122          5.0         3.4          1.6         0.4     setosa
#> 123          5.0         3.2          1.2         0.2     setosa
#> 124          5.0         3.5          1.3         0.3     setosa
#> 125          5.0         3.5          1.6         0.6     setosa
#> 126          5.0         3.3          1.4         0.2     setosa
#> 127          5.0         2.0          3.5         1.0 versicolor
#> 128          5.0         2.3          3.3         1.0 versicolor
#> 129          4.9         3.0          1.4         0.2     setosa
#> 130          4.9         3.1          1.5         0.1     setosa
#> 131          4.9         3.1          1.5         0.2     setosa
#> 132          4.9         3.6          1.4         0.1     setosa
#> 133          4.9         2.4          3.3         1.0 versicolor
#> 134          4.9         2.5          4.5         1.7  virginica
#> 135          4.8         3.4          1.6         0.2     setosa
#> 136          4.8         3.0          1.4         0.1     setosa
#> 137          4.8         3.4          1.9         0.2     setosa
#> 138          4.8         3.1          1.6         0.2     setosa
#> 139          4.8         3.0          1.4         0.3     setosa
#> 140          4.7         3.2          1.3         0.2     setosa
#> 141          4.7         3.2          1.6         0.2     setosa
#> 142          4.6         3.1          1.5         0.2     setosa
#> 143          4.6         3.4          1.4         0.3     setosa
#> 144          4.6         3.6          1.0         0.2     setosa
#> 145          4.6         3.2          1.4         0.2     setosa
#> 146          4.5         2.3          1.3         0.3     setosa
#> 147          4.4         2.9          1.4         0.2     setosa
#> 148          4.4         3.0          1.3         0.2     setosa
#> 149          4.4         3.2          1.3         0.2     setosa
#> 150          4.3         3.0          1.1         0.1     setosa
```

#### BASE

```r
iris[order(iris["Sepal.Length", ], decreasing = TRUE), ]
#>   Sepal.Length Sepal.Width Petal.Length Petal.Width Species
#> 1          5.1         3.5          1.4         0.2  setosa
#> 2          4.9         3.0          1.4         0.2  setosa
#> 3          4.7         3.2          1.3         0.2  setosa
#> 4          4.6         3.1          1.5         0.2  setosa
#> 5          5.0         3.6          1.4         0.2  setosa
```


**Task:** Split a column into one or more new columns.

**Task:** Combine/unite one or more columns into one.

**Task:** Group and summarise a data.frame.

## Common use cases

**Task:** Mutate/Change columns depending on the row number (for example the last row).

**TDVS:** `mutate(if_else(row_number() == n(), 1, 0))`

**Task** Work on more than two tables (`Reduce()`)

**Task** Convert rownames of a data frame into first column.

## Wide and Long data

**Task:** Gather/Melt from wide into long format (two columns).

**Task:** Reshape/Spread from long into wide format (two columns).

## Joins

**Task:** Join on two columns.

## Measures

**Task:** Summarise grouped data by a statistic that returns more than one value.

## Tibbles

## Resources
