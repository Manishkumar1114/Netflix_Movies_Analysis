# Movie Dataset Analysis

An exploratory data analysis of a movie dataset (about 9,800 titles), looking at genre distribution, rating groups, and popularity. The analysis is in a Jupyter notebook written for Google Colab.

## Project Structure

```
.
├── Netflix_movies_Analysis.ipynb   # Main analysis notebook
├── README.md                       # Project documentation
└── Movie_Analysis.pptx             # Presentation summarising the analysis (optional)
```

The dataset (`mymoviedb.csv`) is not included in this repository. See [Dataset](#dataset) below.

## Problem Statement

A large movie catalogue is hard to read at a glance. This analysis asks three questions:

1. **Genres:** Which genres appear most often in the catalogue?
2. **Ratings:** How are vote averages spread across the titles?
3. **Popularity:** Which movies are the most and least popular, and what genres do they belong to?

## Dataset

- **File:** `mymoviedb.csv` (expected at `/content/drive/MyDrive/mymoviedb.csv` when running in Colab)
- **Rows:** 9,837 movies in the raw file
- **Columns:** `Release_Date`, `Title`, `Overview`, `Popularity`, `Vote_Count`, `Vote_Average`, `Original_Language`, `Genre`, `Poster_Url`
- The poster URLs point to `image.tmdb.org`, so the data appears to come from TMDB.

## Approach

1. **Load** the CSV with pandas (`engine='python'`, `on_bad_lines='warn'`).
2. **Inspect** data types, missing values, and duplicates (no duplicates found).
3. **Clean**
   - Drop `Overview`, `Original_Language`, and `Poster_Url`, which are not needed.
   - Drop rows with missing values (about 11 rows).
   - Convert text columns (`Vote_Count`, `Vote_Average`, `Release_Date`) to numeric types.
4. **Categorise** `Vote_Average` into four groups (`not_popular`, `below_avg`, `average`, `popular`) using quartile cut-offs.
5. **Reshape** the data by splitting the comma-separated `Genre` column and exploding it into one row per genre (9,826 movies become 25,551 rows).
6. **Explore** genre and rating counts with seaborn, and look up the highest and lowest popularity values.

## Key Findings

- **Drama** is the most frequent genre tag, with 3,715 entries.
- **Vote-average groups** are almost equal in size (2,398 to 2,467 titles each). This is expected, because the groups are quartile-based.
- **Popularity is heavily right-skewed.** The mean is 40.3, the median is 21.2, and the standard deviation is 108.9.
- **Most popular:** *Spider-Man: No Way Home*, with a popularity of 5,083.95.
- **Least popular (after cleaning):** *Threads* and *The United States vs. Billie Holiday*, both at 13.35.

## Known Issues and Limitations

- **Release year is broken.** In the saved notebook output, every row shows `1970.0` for `Release_Date`. The release-date histogram at the end of the notebook is therefore not valid. The year extraction step needs to be fixed before any time-based analysis.
- **Genre counts are tags, not unique films.** A movie tagged with three genres is counted once in each.
- **Rating groups are quartiles by design**, so their near-equal counts describe the cut-offs, not the quality of the films.

## Next Steps

- Fix the year parsing, for example with `pd.to_datetime(df['Release_Date'], errors='coerce').dt.year`, and re-run.
- Count unique titles per genre.
- Compare average rating and popularity by genre.
- Check how popularity relates to vote count.

## Requirements

- Python 3.x
- pandas
- numpy
- matplotlib
- seaborn

Install with:

```bash
pip install pandas numpy matplotlib seaborn
```

## How to Run

1. Upload `mymoviedb.csv` to your Google Drive at `MyDrive/mymoviedb.csv`.
2. Open `Netflix_movies_Analysis.ipynb` in Google Colab.
3. Run all cells. The notebook mounts Google Drive and reads the CSV from that path.

To run it locally, change `file_path` in the data-loading cell to the local path of your CSV and remove the Google Drive mount cell.

## License

Add a license of your choice here (for example, MIT).
