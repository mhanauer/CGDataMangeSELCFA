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
```

