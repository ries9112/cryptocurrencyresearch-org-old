---
title: "Cryptocurrency Research"
date: 'Last Updated:<br/> 2020-10-25 19:27:28'
site: bookdown::bookdown_site
documentclass: book
bibliography:
- packages.bib
biblio-style: apalike
link-citations: yes
---

# Introduction {#introduction}



**(this book is a work in progress and is NOT ready)**

<!-- Welcome to this tutorial on supervised machine learning. This document will walk you through the process of using data relating to cryptocurrencies and past price movements to create models that help forecast future prices.  -->

<!-- First we will discuss the process at a high-level  -->

Welcome to this programming tutorial on machine learning!

## What will I learn?

-   In this tutorial you will primarily learn about tools for the R programming language developed by [RStudio](https://rstudio.com/) and more specifically the [tidyverse](https://www.tidyverse.org/). If you are not familiar with these open source products, don't worry. We'll introduce these throughout the tutorial as needed.

-   The focus of the tutorial is on supervised machine learning, a process for building models that can predict future events, such as cryptocurrency prices. You will learn how to use the [`caret`](https://topepo.github.io/caret/index.html) package to make many different predictive models, and tools to evaluate their performance.

-   Before we can make the models themselves, we will need to "clean" the data. You will learn how to perform "group by" operations on your data using the [`dplyr`](https://dplyr.tidyverse.org/) package and to clean and modify grouped data.

<!-- -   You will be exposed to some useful data pre-processing R packages. -->

-   You will learn to visualize data using the [`ggplot2`](https://ggplot2.tidyverse.org/) package, as well as some powerful [tools to extend the functionality of ggplot2](https://exts.ggplot2.tidyverse.org/).

-   You will gain a better understanding of the steps involved in any supervised machine learning process, as well as considerations you might need to keep in mind based on your specific data and the way you plan on acting on the predictions you have made. Each problem comes with a unique set of challenges, but there are steps that are necessary in creating any supervised machine learning model, and questions you should ask yourself regardless of the specific problem at hand.

<!-- - [TODO - Add note on cross validation] -->


-   You will also learn a little about cryptocurrencies themselves, but **this is not a tutorial centered around trading or blockchain technology**.

## Before Getting Started

You can toggle the sidebar on the left side of the page by clicking on the menu button in the top left, or by pressing on the **s** key on your keyboard. You can read this document as if it were a book, scrolling to the bottom of the page and going to the next "chapter", or navigating between sections using the sidebar.

This document is the tutorial itself, but in order to make the tutorial more accessible to people with less programming experience (or none) we created a [high-level version of this tutorial](https://cryptocurrencyresearch.org/high-level/), which simplifies both the problem at hand (what we want to predict) and the specific programming steps, but uses the same tools and methodology providing easier to digest examples that can be applied to many different datasets and problems.

### High-Level Version

If you are not very familiar with programming in either R or Python, or are not sure what cryptocurrencies are, you should **definitely** work your way through the [high-level version first](https://cryptocurrencyresearch.org/high-level/). If you are an intermediate programmer we still recommend the [high-level version first](https://cryptocurrencyresearch.org/high-level/). 

Below is an embedded version of the high-level version, but we recommend using any of the links above to view it in its own window. Once in its own window it can be full-screened by pressing on the **f** button on your keyboard.
<iframe src="https://cryptocurrencyresearch.org/high-level/" width="672" height="400px"></iframe>


\BeginKnitrBlock{infoicon}<div class="infoicon">When following along with this example, the results will be completely reproducible and the document is static and does not update over time. The document you are currently reading this text on on the other hand updates every 12 hours. You will be able to access the same data source, but it updates hourly by the 8th minute of every hour with the most recent data, so this document and your results won't perfectly match.</div>\EndKnitrBlock{infoicon}


<!-- ## Who is this example for? -->

<!-- Are you a Bob, Hosung, or Jessica below? This section provides more information on how to proceed with this tutorial. -->

<!-- -   **Bob (beginner)**: Bob is someone who understands the idea of predictive analytics at a high level and has heard of cryptocurrencies and is interested in learning more about both, but he has never used R. Bob would want to first [learn the basics of the R programming language](https://predictcrypto.org/r-basics-tutorial). Afterwards, like Hosung, he might decide to [opt for the more high-level version of this tutorial](https://cryptocurrencyresearch.org/high-level/). -->

<!--     -   Bob might benefit from the book ["R for Data Science"](https://r4ds.had.co.nz/) and DataCamp's free ["Introduction to R"](https://www.datacamp.com/courses/free-introduction-to-r) course are great resources as well. Both are available for free online. -->

<!-- -   **Hosung (intermediate)**: Hosung is a statistician who learned to use R 10 years ago. He has heard of the tidyverse, but doesn't regularly use it in his work and he usually sticks to base R as his preference. Hosung should review the example explaining the pipe operator and the tidyverse [when it comes up in the next section](https://cryptocurrencyresearch.org/setup.html#tidy-example). Hosung does not care much for cryptocurrencies specifically, but is interested in learning new tools to use for his work in R and getting some useful boilerplate code. Hosung would want to start with the [**high-level version of this tutorial**](https://cryptocurrencyresearch.org/high-level/) and later return to the [**detailed tutorial**](https://cryptocurrencyresearch.org/index.html#introduction). -->

<!-- -   **Jessica (expert)**: Jessica is a business analyst who has experience with both R and the Tidyverse and uses the pipe operator (%\>%) regularly. Jessica should [**move onto the next section**](#introduction) for the detailed tutorial. -->


<!-- ### Approach taken -->

<!-- It is also worth mentioning that we will be taking a very practical approach to machine learning. Because we are modeling many cryptocurrencies independently at the same time and the dataset evolves as time passes, it becomes difficult to make a decision across the board in terms of what the "best model" to use is because it may change based on the specific cryptocurrency in question and/or over time. -->

<!-- [TODO - KEEP ADDING HERE?] -->

## Format Notes

-   You can hide the sidebar on the left by pressing the ***s*** key on your keyboard.

-   When the code is `highlighted this way`, that means we are referencing an R object of some kind. This will usually be an object in the R session like a dataset, but functions and packages are highlighted in the same way.

-   The cryptocurrency data was chosen to produce results that change over time and because these markets don't have any downtime (unlike the stock market).

## Plan of Attack

How will we generate predictive models to predict cryptocurrency prices? At a high level, here are the steps we will be taking:

1.  [Setup guide](#setup). Installation guide on getting the tools used installed on your machine.

2.  [Explore data](#explore-data). What ***is*** the data we are working with? How "good" is the "quality"?

3.  [Prepare the data](#data-prep). Make adjustments to "clean" the data based on the findings from the previous section to avoid running into problems when making models to make predictions.

4.  [Visualize the data](#visualization). Visualizing the data can be an effective way to understand relationships, trends and outliers before creating predictive models.

5.  [Make predictive models](#predictive-modeling). Now we are ready to take the data available to us and use it to make predictive models that can be used to make predictions about future price movements using the latest data.

6.  [Evaluate the performance of the models](#evaluate-model-performance). Before we can use the predictive models to make predictions on the latest data, we need to understand how well we expect them to perform, which is also essential in detecting issues.

## Disclaimer

\BeginKnitrBlock{rmdimportant}<div class="rmdimportant">This tutorial is made available for learning and educational purposes only and the information to follow does not constitute trading advice in any way shape or form. We avoid giving any advice when it comes to trading strategies, as this is a very complex ecosystem that is out of the scope of this tutorial; we make no attempt in this regard, and if this, rather than data science, is your interest, your time would be better spent following a different tutorial that aims to answer those questions.</div>\EndKnitrBlock{rmdimportant}

<!-- REMEMBER OTHER SPECIAL BLOCK TYPE IS CALLED "rmdcaution" USE THESE TO DRAW ATTENTION TO ANYTHING IMPORTANT! -->

<!--chapter:end:index.Rmd-->


# Setup and Installation {#setup}

Placeholder


## Option 1 - Run in the Cloud
## Option 2 - Run Locally
### Setup R
## Installing and Loading Packages {#installing-and-loading-packages}

<!--chapter:end:01-Setup.Rmd-->


# Explore the Data {#explore-data}

Placeholder


## Pull the Data
## Data Preview {#data-preview}
## The definition of a "price"
## Data Quality {#data-quality}

<!--chapter:end:02-ExploreData.Rmd-->


# Data Prep {#data-prep}

Placeholder


## Remove Nulls
## Calculate `price_usd` Column
## Clean Data by Group
### Remove symbols without enough rows
### Remove symbols without data from the last 3 days
## Any gaps?
### Convert to tsibble
#### Convert to hourly data and get rid of minutes and seconds
### Scan gaps
### Fill gaps
### Calculate Target
## Cross Validation
## Fix Data by Split
### Zero Variance
## Nest data
## Functional Programming

<!--chapter:end:03-DataPrep.Rmd-->


# Visualization 📉

Placeholder


## Basics - ggplot2
## Using Extensions
### ggthemes
### plotly
### ggpubr
### ggforce
### gganimate
### Calendar Heatmap
### Rayshader

<!--chapter:end:04-Visualization.Rmd-->


# Predictive Modeling {#predictive-modeling}

Placeholder


## Example Simple Model
### Using Functional Programming
## Caret
### Parallel Processing
### Functional Programming - here or elsewhere?
### Cross Validation
### XGBoost models
### All Other Models
#### Neural Network models
#### Gradient Boosting Machines
## Make Predictions
## Traditional Timeseries

<!--chapter:end:05-PredictiveModeling.Rmd-->


# Evaluate Model Performance{#evaluate-model-performance}

Placeholder


## Summarizing models
### Adjust RMSE
#### Predictions Adjustment
#### Actual Results Adjustment
### Calculate RMSE
### Calculate R^2
### New Section
## Visualize Results
### RMSE Visualization
### Both
#### Join Datasets
#### Plot Results
### Results by the Cryptocurrency

<!--chapter:end:06-EvaluateModelPerformance.Rmd-->

# Time Series

Add here

<!--chapter:end:07-TimeSeriesModeling.Rmd-->


<!--chapter:end:08-HyperParameterTuning.Rmd-->

<!-- # Predictions -->


<!-- (look at past versions) -->

<!--chapter:end:09-Predictions.Rmd-->


# Explain Predictions

Placeholder



<!--chapter:end:10-ExplainPredictions.Rmd-->

# Considerations

What we have outlined here is a supervised machine learning problem and a practical possible approach to the problem, but the results contained in this document **would not translate to real-world results**. 

This tutorial is not meant to show anyone how to trade on the cryptocurrency markets, but rather encourage people to apply these tools to their own data problems, and that is the reason the tutorial stops here (also because we like not getting sued).

We stop here before dealing with many difficult execution problems, including but not exclusive to:

- Finding a trading methodology that makes sense. There are lots of decisions to be made, market or limit orders? Coming up with a good trade execution plan that works consistently is not as easy.

-


- ...Keep outlining the issues outlined by Chandler, especially relating to low market cap cryptocurrencies



## Session Information

Below is information relating to the specific R session that was run. If you are unable to reproduce these steps, find the correct version of the tools to install below:


```r
sessionInfo()
```

```
## R version 3.6.3 (2020-02-29)
## Platform: x86_64-w64-mingw32/x64 (64-bit)
## Running under: Windows 10 x64 (build 18363)
## 
## Matrix products: default
## 
## locale:
## [1] LC_COLLATE=English_United States.1252 
## [2] LC_CTYPE=English_United States.1252   
## [3] LC_MONETARY=English_United States.1252
## [4] LC_NUMERIC=C                          
## [5] LC_TIME=English_United States.1252    
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] knitr_1.28    bookdown_0.18
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_1.0.4.6     lubridate_1.7.8  emo_0.0.0.9000   crayon_1.3.4    
##  [5] digest_0.6.25    assertthat_0.2.1 magrittr_1.5     evaluate_0.14   
##  [9] highr_0.8        rlang_0.4.7      stringi_1.4.6    rstudioapi_0.11 
## [13] generics_0.0.2   rmarkdown_2.1    tools_3.6.3      stringr_1.4.0   
## [17] glue_1.4.1       purrr_0.3.4      xfun_0.13        yaml_2.2.1      
## [21] compiler_3.6.3   htmltools_0.5.0
```




<!--chapter:end:12-Considerations.Rmd-->

# Archive

Below is an archive of this same document from different dates:

## May 2020

[May 23, 2020 - Morning](https://5ec910b25ba546000683ef75--researchpaper.netlify.app/)

[May 22, 2020 - Morning](https://5ec7bfda63f2680006dc7adb--researchpaper.netlify.app/)

[May 21, 2020 - Morning](https://5ec66e35e51006000702d6a6--researchpaper.netlify.app/)

[May 20, 2020 - Morning](https://5ec54d80a11b3100064e7032--researchpaper.netlify.app/)

[May 19, 2020 - Morning](https://5ec3ebd67cef13000653e8ec--researchpaper.netlify.app/)

[May 18, 2020 - Morning](https://5ec27a599a31b90007d45f5e--researchpaper.netlify.app/)

<!--chapter:end:13-Archive.Rmd-->

# References

The **bookdown** package [@R-bookdown] was used to produce this document, which was built on top of R Markdown and **knitr** [@xie2015].


```r
knitr::write_bib(c(.packages()), "packages.bib")
```

## Resources Used

### Visualization Section

- https://www.rayshader.com/

- https://github.com/yutannihilation/gghighlight

    - https://github.com/njtierney/rstudioconf20/blob/master/slides/index.Rmd

- ... add rest here


### Time Series Section

- https://stackoverflow.com/questions/42820696/using-prophet-package-to-predict-by-group-in-dataframe-in-r

- ... add rest here


## Packages used and cited

<!--chapter:end:99-references.Rmd-->

