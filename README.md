pipeline\_parnsip
================
Niccolò Salvini
24/4/2020

[![CRAN\_Status\_Badge](http://www.r-pkg.org/badges/version/parsnip)](https://cran.rstudio.com/package=parsnip)

<img src="img/logo.png" alt="drawing" width="139"/>

The story behind *Parnsip* is quite funny it comes from a vegetable and
it comes from a misunderstaning the word carrot with the `caret` package
for modelling.

the idea comes from the fact that R, before *Parnsip* did not have any
conventions in modelling packages, and so each functions and model
requires some *formatted* data instead of others. Talking about
prediction we could have a plenty of formats like data frame, a matrix
or a multidimensional array. So the point is to go from model to model
without stressing to much on syntax.

We do not want to remeber all the stuffs, it is like `caret` but the
tidyier version of it. Recall that `caret` was originally written in
**2005**, so pretty old syntax.

the road map starts with the selection of the model you want to apply
say *linear model*, *logistic regression*, *SVM* and so on…

-----

## `parsnip`

The package

  - creates a unified interface to models

  - organizes them by model type (e.g. logistic regression, MARS, etc)

  - generalizes *how* to fit them (aka their *computational engine*)

  - has a tidy interface

  - returns predictable objects

## Example: Linear Regression

say that you want to apply linear regression with the `linear_reg()` to
some data and add a penalty, here a L2 penalty (0.01). If you want to
use the `glmnet` or `spark` `stan` `keras` instead of the `lm()` you can
do that, why? `pransip` permits to **decouple** the *estimation
procedure* and the *package that you use* to accomplish that from the
actual specification model (lm glmnet), and this is the part of
computational engine. One other cool thing about the engine is that they
can also be outside R, they can ne in *Python* as well in the
scikit-learn.

Like `ggplot2` and `recipes`, `parsnip` defers the computations until
specific point.

Let’s create a model with a ridge penalty (i.e. weight decay). A model
specification is created:

``` r
library(tidymodels)
reg_model <- linear_reg(penalty = 0.01)
reg_model
```

    ## Linear Regression Model Specification (regression)
    ## 
    ## Main Arguments:
    ##   penalty = 0.01

``` r
reg_model %>% 
  set_engine("glmnet")
```

    ## Linear Regression Model Specification (regression)
    ## 
    ## Main Arguments:
    ##   penalty = 0.01
    ## 
    ## Computational engine: glmnet

A further spexification since it is a *crucial* passage: what is
happening it is because many packages includes the same thing aka ways
to perform linear regression. `lm()` can be done in base R or via the
glm package, so once you identify the engine you put it in the
computational engine, via the function `set_engine()`. One important
thing is that you are not doing anything for the moment, you are just
preparing the model to the point final point where at the end you are
fitting it in the data. As a matter of fact you are not imputting data
since the beginning. Once you specified the model, don’t worry, you fit
the model, with the formula and the dataset in the `fit()` part in the
chunk below:

``` r
reg_model %>% 
  set_engine("glmnet") %>% 
  fit(mpg ~ ., data = mtcars)
```

    ## parsnip model object
    ## 
    ## Fit time:  0ms 
    ## 
    ## Call:  glmnet::glmnet(x = as.matrix(x), y = y, family = "gaussian") 
    ## 
    ##       Df   %Dev   Lambda
    ##  [1,]  0 0.0000 5.147000
    ##  [2,]  2 0.1290 4.690000
    ##  [3,]  2 0.2481 4.273000
    ##  [4,]  2 0.3469 3.894000
    ##  [5,]  2 0.4290 3.548000
    ##  [6,]  2 0.4971 3.232000
    ##  [7,]  2 0.5537 2.945000
    ##  [8,]  2 0.6006 2.684000
    ##  [9,]  2 0.6396 2.445000
    ## [10,]  3 0.6726 2.228000
    ## [11,]  3 0.7015 2.030000
    ## [12,]  3 0.7256 1.850000
    ## [13,]  3 0.7455 1.685000
    ## [14,]  3 0.7621 1.536000
    ## [15,]  3 0.7759 1.399000
    ## [16,]  3 0.7873 1.275000
    ## [17,]  3 0.7968 1.162000
    ## [18,]  3 0.8046 1.058000
    ## [19,]  3 0.8112 0.964500
    ## [20,]  3 0.8166 0.878800
    ## [21,]  3 0.8211 0.800700
    ## [22,]  3 0.8249 0.729600
    ## [23,]  4 0.8281 0.664800
    ## [24,]  5 0.8320 0.605700
    ## [25,]  5 0.8360 0.551900
    ## [26,]  6 0.8396 0.502900
    ## [27,]  6 0.8426 0.458200
    ## [28,]  6 0.8451 0.417500
    ## [29,]  6 0.8472 0.380400
    ## [30,]  8 0.8489 0.346600
    ## [31,]  8 0.8514 0.315800
    ## [32,]  8 0.8535 0.287800
    ## [33,]  8 0.8553 0.262200
    ## [34,]  8 0.8568 0.238900
    ## [35,]  8 0.8580 0.217700
    ## [36,]  8 0.8590 0.198300
    ## [37,]  8 0.8598 0.180700
    ## [38,]  9 0.8606 0.164700
    ## [39,]  9 0.8615 0.150000
    ## [40,]  9 0.8622 0.136700
    ## [41,]  9 0.8627 0.124600
    ## [42,]  9 0.8632 0.113500
    ## [43,]  9 0.8636 0.103400
    ## [44,]  9 0.8639 0.094230
    ## [45,]  9 0.8642 0.085860
    ## [46,]  9 0.8644 0.078230
    ## [47,]  9 0.8646 0.071280
    ## [48,]  9 0.8648 0.064950
    ## [49,]  9 0.8649 0.059180
    ## [50,]  9 0.8650 0.053920
    ## [51,]  9 0.8651 0.049130
    ## [52,]  9 0.8652 0.044770
    ## [53,]  9 0.8652 0.040790
    ## [54,] 10 0.8654 0.037170
    ## [55,] 10 0.8660 0.033860
    ## [56,] 10 0.8665 0.030860
    ## [57,] 10 0.8669 0.028110
    ## [58,] 10 0.8673 0.025620
    ## [59,] 10 0.8676 0.023340
    ## [60,] 10 0.8678 0.021270
    ## [61,] 10 0.8680 0.019380
    ## [62,] 10 0.8682 0.017660
    ## [63,] 10 0.8683 0.016090
    ## [64,] 10 0.8684 0.014660
    ## [65,] 10 0.8685 0.013360
    ## [66,] 10 0.8686 0.012170
    ## [67,] 10 0.8687 0.011090
    ## [68,] 10 0.8687 0.010100
    ## [69,] 10 0.8688 0.009206
    ## [70,] 10 0.8688 0.008388
    ## [71,] 10 0.8688 0.007643
    ## [72,] 10 0.8689 0.006964
    ## [73,] 10 0.8689 0.006345
    ## [74,] 10 0.8689 0.005782
    ## [75,] 10 0.8689 0.005268
    ## [76,] 10 0.8689 0.004800
    ## [77,] 10 0.8689 0.004374
    ## [78,] 10 0.8690 0.003985
    ## [79,] 10 0.8690 0.003631

``` r
reg_model %>% 
  set_engine("glmnet") %>% 
  fit_xy(x = mtcars %>% select(-mpg), 
         y = mtcars$mpg)
```

    ## parsnip model object
    ## 
    ## Fit time:  0ms 
    ## 
    ## Call:  glmnet::glmnet(x = as.matrix(x), y = y, family = "gaussian") 
    ## 
    ##       Df   %Dev   Lambda
    ##  [1,]  0 0.0000 5.147000
    ##  [2,]  2 0.1290 4.690000
    ##  [3,]  2 0.2481 4.273000
    ##  [4,]  2 0.3469 3.894000
    ##  [5,]  2 0.4290 3.548000
    ##  [6,]  2 0.4971 3.232000
    ##  [7,]  2 0.5537 2.945000
    ##  [8,]  2 0.6006 2.684000
    ##  [9,]  2 0.6396 2.445000
    ## [10,]  3 0.6726 2.228000
    ## [11,]  3 0.7015 2.030000
    ## [12,]  3 0.7256 1.850000
    ## [13,]  3 0.7455 1.685000
    ## [14,]  3 0.7621 1.536000
    ## [15,]  3 0.7759 1.399000
    ## [16,]  3 0.7873 1.275000
    ## [17,]  3 0.7968 1.162000
    ## [18,]  3 0.8046 1.058000
    ## [19,]  3 0.8112 0.964500
    ## [20,]  3 0.8166 0.878800
    ## [21,]  3 0.8211 0.800700
    ## [22,]  3 0.8249 0.729600
    ## [23,]  4 0.8281 0.664800
    ## [24,]  5 0.8320 0.605700
    ## [25,]  5 0.8360 0.551900
    ## [26,]  6 0.8396 0.502900
    ## [27,]  6 0.8426 0.458200
    ## [28,]  6 0.8451 0.417500
    ## [29,]  6 0.8472 0.380400
    ## [30,]  8 0.8489 0.346600
    ## [31,]  8 0.8514 0.315800
    ## [32,]  8 0.8535 0.287800
    ## [33,]  8 0.8553 0.262200
    ## [34,]  8 0.8568 0.238900
    ## [35,]  8 0.8580 0.217700
    ## [36,]  8 0.8590 0.198300
    ## [37,]  8 0.8598 0.180700
    ## [38,]  9 0.8606 0.164700
    ## [39,]  9 0.8615 0.150000
    ## [40,]  9 0.8622 0.136700
    ## [41,]  9 0.8627 0.124600
    ## [42,]  9 0.8632 0.113500
    ## [43,]  9 0.8636 0.103400
    ## [44,]  9 0.8639 0.094230
    ## [45,]  9 0.8642 0.085860
    ## [46,]  9 0.8644 0.078230
    ## [47,]  9 0.8646 0.071280
    ## [48,]  9 0.8648 0.064950
    ## [49,]  9 0.8649 0.059180
    ## [50,]  9 0.8650 0.053920
    ## [51,]  9 0.8651 0.049130
    ## [52,]  9 0.8652 0.044770
    ## [53,]  9 0.8652 0.040790
    ## [54,] 10 0.8654 0.037170
    ## [55,] 10 0.8660 0.033860
    ## [56,] 10 0.8665 0.030860
    ## [57,] 10 0.8669 0.028110
    ## [58,] 10 0.8673 0.025620
    ## [59,] 10 0.8676 0.023340
    ## [60,] 10 0.8678 0.021270
    ## [61,] 10 0.8680 0.019380
    ## [62,] 10 0.8682 0.017660
    ## [63,] 10 0.8683 0.016090
    ## [64,] 10 0.8684 0.014660
    ## [65,] 10 0.8685 0.013360
    ## [66,] 10 0.8686 0.012170
    ## [67,] 10 0.8687 0.011090
    ## [68,] 10 0.8687 0.010100
    ## [69,] 10 0.8688 0.009206
    ## [70,] 10 0.8688 0.008388
    ## [71,] 10 0.8688 0.007643
    ## [72,] 10 0.8689 0.006964
    ## [73,] 10 0.8689 0.006345
    ## [74,] 10 0.8689 0.005782
    ## [75,] 10 0.8689 0.005268
    ## [76,] 10 0.8689 0.004800
    ## [77,] 10 0.8689 0.004374
    ## [78,] 10 0.8690 0.003985
    ## [79,] 10 0.8690 0.003631

Above You are doing things twice. You use the `fit()` when you want yo
input data through the formula, `fit_xy()` when you want to specify data
columns in a `dplyr` fashion, but again they do the same thing.

## Prediction

It very frustrting as we anticipated before that every time we are
talking about prediction you are not supposed to know which data type
you are goingto end up with. **Parnsip** simplfies that by uniforming
it. In *Parnisip* no matter what model you are using you are going to
end up with a **single .pred named Tibble column** (two more with the CI
lower and upper) double with the **exact number of observations** that
is it supposed to have. most of the predict function are using insisde
the `na.omit()` I might get a lower number of predictions back and then
you shoulf figure out where the na rows at. So at the end with *Parnsip*
you can cbind columns to your test/heldout dataset.

``` r
holdout <- mtcars %>% slice(30:32)
holdout[1, "disp"] <- NA
linear_reg(penalty = 0.01) %>% 
  set_engine("glmnet") %>% 
  fit(mpg ~ ., data = mtcars %>% slice(1:29)) %>% 
  predict(new_data = holdout)
```

    ## # A tibble: 3 x 1
    ##   .pred
    ##   <dbl>
    ## 1  NA  
    ## 2  10.9
    ## 3  25.7

In the previous chunk you are holding out 3 observations and as expected
you are having in the prediction .pred column tibble with 3
observations.

Below we are using the `glmnet` for linear regression (it can also have
logistics multinomila poisson and so on…), this uses a [different
estimation
method](https://web.stanford.edu/~hastie/glmnet/glmnet_alpha.html)
called *penalized maximum likelihood* , and it actually requires to
specify the lamdas penalities. What it does is computing the estimation
of the prediction for all the lamdas taken. As a matter of fact we are
having 80 values for each of the 3 overvations, obtaining a 80X2 for
each of the 3. The two in the 80 is the vakue associated with the
lambda. The first observation is all empty (nas) because it is a
missing, but in the second and in the third you are having values.
`pull(.pred)` drags the column out of dataframe object, `pluck()` helps
you select the list of the list (list\[\[\]\]), then the splice operator
selects the rows.

``` r
preds <- 
  linear_reg() %>%    # <- fit all penalty values
  set_engine("glmnet") %>% 
  fit(mpg ~ ., data = mtcars %>% slice(1:29)) %>% 
  multi_predict(new_data = holdout)
preds
```

    ## # A tibble: 3 x 1
    ##   .pred            
    ##   <list>           
    ## 1 <tibble [80 x 2]>
    ## 2 <tibble [80 x 2]>
    ## 3 <tibble [80 x 2]>

``` r
preds %>% pull(.pred) %>% pluck(1) %>% slice(1:2)
```

    ## # A tibble: 2 x 2
    ##   penalty .pred
    ##     <dbl> <dbl>
    ## 1 0.00346    NA
    ## 2 0.00380    NA

``` r
preds %>% pull(.pred) %>% pluck(2) %>% slice(1:5)
```

    ## # A tibble: 5 x 2
    ##   penalty .pred
    ##     <dbl> <dbl>
    ## 1 0.00346  10.9
    ## 2 0.00380  10.9
    ## 3 0.00417  10.9
    ## 4 0.00457  10.9
    ## 5 0.00502  10.9

# Data Decriptors

The package has *data descriptors* that are abstract **placeholders**
for characteristics of the data:

  - `.obs()`: The current number of rows
  - `.preds()`: The number of columns (before indicators)
  - `.cols()`: The number of columns (after indicators)
  - `.facts()`: The number of factor predictors
  - `.lvls()`: A table with the counts for each level
  - `.x()`: The predictor (data frame or matrix).
  - `.y()`: Outcome(s) (vector, matrix, or data frame)
  - `.dat()`: Training set

If you think about Random Forest the main tuning parameter is one called
*mtry*. And what random forest does is to build a tree and *mtry* is a
number of randomly selected predictor it will choose as candidates for
that split. So if you say *mtry* = 3 and you have 100 predictors it will
choose a random 3 out of 100 as candidates or candidate varibales to
split on and then it gets the next split and choose another 3. The
reason is that it has a big effect on performance so in this case the
preparation phase for the model where you do not even specify the
dataset is not very true for random forest, because actually here you
need to have predictors so the number of columns in your dataset. So to
solve that *Parnsip* has this thing called data descriptors we have this
little functions that capture some aspects of data so `obs()` captures
the number of rows and so on. So in the case below you are saying that
you want to use the 75% of the predictors, you want it to floor results
becuase you are needing an int. Then set the `randomForest` engine (make
sure you have it installed eventhough I think it is in the
dependencies).

## Specifying `mtry`

``` r
mod <- rand_forest(trees = 1000, mtry = floor(.preds() * .75)) %>% 
  set_engine("randomForest")
```
