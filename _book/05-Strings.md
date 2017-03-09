
# (PART) Atomics {-} 

# Strings

## Basic string operations

1. **Paste/concatenate strings.**
    
    
    ```r
    stringr::str_c("a", "b", NA, NaN, NULL, factor("lalala"), character(0))
    #> [1] NA
    #   ____________________________________________________________________________
    paste("a", "b", NA, NaN, NULL, factor("lalala"), character(0), sep = "")
    #> [1] "abNANaNlalala"
    ```

1. **Treat `NA`'s as strings (`"NA"`).**
    
    
    ```r
    stringr::str_replace_na(c(NA, NaN, factor("lalala"), character(0)))
    #> [1] "NA"  "NaN" "1"
    # note that the `c` function coerces the factor to its integer representation
    #   ____________________________________________________________________________
    base_replace_na <- function(x) {ifelse(is.na(x), "NA", x)}
    base_replace_na(c(NA, NaN, factor("lalala"), character(0)))
    #> [1] "NA"  "NaN" "1"
    ```

1. **Count the number of letters in a string.**

    
    ```r
    stringr::str_length(c("four", NA, NaN, NULL, factor("lalala")))
    #> [1]  4 NA  3  1
    #   ____________________________________________________________________________
    nchar(c("four", NA, NaN, NULL, factor("lalala")))
    #> [1]  4 NA  3  1
    ```

1. **Return the last letter of a string, subset a string.**

    
    ```r
    stringr::str_sub(c("five", "six"), -1)
    #> [1] "e" "x"
    #   ____________________________________________________________________________
    substr(c("five", "six"), 
           start = nchar(c("five", "six")), 
           stop = nchar(c("five", "six")))
    #> [1] "e" "x"
    ```
    
1. **Repeat/replicate/duplicate a string several times.**

    
    ```r
    stringr::str_dup("_a_", 5)
    #> [1] "_a__a__a__a__a_"
    #   ____________________________________________________________________________
    paste(rep("_a_", 5), collapse = "")
    #> [1] "_a__a__a__a__a_"
    ```
    
1. **Remove/trim spaces at the beginning (leading) and/or end (trailing) of a string.**

    
    ```r
    stringr::str_trim(c("  2left_1right ", "   "), side = c("left"))
    #> [1] "2left_1right " ""
    #   ____________________________________________________________________________
    # ^\\s+|\\s+$ is the regex for one or more spaces in the beginning
    # or one or more spaces at the end
    gsub("^\\s+", "", c("  2left_1right ", "   "))
    #> [1] "2left_1right " ""
    ```
    
1. **Fill/pad spaces (or other characters) before and/or after a string.**

    
    ```r
    stringr::str_pad(c("lalala", ""), width = 7)
    #> [1] " lalala" "       "
    #   ____________________________________________________________________________
    unname(
      vapply(c("lalala", ""), 
             function(x) if(nchar(x) < 7) {
               paste0(rep(" ", 7 - nchar(x)), x, collapse = "")
               } else {x},
             character(1)
             )
      )
    #> [1] " lalala" "       "
    ```
    
1. **Sort strings for example, by a different local alphabetic order.**

    
    ```r
    # The vowels come before the consonants in Hawaiian
    stringr::str_sort(letters[1:10], locale = "haw")
    #>  [1] "a" "e" "i" "b" "c" "d" "f" "g" "h" "j"
    #   ____________________________________________________________________________
    # one can use `sort()`, but one has to know the specific locale
    ```
    
1. **Return the order of stringelements within a specific alphabet**

    
    ```r
    # The vowels come before the consonants in Hawaiian
    stringr::str_order(letters[1:10], locale = "haw")
    #>  [1]  1  5  9  2  3  4  6  7  8 10
    #   ____________________________________________________________________________
    # one can use `sort()`, but one has to know the specific locale
    ```
    
1. **Change the encoding of a string**

    
    ```r
    # Example from encoding?stringi::stringi
    x <- rawToChar(as.raw(177))
    x
    #> [1] "±"
    stringr::str_conv(x, "ISO-8859-2") # Polish "a with ogonek", (run this line in RStudio)
    #> [1] "a"
    stringr::str_conv(x, "ISO-8859-1") # Plus-minus
    #> [1] "±"
    #   ____________________________________________________________________________
    ```
    
1. **Return the first two words of a string**

    
    ```r
    stringr::word("Hello, how are you?", 1, 2)
    #> [1] "Hello, how"
    #   ____________________________________________________________________________
    ```
    
1. **Format a string (change width, indentaion, ...).**

    
    ```r
    cat(stringr::str_wrap("Hello, how are you?", width = 10, indent = 0, exdent = 2))
    #> Hello, how
    #>   are you?
    #   ____________________________________________________________________________
    ```

