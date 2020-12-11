---
title: URLs
weight: 5
---

The first step in building the App is the gathering of the necessary data that will allow us to build a recommender system. To do so, we first retrieve movies URL figuring on [MUBI](https://mubi.com/), a global film platform that provides a hand-curated selection of films on demand and access to its international film criticism.

The below code chunk alows us to retrieve the URLs present on 416 pages of the most sought movies after on MUBI.

    url.want <- lapply(paste0("https://mubi.com/films?page=", 1:416, "&sort=wanted_sort"),
         function(url.all.movies) {
           url.all.movies %>% read_html() %>%
             html_nodes("div.film-tile-inner.u-subtle-text-shadow a.film-link") %>%
             xml_attr("href") %>%
             paste0("https://mubi.com", .)
         })
         url.want <- as.data.frame(unlist(movies.url.want), stringsAsFactors = FALSE)
         colnames(url.want) <- c("url")

For exhaustivity, we also retrieve URLs the most popular movies present on MUBI and remove any duplicates.

    # Getting URLs for most popular movies
    url.pop <-
      lapply(paste0("https://mubi.com/films?page=", 1:416, "&sort=popularity"),
             function(url.all.movies) {
               url.all.movies %>% read_html() %>%
                 html_nodes("div.film-tile-inner.u-subtle-text-shadow a.film-link") %>%
                 xml_attr("href") %>%
                 paste0("https://mubi.com", .)
             })
    
    url.pop <- as.data.frame(unlist(movies.url.pop), stringsAsFactors = FALSE)
    
    colnames(url.pop) <- c("url")
    
    # Merging the 2 DF and keeping the unique URL
    urls <- rbind(url.pop, url.want)

    urls <- unique(movies.url)
    
As of 27th october 2020, we obtained 12106 movies URLs.
