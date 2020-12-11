---
title: Cleansing
weight: 20
---

Since the average ratings of a movie can be greatly affected by a single review when its total number of ratings is low, we decide to discard the movies with less than 40 reviews.

Consecutively, we decide to remove ratings from users that gave less than 40 ratings. This allow us to keep more "precise" ratings as we can reasonably assume that the users who rate movies frequently have a richer cinematographic culture and thus are move susceptible to give more relevant ratings.

```
# Remove movies with less than 40 reviews
ratings_clean <- ratings %>% 
  group_by(url) %>%
  filter(n() >= 40) %>%
  ungroup()

# Remove user IDS with less than 40 reviews 
ratings_clean <- ratings_clean %>%
  group_by(user_id) %>%
  filter(n() >= 40) %>%
  ungroup()
```

### Movie Information

Once the rating Data Frame is cleaned, we may proceed to the movie information gathering. This is accomplished once again thanks to the MovieScrapR package and its function scrape_info. Note that this operation may take a significant amount of time.

```
urls_for_info <- unique(ratings_clean$url)

info <- scrape_info(urls_for_info)
```

The resulting data frame is of the following form : 

|     title                         |     director             |     country         |     genre                  |     ratings    |     n.ratings    |     duration    |     year    |
|-----------------------------------|--------------------------|---------------------|----------------------------|----------------|------------------|-----------------|-------------|
| Down by Law                       | Jim Jarmusch             | United States       | Comedy, Crime, Cult        | 8.3            | 7260             | 107             | 1986        |
| The Blob                          | Irvin S. Yeaworth Jr.    | United States       | Sci-Fi, Thriller, Horror   | 6.2            | 1078             | 83              | 1958        |
| The Girl with   the Dragon Tattoo | Niels Arden Oplev        | Sweden, Denmark     | Crime, Drama, Mystery      | 7              | 3232             | 153             | 2009        |
| Léon: The   Professional          | Luc Besson               | France              | Action, Crime, Thriller    | 8.1            | 21383            | 133             | 1994        |
| Love Is   Colder Than Death       | Rainer Werner Fassbinder | West Germany        | Drama, Comedy, Avant-Garde | 7.1            | 760              | 89              | 1969        |
| The Red Shoes                     | Michael Powell           | United Kingdom      | Drama, Romance, Music      | 8.8            | 4875             | 135             | 1948        |
| Blind                             | Eskil Vogt               | Norway, Netherlands | Drama                      | 7.7            | 2601             | 96              | 2014        |
| The Florida   Project             | Sean Baker               | United States       | Drama                      | 8.2            | 3697             | 112             | 2017        |
| Eva                               | Joseph Losey             | France, Italy       | Drama, Romance             | 6.9            | 1947             | 109             | 1962        |
| Shoplifters                       | Hirokazu Kore-eda        | Japan               | Drama, Crime               | 8.4            | 2695             | 121             | 2018        |

Note that the url and synopsis have been removed from this table for readibility purposes but are present in the dataframe