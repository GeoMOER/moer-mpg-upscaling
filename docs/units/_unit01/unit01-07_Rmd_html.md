---
order: 7
title: "Example: R Markdown with html output"
header:
  image: "/assets/images/teaser/Rteaser.jpeg"
  caption: 'Image: [**InfoWorld from IDG**](https://www.infoworld.com/article/2262286/do-more-with-r-video-tutorials.html){:target="_blank"}'
toc: true
toc_label: In this example
---

This page shows how a compiled R Markdown file looks like (in fact, all code examples in this course were compiled with R Markdown).

## This is a header

This is an R Markdown document. R Markdown is a simple formatting syntax for creating HTML, PDF, and MS Word documents. 
For more details on using R Markdown, see [rmarkdown.rstudio.com](http://rmarkdown.rstudio.com){:target="_blank"}.

When you click the **Knit** button in RStudio, a document will be generated which includes both content as well as the output of any embedded R code chunks *within* the document.
You can embed an R code chunk like this:


```r
summary(cars)
```

```
##      speed           dist       
##  Min.   : 4.0   Min.   :  2.00  
##  1st Qu.:12.0   1st Qu.: 26.00  
##  Median :15.0   Median : 36.00  
##  Mean   :15.4   Mean   : 42.98  
##  3rd Qu.:19.0   3rd Qu.: 56.00  
##  Max.   :25.0   Max.   :120.00
```


## This is another header

You can also embed plots, for example:

![Scatterplot of speed and stopping distance in the cars dataset]({{ site.baseurl }}/assets/images/rmd_images/rmd_html_out/unnamed-chunk-2-1.png)

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot (see below).


## R Markdown source
The above content of this page is the result of an R Markdown file, which looks like this:


``````yaml
---
title: "Example: R Markdown with html output"
author: "Thomas Nauss"
date: "10 Oktober 2018"
output: 
  html_document: 
    keep_md: yes
---
```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
knitr::opts_chunk$set(fig.path='{{ site.baseurl }}/assets/images/rmd_images/rmd_html_out/')
```

## This is a header

This is an R Markdown document. R Markdown is a simple formatting syntax for creating HTML, PDF, and MS Word documents. 
For more details on using R Markdown, see [rmarkdown.rstudio.com](http://rmarkdown.rstudio.com){:target="_blank"}.

When you click the **Knit** button in RStudio, a document will be generated which includes both content as well as the output of any embedded R code chunks *within* the document.
You can embed an R code chunk like this:

```{r}
summary(cars)
```

## This is another header

You can also embed plots, for example:

```{r echo=FALSE}
plot(cars)
```

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot (see below).

``````

## More fancy layouts?

If you want more styling options for your R Markdown documents, 
check out [this page](https://geomoer.github.io/moer-base-r/unit99/sl03_css.html){:target="_blank"}.



