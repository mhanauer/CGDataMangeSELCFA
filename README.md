

---
title: "Confirmatory Factor Analysis and Reliability Example"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Below we have six questions from a satisfaction assessment that is based upon the Intervention Rating Profile.  Below I demonstrate how to use two common statistical techniques to evaluate an assessment's reliability and validity.  

We start with reliability by generating a simple correlation matrix using the cor function for the included items.  A correlation matrix can range in values from -1 to 1 with negative values indicating items that move in an opposite direction while positive correlation demonstrates items that move in the same direction.  A correlation matrix shows each item's correlation with all other items.  We can see from the correlation matrix that all of the items have positive correlations of at least .4, with the first three items having correlations with each other of at least .75.  


Then we can get another estimate of reliability with Cronbach’s alpha.  Cronbach’s Alpha is an estimate of internal consistency measuring how well the items are related to each other.  Values range between 0 and 1 with values closer to one usually .8 or higher, indicating that a set of items is reliably measuring the same construct.  When we look at the 95% confidence interval for coefficient alpha, it is a tight range of .88 to .91 indicating that these are items reliably measuring a singular construct.
```{r}
setwd("~/Desktop/QualData")
both5 = read.csv("both5.csv", header = TRUE)
both5 = both5[c("Q1_1", "Q1_2", "Q1_3", "Q1_4", "Q1_5", "Q1_6")]
cor(both5, both5)

library(psych)
alpha(both5)

```
Next, I need to set up the confirmatory factor analysis.  I library the lavaan package. I then set up the model with one latent variable that I am calling satisfaction regressed upon the observed variables that I believe make up the satisfaction construct.  Finally, we included the estimator MLR, which a robust measure that creates standard errors that are robust against the violation of normality.  I then run the model with the cfa function and summarize the data to get the summary and model fit statistics.

Unfortunately, the unidimenstional model (i.e. model measuring satisfaction) is not a good fit for the data.  Our first case are the chi-square statistics which compare the observed and model implied covariance matrices.  The chi-square statistics are both significant providing an indication that observed data (i.e. the covariance matrix of items) is significantly different from the model estiamted.  However, the Chi-square statistic is notoriously problematic with large sample sizes therefore, we need to look other statistics that are not as senstive to sample size.  Therefore, we can look at CFI and TLI, which provide an indication of the percentage of improvement from the observed model over the null model.  Usually values of .8 or higher are regarded as adequate.  Unfortunately, both the CFI and TLI are both below .8, which demonstrate the unidimensional model is not a good fit.  Two other statistics that revolve around the residuals which are the difference between the estimated and observed the coviarnace matrices.  The RMSEA and SRMR both estimate the residuals and usually values below .1 are considered adequate.  Unforutnately, both the RMSEA and SRMR are both above .1.
```{r}
library(lavaan)
cfaSEL = 'Satisfaction = ~ Q1_1 + Q1_2 + Q1_3 + Q1_4 + Q1_5 + Q1_6'
cfaSEL2 = cfa(cfaSEL, estimator = "MLR", data = both5)
summary(cfaSEL2, fit.measures = TRUE)
```



