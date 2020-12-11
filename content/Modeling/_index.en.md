---
title: Modeling
weight: 25
---


Once the data has been cleaned and explored, we need to create a algorithm that will recommend movies based on the user's tastes. To do so, we are going to use the package recommenderlab. This package provides different algorithms including UBCF, SVD and ALS which we will investigate in this section. The first step will be to chose the optimal parameters for each candidate and then, compare the algorithm to select the most appropriate one for our dataset.

We are going to test the following models with their respective parameter.

|     Model    |                   Parameter                  |
|:------------:|:--------------------------------------------:|
|  UBCF Cosine | Nearest Neighbors (nn): 1, 5, 10, 20, 30, 40 |
| UBCF Pearson | Nearest Neighbors (nn): 1, 5, 10, 20, 30, 40 |
|      SVD     |  Item Subset (k): 1, 5, 10, 20, 30, 40, 100  |
|      ALS     |   Latent Factors (k): 1, 5, 10, 20, 30, 40   |

To do so, we are going to analyze their ROC curve. The higher the line is, the better the model.


## UBCF Cosine

![UBCF Cosine](/Modeling/images/ROC_UBCF-Cosine.png?classes=shadow&width=60pc)

Regarding the UBCF Cosine algorithm, we can see that using a Nearest Neighbors of 5 yields the best result as the ROC curve is the highest.

## UBCF Pearson
![UBCF Pearson](/Modeling/images/ROC_UBCF-Pearson.png?classes=shadow&width=60pc)

Using the UBCF Pearson algorithm, it can be notice that, as in the previous algorithm, the optimal Nearest Neighbors is 5.

## SVD
![SVD](/Modeling/images/ROC_SVD.png?classes=shadow&width=60pc)

With a SVD algorithm, the ROC curve shows a clear winner, the model with a number of item subset of 20.


## ALS
![ALS](/Modeling/images/ROC_ALS.png?classes=shadow&width=60pc)

Finally, using an ALS algorithm, we see that the optimal parameter is a latent factors of 40.

# Finding the best model

Combining the different optimal algorithms find above and adding a random algorithm for control, we will determine the best model for our data.

```{r}
recc_algos <- list(
  UBCF_cosine = list(name = "UBCF", param = list(method = "Cosine", nn=5)),
  UBCF_pearson = list(name = "UBCF", param = list(method = "Pearson", nn=5)),
  SVD = list(name = "SVD", param = list(k=20)),
  ALS = list(name = "ALS", param = list(n_factors=40)),
  random = list(name = "RANDOM", param=NULL)
)

recommendation_results <- evaluate(eval, method = recc_algos, n = n_recommendations)
```

![OVerall](/Modeling/images/ROC_all.png?classes=shadow&width=60pc)


The ROC curve of the SVD algorithm is well above any other algorithm. We conclude that the best model is a SVD algorithm with number of item (k) subset of 20. We implemented this model in the shiny application that you will see in the next section.