## Pattern matching

1. **Does a string contain a specific sequence of characters?**

    
    ```r
    stringr::str_detect(c("my number is 110", "call 911"), "\\d{3}")
    #> [1] TRUE TRUE
    #   ____________________________________________________________________________
    grepl("\\d{3}", c("my number is 110", "call 911"))
    #> [1] TRUE TRUE
    ```
   
1. **Return every string that contains a specific sequence of characters?**

    
    ```r
    stringr::str_subset(c("my number is 110", "call 911"), "\\d{3}")
    #> [1] "my number is 110" "call 911"
    #   ____________________________________________________________________________
    grep("\\d{3}", c("my number is 110", "call 911"), value = TRUE)
    #> [1] "my number is 110" "call 911"
    ```

1. **Return the (first) start and end index of substrings within a string?**

    
    ```r
    # use stringr::str_locate_all to get a matrix of all start and end indexes
    stringr::str_locate(c("my 112 number is 110", "call 911"), c("\\d{3}"))
    #>      start end
    #> [1,]     4   6
    #> [2,]     6   8
    #   ____________________________________________________________________________
    # Use gregexpr to construct something similar to str_locate_all
    matches <- regexpr("\\d{3}", c("my 112 number is 110", "call 911"))
    
    # formatting
    match_lengths <- attr(matches, "match.length")
    attributes(matches) <- NULL
    match_indices <- matrix(c(matches, matches + match_lengths - 1), ncol = 2)
    colnames(match_indices) <- c("start", "end")
    
    # output
    match_indices
    #>      start end
    #> [1,]     4   6
    #> [2,]     6   8
    ```

1. **Return the (first) matched substring within a string?**

    
    ```r
    # use stringr::str_extract_all to get a matrix of all matched substrings
    stringr::str_extract(c("my 112 number is 110", "call 911"), c("\\d{3}"))
    #> [1] "112" "911"
    #   ____________________________________________________________________________
    # combine `substr()` with match_indices from above
    ```

1. **Return the matching strings and matched capture groups.**

    
    ```r
    # the example from the stringr vignette
    strings <- c(
      "apple",
      "219 733 8965", 
      "329-293-8753", 
      "Work: 579-499-7527; Home: 543.355.3679"
      )
    phone <- "([2-9][0-9]{2})[- .]([0-9]{3})[- .]([0-9]{4})"
    
    stringr::str_match(strings, phone)
    #>      [,1]           [,2]  [,3]  [,4]  
    #> [1,] NA             NA    NA    NA    
    #> [2,] "219 733 8965" "219" "733" "8965"
    #> [3,] "329-293-8753" "329" "293" "8753"
    #> [4,] "579-499-7527" "579" "499" "7527"
    # Use stringr::str_match_all for list return
    #   ____________________________________________________________________________
    # `regmatches()`
    ```
    
1. **Replace/substitute the first matching subtstring within a string.**

    
    ```r
    stringr::str_replace(c("my 112 number is 110", "call 911"),
                         c("\\d{1}(\\d{2})"), "\\1\\1")
    #> [1] "my 1212 number is 110" "call 1111"
    #   ____________________________________________________________________________
    sub(c("\\d{1}(\\d{2})"), "\\1\\1", c("my 112 number is 110", "call 911"))
    #> [1] "my 1212 number is 110" "call 1111"
    ```
    
1. **Replace/substitute all matching subtstrings within a string.**

    
    ```r
    stringr::str_replace_all(c("my 112 number is 110", "call 911"),
                             c("\\d{1}(\\d{2})"), "\\1\\1")
    #> [1] "my 1212 number is 1010" "call 1111"
    #   ____________________________________________________________________________
    gsub(c("\\d{1}(\\d{2})"), "\\1\\1", c("my 112 number is 110", "call 911"))
    #> [1] "my 1212 number is 1010" "call 1111"
    ```
    
1. **Split strings by any pattern into different (optionally n) pieces.**

    
    ```r
    # returns a matrix and padds with empty strings, when n is greater than the
    # number of pieces
    stringr::str_split_fixed(c("lalala lala la",
                               "blabla lala la"),
                             c(" ", "b"), 5)
    #>      [,1]     [,2]   [,3]         [,4] [,5]
    #> [1,] "lalala" "lala" "la"         ""   ""  
    #> [2,] ""       "la"   "la lala la" ""   ""
    # returns a list
    stringr::str_split(c("lalala lala la",
                         "blabla lala la"),
                       c(" ", "b"), 5)
    #> [[1]]
    #> [1] "lalala" "lala"   "la"    
    #> 
    #> [[2]]
    #> [1] ""           "la"         "la lala la"
    #   ____________________________________________________________________________
    # returns a list, but does not have an n argument (however, it shouldn't be
    # too hard to create one).
    strsplit(c("lalala lala la",
               "blabla lala la"),
             c(" ", "b"))
    #> [[1]]
    #> [1] "lalala" "lala"   "la"    
    #> 
    #> [[2]]
    #> [1] ""           "la"         "la lala la"
    ```

