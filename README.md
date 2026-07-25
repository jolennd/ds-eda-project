# King County Housing EDA — Client: William Rodriguez

Exploratory data analysis of King County home sales (Seattle, WA and surrounding area, May 2014–May 2015), built for a specific client rather than as a general market overview. This repo answers three research questions and turns them into concrete buying recommendations.

## The client

**William Rodriguez** — buyer, household of 2, looking to purchase **two homes**:

1. A **country** home — best possible timing to buy, and prefers non-renovated homes.
2. A **city** home — needs to move fast, wants a central location.

Working assumptions behind "country," "city," "best timing," "non-renovated," and "fast" are documented in `04_eda.ipynb`'s intro cell.

## Key insights

1. **Geographic (city vs. country).** Homes in the closest 25% of the market to downtown Seattle (< 9.8 km) sell for a median **$345/sqft**, versus **$220/sqft** for the rest of the county — a ~57% premium for central location. Large-lot ("country") properties don't cost less per sale (median $535,000 vs. $430,760 elsewhere) but do cost slightly less per square foot of living space ($233 vs. $249/sqft), since the buyer is paying for land, not finished square footage.
2. **Renovation.** Among large-lot properties, renovated homes carry a median price of $712,500 versus **$530,000** for non-renovated ones — a ~34% premium. ~97% of all homes in the dataset have no renovation on record, so non-renovated homes are both the norm and the cheaper option, which lines up well with a buyer who prefers it.
3. **Seasonality (city vs. country).** Timing patterns differ by property type. Country-home median price is lowest in **August** ($493,725) and highest in April ($575,000); city-home median price is lowest in **November** ($537,500) and highest in March ($588,750). Sales volume for both peaks in May (604 country sales, 665 city sales) and hits its lowest point in January (258 country, 223 city).
4. **Household size.** William Rodriguez is buying for a household of 2, and price rises steadily with bedroom count. 1-3 bedroom homes — comfortably enough for 2 people — make up **59.2%** of the market at a median price of **$400,000**, versus $565,000 for 4+ bedroom homes, a ~41% premium. There's no need to pay a size premium for bedrooms that won't be used, and staying in that range also covers the majority of available listings.

Size and quality features (`sqft_living`, `grade`, `sqft_above`, `sqft_living15`, `bathrooms`) are the strongest individual numeric predictors of price (all correlate > 0.5), which is the baseline the geographic and renovation findings sit on top of.

## Recommendations

1. **City home:** prioritize zip **98118** — the closest high-turnover zip code to downtown Seattle (median 7.9 km) with the highest sales volume among affordable central zips (507 sales in the window) and the lowest median price of that group ($367,500). 98103, 98115, and 98117 are close seconds if 98118 doesn't work out. Filter listings to **1-3 bedrooms**: it's plenty for a 2-person household, covers 59% of the market, and avoids paying a ~41% size premium for space that won't get used.
2. **Country home:** prioritize zip **98010** or **98014** — genuinely large-lot areas (68–85% of homes there qualify as "large lot," versus the county-wide 25% cutoff), 30+ km from downtown, with affordable medians ($360,000 and $415,000). Lean into non-renovated listings specifically, since they're both the majority of stock and meaningfully cheaper.
3. **Timing (country home):** target an **August** closing for the lowest median price on large-lot properties ($493,725). If choice matters more than shaving the last few percent off price, **May** offers about 18% more listings (604 vs. 512 sales) at only a ~9% price premium over August. (Note: this differs from the county-wide seasonal read — city homes actually bottom out in November, not August — so timing shouldn't be treated the same way for both searches.)

## Repository contents

| File / Folder | Description |
| --- | --- |
| [`01_assignment.md`](01_assignment.md) | The project brief: dataset, tasks, deliverables, and client list. |
| [`02_workflow.md`](02_workflow.md) | The recommended EDA workflow this project follows. |
| [`03_fetching_the_data_eda.ipynb`](03_fetching_the_data_eda.ipynb) | Connects to the PostgreSQL `eda` schema and joins `king_county_house_sales` to `king_county_house_details`, exporting the result to `data/eda.csv`. |
| [`04_eda.ipynb`](04_eda.ipynb) | Main analysis notebook: data understanding, cleaning, correlation analysis, and the three research questions/hypotheses above, with supporting plots. |
| [`column_names.md`](column_names.md) | Data dictionary for the King County housing columns. |
| `presentation.pptx` / `presentation.pdf` | The 10-minute client-facing slide deck summarizing methodology and recommendations (PDF is the version pushed to GitHub per the assignment brief). |
| [`data/`](data/) | Where the joined dataset (`eda.csv`) lives locally. Tracked as a folder, but its contents are git-ignored — nothing here gets pushed. |
| [`.env.example`](.env.example) | Template for database credentials — copy to `.env` and fill in your own values. |
| [`pyproject.toml`](pyproject.toml) / [`uv.lock`](uv.lock) | Project dependencies, managed with `uv`. |

## Setup

```bash
git clone <this-repo-ssh-url>
cd ds-eda-project-template
uv sync
cp .env.example .env   # fill in your King County DB credentials
```

Open the project root in VS Code (`code .`), select the `.venv` created by `uv sync` as your kernel, and run the notebooks in order: `03_fetching_the_data_eda.ipynb` first (to produce `data/eda.csv`), then `04_eda.ipynb`.

## Data source & assumptions

- Source: King County home sales dataset, joined from the `eda.king_county_house_sales` and `eda.king_county_house_details` tables (see `03_fetching_the_data_eda.ipynb` for the exact join).
- "Downtown Seattle" reference point: 47.6062° N, 122.3321° W.
- "City" = closest 25% of sales to downtown; "country" = largest 25% of lots county-wide. These are independent classifications (a home can be neither, either, or in rare cases both).
- "Non-renovated" = `yr_renovated == 0` per `column_names.md` (0/NaN indicates no renovation on record, not missing data).

## References

- [House Sales in King County dataset](https://www.kaggle.com/datasets/harlfoxem/housesalesprediction) — source dataset and column descriptions.
- [Detailed EDA with Python](https://www.kaggle.com/code/ekami66/detailed-exploratory-data-analysis-with-python) — reference notebook this project's analytical approach was adapted from.
