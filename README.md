## Overview of the code

The folder `code` contains the code to generate random matrices, compute
*α*<sub>*F*</sub> and calculate the transitions to and from feasibility
(for small matrices).

### Building random matrices

The file `build_matrices.R` contains the code to build random matrices.
To build a matrix, call `build_matrix(n, dist, rho, sk)` with the
following parameters:

-   `n` size of the matrix
-   `dist` either `discrete`, `beta` or `normal`
-   `rho` correlation between off-diagonal elements
-   `sk` a distribution-dependent parameter controlling skewness

In particular, for `discrete` and `beta`, a parameter `sk == 1` produces
a distribution that is centered around its mean, `sk > 1` a right-skewed
distribution, and `sk < 1` a left-skewed distribution; for these
distributions, only values of `rho` in `1, -1, 0` are implemented.

For the `normal` distribution, `sk = 0` returns the standard normal
distribution, and can accept any `rho`; a positive `sk` makes the
distribution right-skewed, and a negative `sk` a left-skewed
distribution. If `sk` is nonzero, only values of `rho` in `1, -1, 0` are
implemented.

All distributions sample coefficients with mean zero and unit variance.
Addind a positive constant to the matrices does not impact feasibility.

``` r
# example 
source("code/build_matrices.R")
```

    ## Loading required package: stats4

    ## 
    ## Attaching package: 'sn'

    ## The following object is masked from 'package:stats':
    ## 
    ##     sd

``` r
# example with discrete distribution
B <- build_matrix(1500, "discrete", 0, 1.25)
# exclude diagonal elements to compute stats
Bp <- B
diag(Bp) <- NA
Bp <- as.vector(Bp)
print(mean(Bp, na.rm = TRUE))
```

    ## [1] 0.0003524572

``` r
print(var(Bp, na.rm = TRUE))
```

    ## [1] 1.000159

``` r
# example with beta distribution
B <- build_matrix(1500, "beta", -1, 1.25)
# exclude diagonal elements to compute stats
Bp <- B
diag(Bp) <- NA
Bp <- as.vector(Bp)
print(mean(Bp, na.rm = TRUE))
```

    ## [1] 5.904309e-20

``` r
print(var(Bp, na.rm = TRUE))
```

    ## [1] 1.000262

``` r
# example with normal distribution
B <- build_matrix(1500, "normal", 0.25, 0)
# exclude diagonal elements to compute stats
Bp <- B
diag(Bp) <- NA
Bp <- as.vector(Bp)
print(mean(Bp, na.rm = TRUE))
```

    ## [1] -0.0004356391

``` r
print(var(Bp, na.rm = TRUE))
```

    ## [1] 0.9989032
