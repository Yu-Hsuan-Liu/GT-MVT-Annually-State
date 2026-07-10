# GT-MVT-Annually-State

Pulls annual state-level Google Trends data for motor vehicle theft search terms, 2011-2022. The query is `car stolen+find stolen car+report police stolen car+insurance car stolen-dream-check`.

Google Trends resamples on every request, so a single pull is noisy. The notebook repeats the full 2011-2022 pull dozens of times and writes each pass to a timestamped CSV in `data/`; averaging the runs gives a stable state-by-year table.

Needs pandas, pytrends, and selenium with the Edge WebDriver (download it separately; selenium only opens Google Trends once to grab a NID cookie for the requests). The scraper sleeps around three hours between yearly queries to stay under the rate limit, so let it run for days.

Same scraper at other resolutions: [GT-MVT-Annually-DMA](https://github.com/Yu-Hsuan-Liu/GT-MVT-Annually-DMA) and [GT-MVT-Monthly-City](https://github.com/Yu-Hsuan-Liu/GT-MVT-Monthly-City).
