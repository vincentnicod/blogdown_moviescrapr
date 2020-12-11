---
title: Implementation
weight: 30
---

## The application

We decided to create a shiny application that applies that we have seen so far. This application takes the selected movie, the rating inputed by the user and the optional filters and output a table containing at most 5 movies that we think you will like.

You can watch this small videos that talks in more details about this:

{{< youtube BKY-p0nX2v4 >}}


## How to install the package
To install the package, you simply need to run the following command:

```{r}
remote::install_github("ptds2020/MovieScrapR")
```

## How to use the application

1) Once the package installed, you need to run the following command:

```{r}
Movie_Recommender()
```

2) The user need to choose between 2 and 10 movies

3) The user need to rate each movie

4) If desired, the user can add additional filters such as the period, the genre or even the popularity

5) By clicking on run, the user will be provided with at most 5 movies our algorithm found especially for him!

6) Sit back and enjoy your movie!

Here is an example of step 1 to 4:
![Shiny](/Shiny/images/shiny.png?classes=shadow&width=60pc)