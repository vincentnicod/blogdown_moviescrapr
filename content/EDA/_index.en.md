---
title: EDA
weight: 15
---


The ratings dataset contains 469'977 ratings of 9'926 movies given by 33'912 users.

```{r eval=TRUE, message=FALSE, warning=FALSE, include=TRUE}
ratings %>%
  summarize(total_users = n_distinct(user_id))
```
|     total_users    |
|--------------------|
| 33912              |

```{r eval=TRUE, message=FALSE, warning=FALSE, include=TRUE}
ratings %>%
  summarize(total_movies = n_distinct(url))
```
|  total_movies  |
|----------------|
| 9926           |

```{r eval=TRUE, message=FALSE, warning=FALSE, include=TRUE, echo=FALSE}
ratings %>%
  ggplot(aes(rating)) +
    geom_histogram(
    aes(y = stat(width * density)),
    alpha = 0.8,
    position = position_dodge2(0.5),
    binwidth = 0.5,
    color = "black",
  ) +
  scale_fill_viridis_d() +
  labs(y = "proportion") +
  theme(legend.title = element_blank())
```

![Graph1](/EDA/images/eda_rating_proportion.png?classes=shadow&width=60pc)


As shown on the graph above the distribution of the ratings is left skewed, with a mean rating of 3.629 and a median of 4.

```{r echo=FALSE, message=FALSE, warning=FALSE}
n.rating.user <- ratings %>%
  group_by(user_id) %>%
  summarise(n = n()) 

breaks <- c(0, 1, 2, 5, 10 , 20 , 40, 80, 160, 500, max(n.rating.user$n))
labels = c("1","2","2-5", "6-10", "11-20", "21-40", "41-80", "81-160", "161-500", "501+")

n.rating.user <- n.rating.user%>%
  mutate(interval = cut(n,
                        breaks,
                        include.lowest = TRUE,
                        labels = labels,
                        right = TRUE)) %>%
  select(-n)

ratings %>%
  group_by(user_id, rating) %>%
  summarise(n = n()) %>%
  left_join(n.rating.user) %>%
  group_by(rating, interval) %>%
  summarize(n = n()) %>%
  ggplot(aes(interval, n, fill = factor(rating))) +
  geom_col(position = position_dodge2(0.5),
           binwidth = 0.5,
           color = "black") +
  scale_fill_viridis_d() +
  labs(y = "Count of users", x = "Number of rating per user", fill = "Rating")
```

![Graph2](/EDA/images/eda_number_rating_count_user.png?classes=shadow&width=60pc)

the previous figure shows that the vast majority of the users only left a few ratings. Among the ones who left only one rating, a vast majority gave a five star-rating. Whe can also notice that when the number of rating per user increases, the ratings tend be  uniformly distributed.

As we can see on th below tables, some "power-users" are present on MUBI and have rated an impressive amount of movies.

```{r}
ratings %>%
  group_by(user_id) %>%
  summarise(n = n()) %>%
  arrange(desc(n)) %>%
  top_n(10, n)
```
|     user_id    |     n    |
|----------------|----------|
| 7664747        | 3300     |
| 1452605        | 1826     |
| 8224196        | 1739     |
| 7095069        | 1710     |
| 1125150        | 1587     |
| 7637717        | 1452     |
| 7907236        | 1405     |
| 43011          | 1371     |
| 129139         | 1320     |
| 7450820        | 1313     |

```{r echo=FALSE, message=FALSE, warning=FALSE}
n.rating.movie <- ratings %>%
  group_by(url) %>%
  summarise(n = n())

breaks <- c(1, 20, 40, 60, 100, 200, 400, 600, max(n.rating.movie$n))
labels = c("1-20", "21-40", "41-60", "61-100", "101-200", "201-400", "401-600", "601+")

n.rating.movie <- n.rating.movie%>%
  mutate(interval = cut(n,
                        breaks,
                        include.lowest = TRUE,
                        labels = labels,
                        right = TRUE)) %>%
  select(-n)


ratings %>%
  group_by(url) %>%
  summarise(n = n()) %>%
  left_join(n.rating.movie) %>%
  group_by(interval) %>%
  summarize(n = n()) %>%
  ggplot(aes(interval, n)) +
  geom_col(position = position_dodge2(0.5),
           binwidth = 0.5,
           color = "black") +
  scale_fill_viridis_d() +
  labs(y = "Count of movies", x = "Number of rating per movie") +
  theme(legend.title = element_blank())
```

![Graph3](/EDA/images/eda_number_rating_count_movie.png?classes=shadow&width=60pc)


this graph shows that an important number of movies have a small amount of reviews (< 40).

The most retated movies are the following : 

```{r echo=TRUE}
ratings %>%
  group_by(url, title) %>%
  summarise(n = n())%>%
  ungroup() %>%
  select(-url) %>%
  arrange(desc(n)) %>%
  top_n(10, n)
```

|     title                    |     n    |
|------------------------------|----------|
| Drive                        | 666      |
| The Tree of   Life           | 663      |
| Interstellar                 | 613      |
| The Grand   Budapest Hotel   | 600      |
| The Lobster                  | 591      |
| Mad Max: Fury   Road         | 586      |
| Portrait of a   Lady on Fire | 574      |
| La La Land                   | 559      |
| The Revenant                 | 555      |
| Birdman                      | 554      |