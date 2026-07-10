# GT-MVT-Annually-State

Google Trends scraper that collects annual, state-level search interest in motor vehicle theft (MVT) terms across the United States, 2011 to 2022. The repository is part of a dissertation project that evaluates Google Trends search data as a proxy for crime reporting.

## What it does

The notebook `GT_Scrapper_Annually_States.ipynb` queries Google Trends through the `pytrends` API for the composite keyword:

```
car stolen+find stolen car+report police stolen car+insurance car stolen-dream-check
```

For each year from 2011 through 2022 it requests interest-by-region data at the `REGION` (U.S. state) resolution, drops zero-value entries, and assembles a state-by-year table. Each completed pass over all years is written to a timestamped CSV.

Because Google Trends normalizes each response from a fresh sample of searches, a single pull is noisy. The scraper therefore repeats the full 2011-2022 pull many times (the loop is configured for up to 97 runs), saving every run as its own file. Averaging across these repeated samples yields more stable estimates, and all runs are kept in this repository for reproducibility.

## Data

The `data/` folder contains the raw output of each scrape run. File names follow the pattern:

```
REGION_2011_2022_annually_YYYYMMDD_HH_MM.csv
```

where the timestamp records when the run finished. Each file has one row per U.S. state and one column per year (`MVT_GT_2011` through `MVT_GT_2022`), holding the Google Trends relative search-interest index (0-100 within each query) for the MVT keyword set. States with zero (low-volume) values in a given year are omitted from that year's column.

## Repository structure

```
GT_Scrapper_Annually_States.ipynb   Scraper notebook
data/                               Timestamped CSV output, one file per scrape run
LICENSE                             MIT license
README.md                           This file
```

## Requirements

- Python 3 with `pandas`, `numpy`, `scipy`, `pytrends`, `selenium`, `requests`, `torpy`
- Microsoft Edge plus the matching Edge WebDriver (`msedgedriver.exe`). The driver binary is not included in this repository; download the version that matches your Edge installation from Microsoft's Edge WebDriver page and place it next to the notebook (or add it to your PATH).

Selenium is used only to open Google Trends once and capture a `NID` cookie, which is then attached to the `pytrends` requests to reduce rate-limit rejections.

## How to run

1. Install the requirements and download `msedgedriver.exe` into the repository root.
2. Open `GT_Scrapper_Annually_States.ipynb` in Jupyter.
3. Adjust `start_year`, `end_year`, and the keyword list if needed (see the "Dates (From, To)" and "GT Keywords" cells).
4. Run the cells in order. The main loop under "Execute Pytrends to Pull Annually Data from Google Trends" writes one timestamped CSV per completed pass.

Note that the scraper sleeps for long, randomized intervals between requests (roughly three hours between yearly queries) to stay within Google Trends rate limits, so a full run takes days. On repeated rate-limit errors it backs off for about 24 hours before retrying.

## Related repositories

- [GT-MVT-Annually-DMA](https://github.com/Yu-Hsuan-Liu/GT-MVT-Annually-DMA): the same annual scrape at the DMA (media market) level
- [GT-MVT-Monthly-City](https://github.com/Yu-Hsuan-Liu/GT-MVT-Monthly-City): monthly scrape at the city level

## License

MIT. See `LICENSE`.
