--- 
title:  "Course Notes for IS 6489,  *Statistics and Predictive Analytics*"
author: "Jeff Webb"
date: "2017-12-22"
output: 
  bookdown::gitbook:
    keep_md: true
documentclass: book
bibliography: [book.bib, packages.bib]
biblio-style: apalike
link-citations: yes
description: "Course notes for IS 6489."
---

# Introduction

## Course topics



These are the course notes for IS 6489, *Statistics and Predictive Analytics*, offered through the Information Systems (IS) department in the University of Utah's David Eccles School of Business.

This is an exciting time for data analysis!  The field has undergone a revolution in the last 15 years with increases in computing power and the availability of "big data" from web-based systems of data collection.  "Data science" is the umbrella term that describes the result of this revolution---a new discipline at the intersection of many traditional fields such as statistics, computer science, and mathematics (among others). This convergence has resulted in the development of many new approaches to, and methods for, building statistical models to improve decision-making for individuals, companies and institutions.  "Predictive analytics" is in the course title because you will  learn not only how to build models but also how to  use  them to make educated guesses about the future. "Statistics" is in the course title because our approach to prediction will be rooted in the traditional, mathematically-based subject of statistics.

The course focuses on  regression as a staple data analytic technique.  We will cover both linear and logistic regression, as well as  more advanced methods such as regularization.  

Here are the main topics in the course:

- Statistical programming with R
- Exploratory data analysis 
- Statistical inference 
- Simulation
- Linear regression 
- Missing data imputation
- Cross validation
- Regularized regression (ridge regression and Lasso)
- Logistic regression
- Statistical communication

Along the way we will be comparing linear and logistic regression model performance to that of machine learning algorithms such as K-nearest neighbors, random forest, and gradient boosting.

With regression we can (among other things):

- **describe** the relationships between variables in a data sample and assess whether those relationships also exist in the population.

- create a model to **predict** unknown values of the outcome variable given known predictors.

## Software

The statistical software used in the course is  R, an open-source, object-oriented programming language that was invented to do statistics.  R is the successor to the earlier S language and resembles Matlab and Python in style and syntax. These notes presuppose familiarity with R programming. If you are new to the language, there are abundant introductory  resources for learning R available on the web (some of which are referenced in these notes).

R is a tremendous tool for doing data analysis; your efforts to master it will be richly rewarded. R is a more general purpose programming language than Matlab but less general purpose than Python. I have heard  purists complain about some of the conventions in R (probably with good reason). But, R is great.

- It is widely used, so examples of analysis and solutions to programming problems abound on the web.
- Cutting edge techniques are immediately implemented in packages (long before they are incorporated into SAS or SPSS or STATA, for example).
- As a scripting language, it facilitates collaboration, peer review and reproducibility.  
- The R ecosystem of tools has seen rapid growth in recent years, especially after the development of [RStudio](www.rstudio.com), a free IDE that, among many other useful functions, helps organize R files, projects and packages. 

R has even been profiled in in the [New York Times](http://www.nytimes.com/2009/01/07/technology/business-computing/07program.html?pagewanted=all&_r=0)!

## Example


Regression models are easy to fit and extremely powerful, *yet they can be confusing to  use and interpret*.

Consider the following dataset, `mtcars`, which lists data on various (older) makes of cars:


```r
data(mtcars)
head(mtcars)
```

```
##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
```

`head()` is a function that selects the first 6 rows of a dataset.

Suppose we wanted to create a model of `mpg` in an effort to understand the car features that influence fuel efficiency.  We could use linear regression, the command for which in R is  `lm()`.  For example, the following simple regression models the relationship between the number of cylinders in a car and its miles per gallon:


```r
summary(lm(mpg ~ cyl, data = mtcars))
```

```
## 
## Call:
## lm(formula = mpg ~ cyl, data = mtcars)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -4.9814 -2.1185  0.2217  1.0717  7.5186 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  37.8846     2.0738   18.27  < 2e-16 ***
## cyl          -2.8758     0.3224   -8.92 6.11e-10 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 3.206 on 30 degrees of freedom
## Multiple R-squared:  0.7262,	Adjusted R-squared:  0.7171 
## F-statistic: 79.56 on 1 and 30 DF,  p-value: 6.113e-10
```

Regression allows us to explore more complicated relationships as well.  The following multivariable linear regression models miles per gallon as a function of the car's number of cylinders, weight and type of carburetor.


```r
summary(lm(mpg ~ cyl + wt + carb, data = mtcars))
```

```
## 
## Call:
## lm(formula = mpg ~ cyl + wt + carb, data = mtcars)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -4.6692 -1.5668 -0.4254  1.2567  5.7404 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  39.6021     1.6823  23.541  < 2e-16 ***
## cyl          -1.2898     0.4326  -2.981 0.005880 ** 
## wt           -3.1595     0.7423  -4.256 0.000211 ***
## carb         -0.4858     0.3295  -1.474 0.151536    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.517 on 28 degrees of freedom
## Multiple R-squared:  0.8425,	Adjusted R-squared:  0.8256 
## F-statistic: 49.91 on 3 and 28 DF,  p-value: 2.322e-11
```

## Questions
- What do these coefficients mean exactly? 
- What does the rest of the complicated-looking output mean?  Standard errors, t-values, p-values, $R^2$, degrees of freedom, residuals, etc.?
- How would we translate the coefficients into meaningful quantities for a client with no background in statistics?
- Is this a good model? (And what do we mean by "good model"?)
- If we wanted to make it better, which variables should be added or removed?
- How would we know if adding or removing variables improved the model fit?
- Should any of these variables be transformed or should any outlying observations be removed from the dataset?
- Why does the coefficient for `cyl` differ between the two models?  Which one should we trust? 
- Does this model violate any of the mathematical assumptions of linear regression?

**Bottom line: Using modern statistical software to fit models is easy, but understanding, validating, improving and communicating your results can be a challenge.** 

This course will equip you for that challenge.



