Simulations
================

## Let’s simulate something.

I have a function

``` r
sim_mean_sd = function(sample_size, mu = 3, sigma = 4) {
  
  sim_data = 
    tibble(
    x = rnorm(n = sample_size, mean = mu, sd = sigma)
  )

sim_data %>% 
  summarise(
    mean = mean(x),
    sd = sd(x)
  )

}
```

I can “simulate” by running this line

``` r
sim_mean_sd(30)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.70  4.61

## Let’s simulate a lot

Let’s atart with a for loop.

``` r
output = vector("list", length = 100)

for (i in 1:100) {
  
  output[[i]] = sim_mean_sd(sample_size = 30)
  
}

bind_rows(output)
```

    ## # A tibble: 100 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ##  1  3.19  4.61
    ##  2  2.87  3.63
    ##  3  2.29  3.65
    ##  4  3.69  4.49
    ##  5  2.99  4.44
    ##  6  2.96  3.55
    ##  7  3.34  3.28
    ##  8  3.96  4.00
    ##  9  1.33  3.26
    ## 10  2.40  4.57
    ## # … with 90 more rows

Let’s use a loop function

``` r
sim_results = 
  rerun(100, sim_mean_sd(sample_size = 30)) %>% 
  bind_rows()
```

Let’s look at results…

``` r
sim_results %>% 
  ggplot(aes(x = mean)) + geom_density()
```

<img src="simulation_files/figure-gfm/unnamed-chunk-5-1.png" width="90%" />

``` r
sim_results %>% 
  summarise(
    avg_sample_mean = mean(mean),
    sd_sample_mean = sd(mean)
  )
```

    ## # A tibble: 1 x 2
    ##   avg_sample_mean sd_sample_mean
    ##             <dbl>          <dbl>
    ## 1            3.08          0.732

``` r
sim_results %>% 
  ggplot(aes(x = sd)) + geom_density()
```

<img src="simulation_files/figure-gfm/unnamed-chunk-5-2.png" width="90%" />
