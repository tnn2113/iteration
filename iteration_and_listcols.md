Iteration and listcols
================

\#\#Lists

You can put anything in a list.

``` r
l = list(
  vec_numeric = 5:8,
  vec_logical = c(TRUE, TRUE, TRUE, FALSE),
  mat = matrix(1:8, nrow = 2, ncol = 4),
  summary = summary(rnorm(100))
)
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE  TRUE FALSE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ## -2.394717 -0.833323  0.003324 -0.019799  0.695544  2.052901

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## `for` loop

Create a new list.

``` r
list_norm =
  list(
    a = rnorm(20, mean = 4, sd = 1),
    b = rnorm(30, mean = 0, sd = 5),
    c = rnorm(40, mean = 10, sd = .2),
    d = rnorm(20, mean = -3, sd = 1)
  )
```

``` r
list_norm
```

    ## $a
    ##  [1] 4.409565 4.928509 3.625055 3.166320 4.692627 2.693128 3.018812 3.155030
    ##  [9] 3.360432 2.945424 1.995202 4.423139 3.118809 3.035022 4.489253 3.475383
    ## [17] 5.207920 5.580188 3.926659 2.951928
    ## 
    ## $b
    ##  [1]  2.78049400  0.06221507  2.80470448  0.96455518 -2.33507959  0.65999422
    ##  [7]  1.99939087  5.87312855 -0.34955921 -2.29299210  7.92999167 -2.47261931
    ## [13] -7.58161543 -5.54057898 -1.23717797 -4.00859408 -3.16136344 -7.49598807
    ## [19] -2.08380904 -8.87640305  7.34901531 -2.74562332 -4.62388406  8.01374788
    ## [25]  6.58795984 -0.61260224 -3.53276719 -2.17764667 -7.66954412 -3.46979638
    ## 
    ## $c
    ##  [1]  9.912981 10.195898 10.081896  9.466364 10.001337  9.749018  9.673445
    ##  [8] 10.015759 10.289039  9.976291 10.085997  9.847290 10.144395 10.438674
    ## [15]  9.896204 10.171794  9.994822 10.179967  9.988527 10.008631  9.743386
    ## [22]  9.603292  9.693764  9.860629  9.879052 10.170701  9.465758  9.694869
    ## [29]  9.997328  9.971673  9.912297 10.172424 10.197675  9.853047 10.152864
    ## [36]  9.914335 10.126113  9.716408  9.750528  9.812009
    ## 
    ## $d
    ##  [1] -3.150066 -3.431166 -3.030559 -4.006428 -2.155875 -1.527942 -3.467754
    ##  [8] -2.138337 -2.048285 -3.668022 -2.629328 -4.004218 -2.579128 -2.526668
    ## [15] -2.670150 -3.759580 -2.467904 -3.233306 -5.346504 -2.319701

Pause and get my old function.

``` r
mean_and_sd = function(x){
  
  if(!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if(length(x) < 3){
    stop("Input must have at least three numbers")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
  
}
```

I can apply that function to each list element.

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.71 0.947

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.908  4.71

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.95 0.219

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.01 0.887

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for(i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```

## Let’s try map\!

This code will produce the same output as the for loop above

``` r
output = map(list_norm, mean_and_sd)
```

what if you want a different function..?

``` r
output = map(list_norm, median)
```
