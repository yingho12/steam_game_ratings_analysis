
# Steam Game Ratings and Estimated Ownership

I wanted to see whether Steam games with higher user ratings tend to have more owners, and whether this relationship becomes weaker at higher ratings.

## Data Preparation

I used the [Steam Games Dataset (SteamSpy Version)](https://www.kaggle.com/datasets/fronkongames/steam-games-dataset), which contains 111,452 games.

The original CSV had a column header problem, so I corrected the column names before reading the data.

I calculated positive review share using:

`Positive / (Positive + Negative)`

Since estimated ownership is given as a range, I used the midpoint of each range as an approximate owner count.

After filtering the data, I randomly selected 20,000 games for the analysis.

## Why I Used Different Review Counts

When I first looked at the data, I noticed that many games had 100% positive ratings but very few reviews.

Of the 4,748 games with perfect ratings, **92.9% had fewer than 20 reviews**. A game with only one positive review can have the same rating as a game with hundreds of positive reviews.

Because of this, I used games with **at least 100 reviews** for the main analysis (4,995 games).

I also compared results using minimum review counts of 20, 50 and 100 to see whether the conclusions changed. The original 20,000 games were kept for exploratory data analysis (EDA).

## Analysis

I used R to:

- Explore rating, ownership and price distributions
- Fit linear and quadratic regression models
- Include price in the regression
- Compare ownership across rating groups
- Examine trends using LOESS and piecewise regression
- Compare free-to-play and paid games
- Check how review count affects the results
- Examine regression diagnostics

## Results

### 1. Ratings explained very little ownership variation

In the 100+ review sample, the Quadratic + Price model had an R² of only **0.0010**. The quadratic term was not statistically significant at the 5% level (p = 0.0932).

Ratings and price explained very little variation in estimated ownership in the main sample.

### 2. The results changed with different review cutoffs

The quadratic coefficient changed as the minimum review count increased:

| Minimum Reviews | Games | Quadratic Coefficient | p-value |
|---|---:|---:|---:|
| 20+ | 9,865 | -1.0205 | < 0.001 |
| 50+ | 6,757 | -0.5915 | 0.0090 |
| 100+ | 4,995 | -0.4717 | 0.0932 |

The quadratic relationship became weaker as the minimum review count increased. This showed that the result was sensitive to which games were included.

### 3. No clear diminishing-returns point

The rating bands from 0.85 to 0.95 had similar mean log ownership.

Games rated between 0.95 and 1.00 had lower mean log ownership, but the regression results did not support a reliable turning point.

### 4. Free-to-play vs paid games

In the 100+ review sample:

| | Free-to-Play | Paid |
|---|---:|---:|
| Games | 769 | 4,226 |
| Median estimated owners | 75,000 | 35,000 |
| Games with owner midpoint ≥ 50,000 | 68.0% | 48.2% |

Free-to-play games generally had higher estimated ownership in this sample. However, this does not mean that free pricing caused higher ownership.

## Conclusion

I originally expected to find a clear diminishing-returns relationship between ratings and ownership.

However, after checking review counts and comparing different samples, I found that the relationship was much weaker than it first appeared.

For games with at least 100 reviews, positive review share alone was not a strong indicator of estimated ownership.

There are also some limitations:

- Ownership values are estimated from ranges rather than exact counts.
- Filtering by review count changes the sample and may exclude smaller or newer games.
- Other factors such as genre, release year, publisher and marketing were not included in the models.

## Files and Tools

**File:** `steam_games_analysis.Rmd`

**Language:** R

**Libraries:** dplyr, readr, ggplot2

To run the analysis:

1. Download the dataset from the link above.
2. Save it as `steam_games.csv` in the same folder as the R Markdown file.
3. Open `steam_games_analysis.Rmd` in RStudio.
4. Knit the file to HTML.

The original dataset is not included in this repository.