1. **Set all characters to lower/upper case.**

    
    ```r
    stringr::str_to_lower("aBcDe")
    #> [1] "abcde"
    stringr::str_to_upper("aBcDe")
    #> [1] "ABCDE"
    #   ____________________________________________________________________________
    tolower("aBcDe")
    #> [1] "abcde"
    toupper("aBcDe")
    #> [1] "ABCDE"
    ```

1. **Truncate a string.**

    
    ```r
    stringr::str_trunc("123456789", width = 7, side = "right", ellipsis = "")
    #> [1] "1234567"
    #   ____________________________________________________________________________
    ```

### Regular expressions

1. **Where can I learn about regular expressions?**

  *  `?stringr::modifiers` for special regular expression functionality of the stringr package.
  *  [reference sheet](http://www.regular-expressions.info/reference.html)
  *  [interactively test](http://regexr.com/)
  *  [Build a regular expressin](http://www.txt2re.com/)
  *  Try the `str_view()` function to see, which substring a regex matches
  
    
    ```r
    stringr::str_view(c("abc", "a.c_blabla", "bef"), "a\\.c")
    ```
    
    <!--html_preserve--><div id="htmlwidget-bf0be613e310b474aaa0" style="width:960px;height:auto;" class="str_view html-widget"></div>
    <script type="application/json" data-for="htmlwidget-bf0be613e310b474aaa0">{"x":{"html":"<ul>\n  <li>abc\u003c/li>\n  <li><span class='match'>a.c\u003c/span>_blabla\u003c/li>\n  <li>bef\u003c/li>\n\u003c/ul>"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

1. **How to test regular expressions?**

  > When writing regular expressions, I strongly recommend generating a list of positive (pattern should match) and negative (pattern shouldn’t match) test cases to ensure that you are matching the correct components.
  
### Modifiers

1.  **Count the number of words within a string.**

    
    ```r
    words <- c("These are some words. 
               Some more words.")
    stringr::str_count(words, stringr::boundary("word"))
    #> [1] 7
    #   ____________________________________________________________________________
    ```
    
1.  **Count the number of lines within a string.**

    
    ```r
    stringr::str_count(words, stringr::boundary("line_break")) -
    stringr::str_count(words, stringr::boundary("word"))
    #> [1] 1
    #   ____________________________________________________________________________
    ```

1.  **Count the number of sentences within a string.**

    
    ```r
    stringr::str_count(words, stringr::boundary("sentence"))
    #> [1] 2
    #   ____________________________________________________________________________
    ```

1.  **Split a string by characters.**

    
    ```r
    stringr::str_split(words, stringr::boundary("character"))
    #> [[1]]
    #>  [1] "T"  "h"  "e"  "s"  "e"  " "  "a"  "r"  "e"  " "  "s"  "o"  "m"  "e" 
    #> [15] " "  "w"  "o"  "r"  "d"  "s"  "."  " "  "\n" " "  " "  " "  " "  " " 
    #> [29] " "  " "  " "  " "  " "  " "  "S"  "o"  "m"  "e"  " "  "m"  "o"  "r" 
    #> [43] "e"  " "  "w"  "o"  "r"  "d"  "s"  "."
    #   ____________________________________________________________________________
    strsplit(words, "")
    #> [[1]]
    #>  [1] "T"  "h"  "e"  "s"  "e"  " "  "a"  "r"  "e"  " "  "s"  "o"  "m"  "e" 
    #> [15] " "  "w"  "o"  "r"  "d"  "s"  "."  " "  "\n" " "  " "  " "  " "  " " 
    #> [29] " "  " "  " "  " "  " "  " "  "S"  "o"  "m"  "e"  " "  "m"  "o"  "r" 
    #> [43] "e"  " "  "w"  "o"  "r"  "d"  "s"  "."
    ```

## List output

1. **How to work with string functions, that return lists?**

## Resources

* [stringr vignette](https://cran.r-project.org/web/packages/stringr/vignettes/stringr.html)
* [Regular Expressions in R - Cheatsheet]https://www.rstudio.com/wp-content/uploads/2016/09/RegExCheatsheet.pdf
