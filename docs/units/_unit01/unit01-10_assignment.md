---
order: 10
title: "Assignment: Hello R, Hello GitHub"
header:
  image: "/assets/images/teaser/Rteaser.jpeg"
  caption: 'Image: [**Infoworld from IDG**](https://www.infoworld.com/video/series/8563/do-more-with-r){:target="_blank"}'
toc: true
toc_label: In this worksheet
---

This worksheet introduces you to R, R scripts and R Markdown. 
Your submission will be pushed to your course repository on GitHub.
After completion, you should have gained some experience in running R commands within scripts, R Markdown for running code and documentation within one file and Git/GitHub.

Before submitting, follow the [Classroom 50 setup steps](/moer-mpg-upscaling/unit00/unit00-01_learning_environment.html#classroom-50-and-github) to create your personal course repository.

## Things you need for this worksheet
  * [R](https://cran.r-project.org/){:target="_blank"} — the interpreter can be installed on any operating system.
  * [RStudio](https://posit.co/download/rstudio-desktop/){:target="_blank"} — we recommend to use RStudio for (interactive) programming with R.
  * [Git](https://git-scm.com/downloads){:target="_blank"} environment for your operating system. For Windows users with little experience on the command line, we recommend [GitHub Desktop](https://desktop.github.com/){:target="_blank"}.

## Hello R and GitHub submission
Please write an R script as an Rmd file with html output for the following task and commit it to your GitHub-based learning log:

1. Assign the value of five to a variable called `a` and the value of two to a variable called `b`.
1. Compute the sum, difference, product and ratio of a and b (a always in the first place) and store the results to four different variables called `r1`, `r2`, `r3`, and `r4`.
1. Create a vector `v1` which contains the values stored within the four variables from step 2.
1. Add a fifth entry to vector `v1` which represents `a` by the power of `b` (i.e. `a**b`).
1. Show the content of vector `v1` (e.g. use the `print` function or just type the variable name in a separate row).
1. Create a second vector `v2` which contains information on the type of mathematical operation used to derive the five results. Hence this vector should have five entries of values *sum*, *difference*,...
1. Show the content of vector `v2`.
1. Combine the two vectors `v1` and `v2` into a data frame called `df`. Each vector should become one column of the data frame so you will end up with a data frame having 5 rows and 2 columns.
1. Make sure that the column with the data of `v1` is named *Results* and `v2` is named *Operation*.
1. Show the entire content of `df`.
1. Show just the entry of the cell in the second row and first column.

Save your Rmd file in your course repository and knit it to HTML. Commit both the Rmd source and the generated HTML file, then push your changes to GitHub. Check that both files are present in your course repository on GitHub.

As you might become aware by reading this assignment, there is a great chance that you have to gather considerably more information to actually solve the given assignment. More precisely, once you have solved the R programming task, you are not off the hook since you have to familiarize yourself with Git and GitHub to actually submit your solution.
{: .notice--warning}


