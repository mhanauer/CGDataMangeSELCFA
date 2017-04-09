---
title: "Confirmatory Factor Analysis and Reliability Example"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

Below we have six questions from a satisfaction assessment that is based upon the Intervention Rating Profile.  Below I demonstrate how to use two common statistical techniques to evaluate an assessment's reliability and validity.  

We start with reliability by generating a simple correlation matrix using the cor function for the included items.  A correlation matrix can range in values from -1 to 1 with negative values indicating items that move in an opposite direction while positive correlation demonstrates items that move in the same direction.  A correlation matrix shows each item's correlation with all other items.  We can see from the tables that 

Then we can get another estimate of reliability with Cronbach’s alpha.  Cronbach’s Alpha is an estimate of internal consistency measuring how well the items are related to each other.  Values range between 0 and 1 with values closer to one usually .8 or higher, indicating that a set of items is reliably measuring the same construct.

```{r}
cor(both5, both5)

library(psych)
alpha(both5)

```
Next, I need to set up the confirmatory factor analysis.  I library the lavaan package. I then set up the model with the latent variable that I am calling satisfaction regressed upon the observed variables that I believe make up the satisfaction construct.  I then run the model with the cfa function and summarize the data to get the summary and model fit statistics.
```{r}
install.packages("lavaan")
library(lavaan)
cfaSEL = 'Satisfaction = ~ Q1_1 + Q1_2 + Q1_3 + Q1_4 + Q1_5 + Q1_6'
cfaSEL2 = cfa(cfaSEL, data = both5)
summary(cfaSEL2, fit.measures = TRUE)
```


