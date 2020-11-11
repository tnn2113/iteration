Writing functions
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -1.66578818 -0.62873488  0.51173935 -0.99216924  1.10098532  0.36659245
    ##  [7] -0.61762155  1.44807353  1.55859353 -0.52723569  0.33977768 -0.33421384
    ## [13] -0.29079802  0.05151310 -0.67985973  0.97934695  1.08856724  1.55024388
    ## [19] -0.65677327 -0.26202304 -1.13477888 -0.26825047 -0.70409310  1.38017219
    ## [25]  0.05322818 -1.38412729  0.13872754 -2.09238764  0.22139253  1.44990138

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

    ##  [1] -1.66578818 -0.62873488  0.51173935 -0.99216924  1.10098532  0.36659245
    ##  [7] -0.61762155  1.44807353  1.55859353 -0.52723569  0.33977768 -0.33421384
    ## [13] -0.29079802  0.05151310 -0.67985973  0.97934695  1.08856724  1.55024388
    ## [19] -0.65677327 -0.26202304 -1.13477888 -0.26825047 -0.70409310  1.38017219
    ## [25]  0.05322818 -1.38412729  0.13872754 -2.09238764  0.22139253  1.44990138

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

## Multiole outputs

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

Check that the function works.

``` r
x_vec = rnorm(100, mean = 3, sd = 4)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.90  4.20

## Multiple inputs

``` r
sim_data = 
  tibble(
    x = rnorm(n = 100, mean = 4, sd = 3)
  )

sim_data %>% 
  summarise(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.44  2.80

``` r
sim_mean_sd = function(sample_size, mu, signma) {
  sim_data = 
    tibble(
    x = rnorm(n = sample_size, mean = mu, sd = signma)
  )

sim_data %>% 
  summarise(
    mean = mean(x),
    sd = sd(x)
  )
}

sim_mean_sd(sample_size = 100, mu = 6, signma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.12  2.80
