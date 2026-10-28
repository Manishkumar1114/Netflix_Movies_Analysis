# Movie Dataset Analysis

An exploratory data analysis of a movie dataset of about 9,800 titles. It looks at genre distribution, vote-average groups, and popularity. The analysis is available as a Jupyter notebook for Google Colab and as a standalone Python script.

## Project Structure

```
.
├── Netflix_movies_Analysis.ipynb   # Original analysis notebook (Google Colab)
├── movie_analysis.py               # Cleaned-up script with the fixes applied
├── README.md                       # Project documentation
└── Movie_Analysis.pptx             # Presentation summarising the analysis 
```

The dataset (`mymoviedb.csv`) is not included in this repository. See [Dataset](#dataset).

## Problem Statement

A large movie catalogue is hard to read at a glance. This analysis asks three questions:

1. **Genres:** Which genres appear most often in the catalogue?
2. **Ratings:** How are vote averages spread across the titles?
3. **Popularity:** Which movies are the most and least popular, and what genres do they belong to?

## Dataset

- **File:** `mymoviedb.csv`. The notebook expects it at `/content/drive/MyDrive/mymoviedb.csv`. The script expects it in the working directory.
- **Rows:** 9,837 movies in the raw file.
- **Columns:** `Release_Date`, `Title`, `Overview`, `Popularity`, `Vote_Count`, `Vote_Average`, `Original_Language`, `Genre`, `Poster_Url`.
- The poster URLs point to `image.tmdb.org`, so the data appears to come from TMDB.

## Approach

1. **Load** the CSV with pandas (`engine='python'`, `on_bad_lines='warn'`).
2. **Inspect** data types, missing values, and duplicates (no duplicates found).
3. **Clean**
   - Drop `Overview`, `Original_Language`, and `Poster_Url`, which are not needed.
   - Convert `Release_Date` to a year, and convert `Popularity`, `Vote_Count`, and `Vote_Average` to numbers.
   - Drop rows with missing values (about 11 rows).
4. **Categorise** `Vote_Average` into four quartile groups: `not_popular`, `below_avg`, `average`, and `popular`.
5. **Reshape** the data by splitting the comma-separated `Genre` column and exploding it into one row per genre. This turns 9,826 movies into 25,551 rows.
6. **Explore** genre and rating counts with seaborn, and look up the highest and lowest popularity values.

## Key Findings

- **Drama** is the most frequent genre tag, with 3,715 entries (from the notebook).
- **Vote-average groups** are almost equal in size, at roughly 2,400 titles each (from the notebook). This is expected, because the groups are quartile-based.
- **Popularity is heavily right-skewed.** The mean is 40.3, the median is 21.2, and the standard deviation is 108.9.
- **Most popular:** *Spider-Man: No Way Home*, with a popularity of 5,083.95.
- **Least popular (after cleaning):** *Threads* and *The United States vs. Billie Holiday*, both at 13.35.

## Known Issues

Issues found in the original notebook and how they are handled:

- **Release year was broken.** In the notebook's saved output, every row shows `1970.0` for `Release_Date`, so the release-date histogram is not valid. `movie_analysis.py` fixes this with `pd.to_datetime(..., errors="coerce").dt.year`. The notebook itself has not been changed.
- **Minimum vote values were dropped.** The notebook's `pd.cut` call did not include the lowest value, so about 100 titles with the minimum vote average became NaN. `movie_analysis.py` uses `include_lowest=True`.
- **Genre counts are tags, not unique films.** A movie with three genres is counted once in each genre.
- **Rating groups are quartiles by design**, so their near-equal sizes describe the cut-offs, not the quality of the films.

## Next Steps

- Count unique titles per genre.
- Compare average rating and popularity by genre.
- Check how popularity relates to vote count.
- Confirm the rating-group counts after the script's fixes, and update the figures above if needed.

## Requirements

- Python 3.8 or later
- pandas
- matplotlib
- seaborn

Install with:

```bash
pip install pandas matplotlib seaborn
```

## How to Run

### Option 1: Google Colab (notebook)

1. Upload `mymoviedb.csv` to your Google Drive at `MyDrive/mymoviedb.csv`.
2. Open `Netflix_movies_Analysis.ipynb` in Google Colab.
3. Run all cells. The notebook mounts Google Drive and reads the CSV from that path.

### Option 2: Python script (local)

1. Place `mymoviedb.csv` in the same folder as `movie_analysis.py`.
2. Run:

```bash
python movie_analysis.py
```

The script prints summary statistics and shows the genre, vote-group, and release-year plots.

