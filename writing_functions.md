Writing functions
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.17601612  0.88662927 -0.46509183 -1.67427572 -1.12925724 -1.51526312
    ##  [7]  0.51818958  0.70765127 -0.08475115  0.75778524 -1.62389207 -0.23731609
    ## [13]  2.35505174  0.13146290 -1.22384843 -0.19595200  0.04587783  0.12989103
    ## [19]  0.84060110  0.86899438  0.92590006 -0.58455837  1.00665339 -0.95374312
    ## [25] -0.30162089 -0.54387165  1.93232789  0.01203239 -1.11059298  0.70100273

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

    ##  [1] -0.17601612  0.88662927 -0.46509183 -1.67427572 -1.12925724 -1.51526312
    ##  [7]  0.51818958  0.70765127 -0.08475115  0.75778524 -1.62389207 -0.23731609
    ## [13]  2.35505174  0.13146290 -1.22384843 -0.19595200  0.04587783  0.12989103
    ## [19]  0.84060110  0.86899438  0.92590006 -0.58455837  1.00665339 -0.95374312
    ## [25] -0.30162089 -0.54387165  1.93232789  0.01203239 -1.11059298  0.70100273

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
    ## 1  2.81  3.91

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
    ## 1  4.19  3.09

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

sim_mean_sd(100, 6, 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.76  2.77
