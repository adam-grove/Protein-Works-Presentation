Univariate Analysis
================
2024-09-30

``` r
library(moments)
```

``` r
# Reading in the data
Sale_Price <- read.csv("Sales_data.csv",header=T)
```

# Summary Statistics

``` r
summary(Sale_Price)
```

    ##  unit_sale_price_gbp
    ##  Min.   : 0.0531    
    ##  1st Qu.: 3.2870    
    ##  Median :10.0024    
    ##  Mean   :12.5501    
    ##  3rd Qu.:18.8091    
    ##  Max.   :70.5735

``` r
# Calculate and print summary statistics for unit_sale_price_gbp
mean_unit_sale_price_gbp <- mean(Sale_Price$unit_sale_price_gbp)
median_unit_sale_price_gbp <- median(Sale_Price$unit_sale_price_gbp)
sd_unit_sale_price_gbp <- sd(Sale_Price$unit_sale_price_gbp)
var_unit_sale_price_gbp <- var(Sale_Price$unit_sale_price_gbp)
skewness_unit_sale_price_gbp <- skewness(Sale_Price$unit_sale_price_gbp)
kurtosis_unit_sale_price_gbp <- kurtosis(Sale_Price$unit_sale_price_gbp)

# Print the results
print(paste0("Mean: ", mean_unit_sale_price_gbp))
```

    ## [1] "Mean: 12.550068731976"

``` r
print(paste0("Median: ", median_unit_sale_price_gbp))
```

    ## [1] "Median: 10.002398097"

``` r
print(paste0("Standard Deviation: ", sd_unit_sale_price_gbp))
```

    ## [1] "Standard Deviation: 11.3248868529429"

``` r
print(paste0("Variance: ", var_unit_sale_price_gbp))
```

    ## [1] "Variance: 128.253062231958"

``` r
print(paste0("Skewness: ", skewness_unit_sale_price_gbp))
```

    ## [1] "Skewness: 1.38015163225954"

``` r
print(paste0("Kurtosis: ", kurtosis_unit_sale_price_gbp))
```

    ## [1] "Kurtosis: 5.20636911964138"

# Analysis of results

Adressing the skewness of the data. The mean of the data is larger than
the median, as wel las a skewness value of 1.38 suggesting that the data
is *right skewed*

``` r
# Histogram
hist(Sale_Price$unit_sale_price_gbp,main = "Histogram of Sale Price" ,xlab = "Sales Price", ylab = "Frequency", col = "lightblue")
```

![](Univarate-Analysis_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

This histogram has a long tail towards the right (right skew). This can
be stastically represented by Kurtosis of 5.21 which indicates the heavy
tail.

``` r
# Density Plot
plot(density(Sale_Price$unit_sale_price_gbp), main = "Density Plot of Sales Prices", xlim = c(0,70))
```

![](Univarate-Analysis_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

# Testing for Normality

The normality of data can affect the choice of stastical tests and
models created. The shape of the data suggests that it does not follow a
normal distribution but there are additional tests that we can use. The
example I’ve used here is a *Shapiro-Wilk* test

``` r
# Shapiro Test
shapiro.test(Sale_Price$unit_sale_price_gbp)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  Sale_Price$unit_sale_price_gbp
    ## W = 0.8744, p-value < 2.2e-16

This test gave a very small p value. Meaning we can say with confidence
that the data is not normally distributed. This can be visualised in a
Q-Q plot. The closer the points are to the normal line, the more likely
the data follows a normal distribution

``` r
# Q-Q Plot
qqnorm(Sale_Price$unit_sale_price_gbp)
qqline(Sale_Price$unit_sale_price_gbp)
```

![](Univarate-Analysis_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

In this diagram, the data deviated from the line in the 2nd and 3rd
quartiles, again suggesting it does not follow a normal distribution. It
also helps us to confirm the right skew of the data

# Investigating outliers

We know from the shape of the histogram and the Kurtosis value that
there are some large values that are skewing the data. These can be
visualised by a boxplot

``` r
# Boxplot
boxplot(Sale_Price$unit_sale_price_gbp,main = "Box Plot of Sales Prices", col = "lightblue")
```

![](Univarate-Analysis_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

This boxplot shows that there are values that are outside limits to be
considered an outlier and and will skew the data.

# Conclusions

This dataset has a wide variance, numbers are spread far out and there
are extremes at the larger end that are causing right skew

The mean of this data will not be ideal to use as there are distruptions
to the *central tendancy of the data*. In this case it will be most
useful to use the **median** instead of the mean. As it is less affected
by the skew of the data and will be more representative of an average
unit price

## Some considerations

With larger datasets median may not be so effective 1. Computational
power used to calculate the median is higher 2. Medians are less useful
with lots of zero values (median number of pregnancies per copulation is
zero) 3. There are some data types where median is as useful. E.g. In
sales, removing the tails might underestimate your
