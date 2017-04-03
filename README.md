---
title: "CGDataManageSELCFA"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Grab data from both districts and put them into seperate data frames
```{r}
setwd("~/Desktop/QualData")
mccsc = read.csv("MCCSCStaffSurvey.csv", header = TRUE)
rbbcsc = read.csv("RBBCSCStaffSurvey.csv", header = TRUE)
```
Next we grab the variables of interest which are the SEL variables from each of them
```{r}
mccsc1 = mccsc[c("Q1_1", "Q1_2", "Q1_3", "Q1_4", "Q1_5", "Q1_6")]
mccsc2 = mccsc1[-c(1:2), ]

rbbcsc1 = rbbcsc[c("Q1_1", "Q1_2", "Q1_3", "Q1_4", "Q1_5", "Q1_6")]
rbbcsc2 = rbbcsc1[-c(1:2), ]

both = rbind(mccsc2, rbbcsc2)

write.csv(both, "both.csv")
both1 = read.csv("both.csv", header = TRUE, , na.strings = c(""))
both2 = na.omit(both1); head(both2)
```
Now we need to transform all of the categorical variables into numerical ones
```{r}
both2 = apply(both2, 2, function(x){ifelse(x == "Strongly agree", 7, x)})
both2 = as.data.frame(both2); head(both2)
both2 = apply(both2, 2, function(x){ifelse(x == "Agree", 6, x)})
both2 = as.data.frame(both2)
both2 = apply(both2, 2, function(x){ifelse(x == "Somewhat agree", 5, x)})
both2 = as.data.frame(both2)
both2 = apply(both2, 2, function(x){ifelse(x == "Neither agree nor disagree", 4, x)})
both2 = as.data.frame(both2)
both2 = apply(both2, 2, function(x){ifelse(x == "Somewhat disagree", 3, x)})
both2 = as.data.frame(both2)
both2 = apply(both2, 2, function(x){ifelse(x == "Disagree", 2, x)})
both2 = as.data.frame(both2)
both2 = apply(both2, 2, function(x){ifelse(x == "Strongly disagree", 1, x)})
both2 = as.data.frame(both2)
```
There is an extra "x" variable, so we need to get rid of that variable
```{r}
both3 = both2[c("Q1_1", "Q1_2", "Q1_3", "Q1_4", "Q1_5", "Q1_6")]
both3 = as.data.frame(both3)
```
Now we run a simple correlation for descriptive statistics.  Need to reinput the data so we can get it into the right format and again get rid of the extra x variable.

Then we can run the correlation between the variables.

Then we can get an estimate of reliablilty with coefficient alpha
```{r}
write.csv(both3, "both3.csv")
both4 = read.csv("both3.csv", header = TRUE)
both5 = both4[c("Q1_1", "Q1_2", "Q1_3", "Q1_4", "Q1_5", "Q1_6")]

cor(both5, both5)

install.packages("psych")
library(psych)
alpha(both5)

```
Next we need to set up the CFA
```{r}
install.packages("lavaan")
library(lavaan)
cfaSEL = 'Satisfaction = ~ Q1_1 + Q1_2 + Q1_3 + Q1_4 + Q1_5 + Q1_6'
cfaSEL2 = cfa(cfaSEL, data = both5)
summary(cfaSEL2, fit.measures = TRUE)
```

