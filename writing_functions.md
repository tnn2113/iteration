Writing functions
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.41531591 -0.55038074  0.50827890 -0.82022469 -0.73734593  2.04098408
    ##  [7] -0.61109659  1.50016133  0.37034070 -0.87254918  0.31173344  0.11807333
    ## [13]  0.76378466  0.64644550 -0.19245487 -2.19202569 -0.85520722  1.05952373
    ## [19] -0.49159045  1.98353659 -0.63502343  1.62283642 -1.44260015 -0.46226605
    ## [25]  0.54539128 -0.52153780 -0.91041374  0.08908845  0.11111590 -0.79189368

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

    ##  [1]  0.41531591 -0.55038074  0.50827890 -0.82022469 -0.73734593  2.04098408
    ##  [7] -0.61109659  1.50016133  0.37034070 -0.87254918  0.31173344  0.11807333
    ## [13]  0.76378466  0.64644550 -0.19245487 -2.19202569 -0.85520722  1.05952373
    ## [19] -0.49159045  1.98353659 -0.63502343  1.62283642 -1.44260015 -0.46226605
    ## [25]  0.54539128 -0.52153780 -0.91041374  0.08908845  0.11111590 -0.79189368

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

## Multiple outputs

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
    ## 1  3.27  3.81

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
    ## 1  4.36  2.64

``` r
sim_mean_sd = function(sample_size, mu, signa) {
  sim_data = 
    tibble(
    x = rnorm(n = sample_size, mean = mu, sd = signa)
  )

sim_data %>% 
  summarise(
    mean = mean(x),
    sd = sd(x)
  )
}

sim_mean_sd(sample_size = 100, mu = 6, signa = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.83  2.71

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

## Mean scoping example

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

    ## [1] 2.420758

``` r
median(x_vec)
```

    ## [1] 2.170307

``` r
my_summary(x_vec, mean)
```

    ## [1] 2.420758
