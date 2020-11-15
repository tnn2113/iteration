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
    ## -2.43147 -0.83324 -0.09533 -0.14448  0.47823  2.04619

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
    ##  [1] 4.947986 3.770471 6.302076 2.898370 2.631906 4.127580 3.301441 5.296431
    ##  [9] 3.544754 4.638403 3.477957 4.480543 5.229608 2.280779 3.034516 4.642221
    ## [17] 3.565160 3.018643 3.960841 2.042528
    ## 
    ## $b
    ##  [1] -3.5284131 -0.2400650 -6.9432598  6.2108160 -5.9675723  2.0246166
    ##  [7]  0.4727502 -3.6623364  0.5368695 -7.9023744  7.1571649  0.9056083
    ## [13]  1.0778571  5.2577335  1.5073652 -4.0164682  4.0019455  6.7860676
    ## [19]  7.8562844 -2.6397901  3.2333730 -7.6007318  1.6113551  4.0605128
    ## [25]  6.2037898 -2.2851142 -5.3961680  1.4245271  5.4714884 -0.2719591
    ## 
    ## $c
    ##  [1] 10.619434 10.251446  9.704822 10.249461 10.070713  9.526596 10.021342
    ##  [8]  9.864794  9.902628  9.955142  9.631028 10.128782 10.306513  9.690958
    ## [15] 10.151096  9.954402  9.804707  9.842490  9.798839 10.106655  9.932863
    ## [22] 10.011829 10.070877 10.011642  9.649165 10.112648  9.744835  9.806963
    ## [29]  9.855529  9.676339 10.286192  9.894603  9.737585  9.856970  9.981401
    ## [36] 10.217295  9.971307 10.381957  9.998496  9.581932
    ## 
    ## $d
    ##  [1] -1.985404 -1.289536 -2.132328 -2.362427 -2.348981 -2.308604 -3.580162
    ##  [8] -3.033659 -1.937515 -3.522993 -3.758554 -3.379868 -1.066697 -2.509861
    ## [15] -2.950459 -2.959062 -2.647214 -2.537272 -4.053760 -2.378545

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
    ## 1  3.86  1.10

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.512  4.66

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.96 0.237

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.64 0.783

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

``` r
output = map_df(list_norm, mean_and_sd, .id = "input")
```

## List columns

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 4.947986 3.770471 6.302076 2.898370 2.631906 4.127580 3.301441 5.296431
    ##  [9] 3.544754 4.638403 3.477957 4.480543 5.229608 2.280779 3.034516 4.642221
    ## [17] 3.565160 3.018643 3.960841 2.042528
    ## 
    ## $b
    ##  [1] -3.5284131 -0.2400650 -6.9432598  6.2108160 -5.9675723  2.0246166
    ##  [7]  0.4727502 -3.6623364  0.5368695 -7.9023744  7.1571649  0.9056083
    ## [13]  1.0778571  5.2577335  1.5073652 -4.0164682  4.0019455  6.7860676
    ## [19]  7.8562844 -2.6397901  3.2333730 -7.6007318  1.6113551  4.0605128
    ## [25]  6.2037898 -2.2851142 -5.3961680  1.4245271  5.4714884 -0.2719591
    ## 
    ## $c
    ##  [1] 10.619434 10.251446  9.704822 10.249461 10.070713  9.526596 10.021342
    ##  [8]  9.864794  9.902628  9.955142  9.631028 10.128782 10.306513  9.690958
    ## [15] 10.151096  9.954402  9.804707  9.842490  9.798839 10.106655  9.932863
    ## [22] 10.011829 10.070877 10.011642  9.649165 10.112648  9.744835  9.806963
    ## [29]  9.855529  9.676339 10.286192  9.894603  9.737585  9.856970  9.981401
    ## [36] 10.217295  9.971307 10.381957  9.998496  9.581932
    ## 
    ## $d
    ##  [1] -1.985404 -1.289536 -2.132328 -2.362427 -2.348981 -2.308604 -3.580162
    ##  [8] -3.033659 -1.937515 -3.522993 -3.758554 -3.379868 -1.066697 -2.509861
    ## [15] -2.950459 -2.959062 -2.647214 -2.537272 -4.053760 -2.378545

``` r
listcol_df %>% 
  filter(name == "a")
```

    ## # A tibble: 1 x 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Let’s try some operations

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.86  1.10

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.512  4.66

Can I just …map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.86  1.10
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.512  4.66
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.96 0.237
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.64 0.783

So … can I add a list column??

``` r
listcol_df = 
  listcol_df %>% 
  mutate(summary = map(samp, mean_and_sd),
         medians = map(samp, median)
         )
```
