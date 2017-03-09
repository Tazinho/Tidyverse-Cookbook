
# Dates and times

## Dates

**Task:** Convert a string into a date.


```r
lubridate::ymd("2016-01-01") # also "2016/01/01", "20160101" and "2016:01:01" will work
#> [1] "2016-01-01"
#   ____________________________________________________________________________
as.Date.character("2016-01-01") # also "2016/01/01" will work
#> [1] "2016-01-01"
```

**Task:** Add days (as an integer) to a date.


```r
lubridate::ymd("2016-01-01") + 5
#> [1] "2016-01-06"
#   ____________________________________________________________________________
as.Date.character("2016-01-01") + 5
#> [1] "2016-01-06"
```

**Task:** Generate a (dense) sequence of days/weeks/months/years.


```r
lubridate::ymd("2016-01-01") + lubridate::days(0:3)
#> [1] "2016-01-01" "2016-01-02" "2016-01-03" "2016-01-04"
lubridate::ymd("2016-01-01") + lubridate::weeks(0:3)
#> [1] "2016-01-01" "2016-01-08" "2016-01-15" "2016-01-22"
lubridate::ymd("2016-01-01") + lubridate::month(0:3)
#> [1] "2016-01-01" "2016-01-02" "2016-01-03" "2016-01-04"
lubridate::ymd("2016-01-01") + lubridate::years(0:3)
#> [1] "2016-01-01" "2017-01-01" "2018-01-01" "2019-01-01"
lubridate::ymd("2012-01-01") + lubridate::dyears(1) # adds 365 days also for leap years
#> [1] "2012-12-31"
```

**TASK:** Get the exact difference between two dates in days or other units:


```r
as.double(difftime(lubridate::ymd_hms("2016-01-01 08:04:20"),
                   lubridate::ymd_hms("2016-04-01 12:03:00"),
                   units = "days"))
#> [1] -91.16574
#   ____________________________________________________________________________
```

**Task:** Get the day of a week as an integer or as its name.


```r
lubridate::wday("2016-01-01")
#> [1] 6
lubridate::wday("2016-01-01", label = TRUE)
#> [1] Fri
#> Levels: Sun < Mon < Tues < Wed < Thurs < Fri < Sat
#   ____________________________________________________________________________
```

**Task:** Get the day/month/year from a date.


```r
lubridate::day("2016-01-01")
#> [1] 1
lubridate::week("2016-01-01")
#> [1] 1
lubridate::month("2016-01-01")
#> [1] 1
lubridate::month("2016-01-01", label = TRUE)
#> [1] Jan
#> 12 Levels: Jan < Feb < Mar < Apr < May < Jun < Jul < Aug < Sep < ... < Dec
lubridate::year("2016-01-01")
#> [1] 2016
#   ____________________________________________________________________________
```

**Task:** What happens if dates are not valid?


```r
lubridate::ymd("20160142")
#> Warning: All formats failed to parse. No formats found.
#> [1] NA
jan31 <- ymd("2013-01-31")
jan31 + months(0:11)
#>  [1] "2013-01-31" NA           "2013-03-31" NA           "2013-05-31"
#>  [6] NA           "2013-07-31" "2013-08-31" NA           "2013-10-31"
#> [11] NA           "2013-12-31"
floor_date(jan31, "month") + months(0:11) + days(31)
#>  [1] "2013-02-01" "2013-03-04" "2013-04-01" "2013-05-02" "2013-06-01"
#>  [6] "2013-07-02" "2013-08-01" "2013-09-01" "2013-10-02" "2013-11-01"
#> [11] "2013-12-02" "2014-01-01"
lubridate::`%m+%`(jan31, months(0:11))
#>  [1] "2013-01-31" "2013-02-28" "2013-03-31" "2013-04-30" "2013-05-31"
#>  [6] "2013-06-30" "2013-07-31" "2013-08-31" "2013-09-30" "2013-10-31"
#> [11] "2013-11-30" "2013-12-31"
```

**Task:** Get the last day of a month


```r
last_day <- function(date) {
  lubridate::ceiling_date(date, "month") - lubridate::days(1)
  }
last_day(lubridate::ymd("2016-01-01"))
#> [1] "2016-01-31"
```

**Task:** Check if a year is a leap/non regular year:


```r
lubridate::leap_year(2011)
#> [1] FALSE
#   ____________________________________________________________________________
```

## Dates and Times

**Task:** Subtract dates/date-times.

**Task:** Convert to date-time (and set a specific timezone).


```r
lubridate::ymd_hms("2011-06-04 12:00:00", tz = "Pacific/Auckland")
#> [1] "2011-06-04 12:00:00 NZST"
```

