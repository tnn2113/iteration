Writing functions
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.11631613  1.42118233 -0.79140224 -0.50542536  1.03929016  1.17138825
    ##  [7] -0.93083813 -0.47769674  1.16497442 -1.29706813 -0.92341025 -2.34923635
    ## [13]  1.33568470 -0.22236801  1.86267723 -0.14171250  0.19552429 -0.79781252
    ## [19] -0.62799201  0.57451884  0.87448119 -0.12623377  0.15627686 -0.03117287
    ## [25]  0.28802237  1.20913506 -0.57079794 -1.44001596 -0.99327575  0.81698672

I want a function to compute z-score

``` r
z_scores = function(x){
  
  if(!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if(length(x) < 3){
    stop("Input must have at least three numbers")
  }
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
  
}

z_scores(x_vec)
```

    ##  [1]  0.11631613  1.42118233 -0.79140224 -0.50542536  1.03929016  1.17138825
    ##  [7] -0.93083813 -0.47769674  1.16497442 -1.29706813 -0.92341025 -2.34923635
    ## [13]  1.33568470 -0.22236801  1.86267723 -0.14171250  0.19552429 -0.79781252
    ## [19] -0.62799201  0.57451884  0.87448119 -0.12623377  0.15627686 -0.03117287
    ## [25]  0.28802237  1.20913506 -0.57079794 -1.44001596 -0.99327575  0.81698672

Try my function on some other things. These should give errors.

``` r
z_scores(3)
```

    ## Error in z_scores(3): Input must have at least three numbers

``` r
z_scores("my name is Tu")
```

    ## Error in z_scores("my name is Tu"): Input must be numeric

``` r
z_scores(mtcars)
```

    ## Error in z_scores(mtcars): Input must be numeric

``` r
z_scores(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Input must be numeric
