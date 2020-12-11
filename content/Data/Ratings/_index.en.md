---
title: Ratings
weight: 10
---
The next step is to retrieve each movie ratings and their associated user ID that will be used thereafter to build the predictions of our recommender tool. Thanks to the dedicated package MovieScrapR, we can easily retrieve the ratings using the function scrape_ratings. Note that this operation may take several hours and could be done in multiple steps.

    library(MovieScrapR)
    ratings <- scrape_ratings(urls)

The resulting Data Frame is of the following structure :

    sample_n(ratings, size = 10)
    
|     title                        |     url                                              |     user_id    |     rating    |
|----------------------------------|------------------------------------------------------|----------------|---------------|
| The Wind Will   Carry Us         | https://mubi.com/films/the-wind-will-carry-us        | 7942137        | 5             |
| Ten Minutes   Older: The Trumpet | https://mubi.com/films/ten-minutes-older-the-trumpet | 833283         | 3             |
| Alien³                           | https://mubi.com/films/alien-1992                    | 253402         | 2             |
| The   Basketball Diaries         | https://mubi.com/films/the-basketball-diaries        | 3216347        | 3             |
| This Gun for   Hire              | https://mubi.com/films/this-gun-for-hire             | 4952891        | 5             |
| In a Better   World              | https://mubi.com/films/in-a-better-world             | 6563386        | 3             |
| Goodbye,   Children              | https://mubi.com/films/au-revoir-les-enfants         | 7498190        | 3             |
| The Great   Gatsby               | https://mubi.com/films/the-great-gatsby              | 7096243        | 4             |
| Mid90s                           | https://mubi.com/films/mid-90s                       | 1015485        | 5             |
| To Have and Have   Not           | https://mubi.com/films/to-have-and-have-not          | 4088142        | 2             |