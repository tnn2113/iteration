Writing functions
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  1.4392101 -0.2377460 -0.9933344  0.1605835  1.3801643 -0.3678570
    ##  [7] -0.1745752  0.2083060  0.0115147 -1.1235147  0.7384245 -0.4588860
    ## [13]  2.9227112 -0.7441130 -1.0331676 -1.0961466 -0.2648805  0.9052441
    ## [19]  0.3314437 -0.6422126  1.8024319  0.5633389 -0.2422900 -0.4530752
    ## [25]  0.7454176 -0.7437154 -1.1122138 -1.5192704  0.4820383 -0.4838304

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

    ##  [1]  1.4392101 -0.2377460 -0.9933344  0.1605835  1.3801643 -0.3678570
    ##  [7] -0.1745752  0.2083060  0.0115147 -1.1235147  0.7384245 -0.4588860
    ## [13]  2.9227112 -0.7441130 -1.0331676 -1.0961466 -0.2648805  0.9052441
    ## [19]  0.3314437 -0.6422126  1.8024319  0.5633389 -0.2422900 -0.4530752
    ## [25]  0.7454176 -0.7437154 -1.1122138 -1.5192704  0.4820383 -0.4838304

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
    ## 1  3.22  4.12

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
    ## 1  3.93  3.27

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
    ## 1  6.25  3.22

## Let’s review Napoleon Dynamite

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

What about the next page of reviews…

Let’s turn that code into a function

``` r
read_page_reviews = function(url){
  
  dynamite_html = read_html(url)
  
  review_titles = 
    dynamite_html %>%
    html_nodes(".a-text-bold span") %>%
    html_text()

  review_stars = 
    dynamite_html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text() %>%
    str_extract("^\\d") %>%
    as.numeric()

  review_text = 
    dynamite_html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim()

  reviews = tibble(
    title = review_titles,
    stars = review_stars,
    text = review_text
)
  reviews
}
```

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

read_page_reviews(dynamite_url)
```

    ## # A tibble: 10 x 3
    ##    title                  stars text                                            
    ##    <chr>                  <dbl> <chr>                                           
    ##  1 Vote for Pedro!            5 Just watch the movie. Gosh!                     
    ##  2 Just watch the freaki…     5 Its a great movie, gosh!!                       
    ##  3 Great Value                5 Great Value                                     
    ##  4 I LOVE THIS MOVIE          5 THIS MOVIE IS SO FUNNY ONE OF MY FAVORITES      
    ##  5 Don't you wish you co…     5 Watch it 100 times. Never. Gets. Old.           
    ##  6 Stupid, but very funn…     5 If you like stupidly funny '90s teenage movies …
    ##  7 The beat                   5 The best                                        
    ##  8 Hilarious                  5 Super funny! Loved the online rental.           
    ##  9 Love this movie            5 We love this product.  It came in a timely mann…
    ## 10 Entertaining, limited…     4 Entertainment level gets a 5 star but having pr…

Let’s read a few pages of reviews

``` r
dynamite_url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="
 
dynamite_urls = str_c(dynamite_url_base, 1:5) 

all_reviews = bind_rows(
  read_page_reviews(dynamite_urls[1]),
  read_page_reviews(dynamite_urls[2]),
  read_page_reviews(dynamite_urls[3]),
  read_page_reviews(dynamite_urls[4]),
  read_page_reviews(dynamite_urls[5])
)
```

## Mean scoping exmaple

``` r
f = function(x) {
  z = x + y
  z
}

x = 1
y = 2

f(x = y)
```

    ## [1] 4

## functions as arguments

``` r
my_summary = function(x, summ_func) {
  
  summ_func(x)
  
}

x_vec = rnorm(100, 3, 7)

mean(x_vec)
```

    ## [1] 3.315652

``` r
median(x_vec)
```

    ## [1] 3.36427

``` r
my_summary(x_vec, mean)
```

    ## [1] 3.315652