**Task:** Get the second/minute/hour from a date.


```r
lubridate::second(lubridate::ymd_hms("2011-06-04 12:00:00", tz = "Pacific/Auckland"))
#> [1] 0
lubridate::minute(lubridate::ymd_hms("2011-06-04 12:00:00", tz = "Pacific/Auckland"))
#> [1] 0
lubridate::hour(lubridate::ymd_hms("2011-06-04 12:00:00", tz = "Pacific/Auckland"))
#> [1] 12
#   ____________________________________________________________________________
```

**Task:** Change the time depending on a timezone.


```r
meeting <- lubridate::ymd_hms("2011-07-01 09:00:00", tz = "Pacific/Auckland")
lubridate::with_tz(meeting, "America/Chicago")
#> [1] "2011-06-30 16:00:00 CDT"
#   ____________________________________________________________________________
```

## Intervals

**Task:** Create an interval.


```r
int1 <- lubridate::interval(lubridate::ymd_hms("2011-06-04 12:00:00"),
                            lubridate::ymd_hms("2011-08-10 14:00:00")) 
# or
int2 <- lubridate::`%--%`(lubridate::ymd("2011-07-20"),
                          lubridate::ymd("2011-08-31"))
#   ____________________________________________________________________________
```

**Task:** Check if two intervals overlap.


```r
lubridate::int_overlaps(int1, int2)
#> [1] TRUE
#   ____________________________________________________________________________
```

**Task:** What is the intersection of two intervals?


```r
lubridate::intersect(int1, int2)
#> [1] 2011-07-20 UTC--2011-08-10 14:00:00 UTC
#   ____________________________________________________________________________
```

**Task:** What is the setdiff of two intervals (one interval without the other)?


```r
setdiff(int1, int2)
#> [1] 2011-06-04 12:00:00 UTC--2011-07-20 UTC
#   ____________________________________________________________________________
```

**Task:** What is the begin/end of the interval?


```r
lubridate::int_start(int1)
#> [1] "2011-06-04 12:00:00 UTC"
lubridate::int_end(int1)
#> [1] "2011-08-10 14:00:00 UTC"
#   ____________________________________________________________________________
```

**Task:** How to reverse the start and end of an interval?


```r
lubridate::int_flip(int1)
#> [1] 2011-08-10 14:00:00 UTC--2011-06-04 12:00:00 UTC
#   ____________________________________________________________________________
```

**Task:** Add/substract an amount of time to an interval.


```r
lubridate::int_shift(int1, by = seconds(102))
#> [1] 2011-06-04 12:01:42 UTC--2011-08-10 14:01:42 UTC
#   ____________________________________________________________________________
```

**Task:** Check if two intervals are starting or ending at the same time.


```r
lubridate::int_aligns(int1, int2)
#> [1] FALSE
#   ____________________________________________________________________________
```

**Task:** Unite two intevals into one.


```r
lubridate::union(int1, int2)
#> [1] 2011-06-04 12:00:00 UTC--2011-08-31 UTC
#   ____________________________________________________________________________
```

**Task** Check if a date-time lies within an interval.


```r
lubridate::`%within%`(lubridate::ymd_hms("2011-06-06 12:12:12"), int1)
#> [1] TRUE
#   ____________________________________________________________________________
```

**Task:** What are general time intervals available and what are their differences?


```r
# Intervals are specific time spans (because they are tied to specific dates)

# Durations will always supply mathematically precise results, for example
# duration year will always equal 365 days.

# Periods fluctuate the same way the timeline does to give intuitive results.
# This makes them useful for modeling clock times.

lubridate::minutes(2) ## period
#> [1] "2M 0S"
lubridate::dminutes(2) ## duration
#> [1] "120s (~2 minutes)"
```

**Task:** Perform calculations with intervals:


```r
int1 / lubridate::ddays(2)
#> [1] 33.54167
int1 / lubridate::dminutes(1)
#> [1] 96600
int1 %/% months(1)
#> Note: method with signature 'Timespan#Timespan' chosen for function '%/%',
#>  target signature 'Interval#Period'.
#>  "Interval#ANY", "ANY#Period" would also be valid
#> [1] 2
lubridate::as.period(int1)
#> [1] "2m 6d 2H 0M 0S"
lubridate::as.period(int1 %% months(1))
#> [1] "6d 2H 0M 0S"
#   ____________________________________________________________________________
```

## Resources

* [original lubridate paper](https://www.jstatsoft.org/article/view/v040i03)
* lubridate google group
* [development page on github](https://github.com/hadley/lubridate)
* [lubridate vignette](https://cran.r-project.org/web/packages/lubridate/vignettes/lubridate.html)
