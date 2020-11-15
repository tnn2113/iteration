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
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -1.91891 -0.87803 -0.22976 -0.05103  0.72109  3.09394

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
    ##  [1] 3.847520 5.521624 5.352287 5.006629 4.536795 5.176031 4.412065 3.785495
    ##  [9] 4.236358 4.545072 3.940414 4.576806 3.415795 4.205627 4.348826 3.707689
    ## [17] 3.225637 5.317801 3.618406 4.997881
    ## 
    ## $b
    ##  [1] -1.4904995 12.7158673  6.3491831 -4.6956130  4.6902983  3.0618674
    ##  [7] -0.8070603  6.9528268  2.9873177 -0.4116985  0.2263271 -1.6325715
    ## [13] -8.6060631  1.3702784  0.9872479 -0.5481416 -6.5616877 -3.9265270
    ## [19]  4.4457424 -5.2203809 -2.6145223  0.9058247  1.5109651  2.4159629
    ## [25] -1.8266799 -4.8700498 -7.9605302  0.8038852  3.8403879 -9.0702550
    ## 
    ## $c
    ##  [1]  9.943384 10.123273  9.982172 10.081813  9.706642  9.613203  9.989813
    ##  [8]  9.635252  9.632626 10.166736  9.658552 10.031543 10.359808 10.243431
    ## [15] 10.041068  9.986181  9.644711  9.735933  9.999695  9.830531 10.020974
    ## [22]  9.641100 10.085351 10.032673  9.800079 10.226502  9.806329 10.169849
    ## [29] 10.341421  9.669942  9.874030  9.959638 10.318005 10.023252 10.112562
    ## [36]  9.858499  9.931545 10.060576  9.795401 10.079532
    ## 
    ## $d
    ##  [1] -4.2856700 -2.6447847 -2.7367913 -1.9260594 -2.6415235 -4.0572244
    ##  [7] -3.2635259 -1.6036281 -3.8820460 -3.1036057 -4.2975525 -4.6839946
    ## [13] -0.4517432 -4.7455874 -2.6405526 -1.7596093 -4.3674698 -2.6803426
    ## [19] -3.9495132 -2.4140386

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
    ## 1  4.39 0.681

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.233  4.92

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.96 0.211

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.11  1.16

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for(i in 1:4) {
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```
