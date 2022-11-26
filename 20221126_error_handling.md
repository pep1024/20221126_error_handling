Functions to handle errors in R and python
================
Pep Porrà
2022-11-26

## Goal

Explore functions to handle errors (intercept them) in both R and python

## References

-   Advance R
-   Efficient R programming
-   Python for dats Scientists
-   Python handbook

## R

Let us take a simple example. We call a function `f()` that, in some
cases, raises an error and stops execution. If `f()` function is called
within one of our functions or scripts, it will stop as soon as `f()`
fails.

``` r
f <- function(x) charToRaw(x)
```

``` r
f(9)
#> Error in charToRaw(x) : argument must be a character vector of length 1
```

``` r
our_function <- function(x){
  y <- f(x)
  now <- Sys.time()
  class(y)
  return(now)
}

our_function(9)
#> Error in charToRaw(x) : argument must be a character vector of length 1
```

### `try()`

``` r
our_function_1 <- function(x){
  y <- try(f(x))
  now <- Sys.time()
  print(class(y))
  return(now)
}
our_function_1(9)
```

    ## Error in charToRaw(x) : argument must be a character vector of length 1
    ## [1] "try-error"

    ## [1] "2022-11-26 21:26:40 CET"

With `try()` function, execution does not stop after the error. Error
message is shown but code after the error is executed

``` r
our_function_1 <- function(x){
  y <- try(f(x), silent = TRUE)
  now <- Sys.time()
  class(y)
  return(now)
}
our_function_1(9)
```

    ## [1] "2022-11-26 21:26:40 CET"

Parameter `silent = TRUE` of `try()` function prevents the message error
to be shown.

Note that `try()` return an object of class “try-error” when `f()`call
fails, which can be used to run specific actions after the error \[1\]:

``` r
our_function_1 <- function(x){
  y <- try(f(x), silent = TRUE)
  if (class(y) == "try-error") {
    # action after the error is detected
    message("ERROR in function f")
  }
  now <- Sys.time()
  class(y)
  return(now)
}
our_function_1(9)
```

    ## ERROR in function f

    ## [1] "2022-11-26 21:26:40 CET"

### `tryCatch()`

``` r
our_function_2 <- function(x){
  y <- tryCatch( f(x),
    error = function(c) {
      message("standard error message: ", c$message, "\n")
      message("Additional information ...\n")
    },
    finally = message("End of function tryCatch()"))
  if (class(y) == "try-error") {
    # action after the error is detected
    message("ERROR in function f")
  } else {
    message(paste("y =", y))
  }
  now <- Sys.time()
  message(paste("class of y:", class(y)))
  return(now)
}
our_function_2(9)
```

    ## standard error message: argument must be a character vector of length 1

    ## Additional information ...

    ## End of function tryCatch()

    ## y =

    ## class of y: NULL

    ## [1] "2022-11-26 21:26:40 CET"

``` r
suppressMessages(our_function_2(9))
```

    ## [1] "2022-11-26 21:26:40 CET"

``` r
our_function_2("9")
```

    ## End of function tryCatch()

    ## y = 39

    ## class of y: raw

    ## [1] "2022-11-26 21:26:40 CET"

## Python

``` r
reticulate::py_config()
```

    ## python:         C:/Users/josep/miniconda3/envs/r-reticulate/python.exe
    ## libpython:      C:/Users/josep/miniconda3/envs/r-reticulate/python38.dll
    ## pythonhome:     C:/Users/josep/miniconda3/envs/r-reticulate
    ## version:        3.8.12 | packaged by conda-forge | (default, Oct 12 2021, 21:19:05) [MSC v.1916 64 bit (AMD64)]
    ## Architecture:   64bit
    ## numpy:          C:/Users/josep/miniconda3/envs/r-reticulate/Lib/site-packages/numpy
    ## numpy_version:  1.22.3
    ## 
    ## NOTE: Python version was forced by RETICULATE_PYTHON

``` python
# def 
```
