# Mexico Real Estate: Property Price Analysis

This project analyzes property prices (houses and apartments) in Mexico using Python (pandas, matplotlib, and seaborn). It has 4 notebooks that should be run in order.

## Data

Three raw CSV files (`mexico-real-estate-1.csv`, `-2.csv`, and `-3.csv`) are cleaned and combined into `mexico-real-estate-combined-clean.csv`.

| Column | Description |
|---|---|
| `property_type` | Property type (`house` / `apartment`) |
| `state` | State |
| `lat`, `lon` | Location coordinates |
| `area_m2` | Building area (m²) |
| `price_usd` | Price in USD |

Final result: **1,736 rows, 6 columns, and no missing values**.

## Notebook Workflow

### 1. Data Wrangling (Cleaning and Combining)
- Dataset 1: remove empty rows, remove `$` and `,` from prices, then convert prices to numbers (583 rows).
- Dataset 2: convert prices from pesos to USD using a fixed exchange rate of 19 MXN/USD (the 2014 rate), then remove empty rows (571 rows).
- Dataset 3: split `lat-lon` into `lat` and `lon`, and get `state` from `place_with_parent_names` (582 rows).
- Combine all three datasets with `pd.concat` and save them as a clean CSV file.
- Average price: 115,332 USD (median: 99,262 USD). Average area: 170 m².

### 2. Data Visualization and Exploration
- Create histograms for price and area, box plots by property type and by the top 3 states, and a scatter plot of area versus price.
- Remove outliers (prices below the 5th or above the 90th percentile, and areas above the 90th percentile): the data goes from 1,736 to **1,365 rows**.
- Add a trend line with seaborn `regplot` and calculate the average price for each state.

### 3. Correlation Analysis
- Overall correlation between area and price: **0.59** (moderately positive). Coordinates (`lat`/`lon`) have almost no correlation with price.
- Correlation by property type: houses **0.72**, apartments **0.52**.
- Correlation varies by state. Extreme values (for example, Zacatecas at 1.00 and Oaxaca at -1.00) are likely due to small amounts of data.

### 4. Price per Square Meter
- Create a new feature, `price_per_m2 = price_usd / area_m2`: average of 733 USD/m² and median of 614 USD/m².
- Highest: Distrito Federal (median of 959 USD/m²). Lowest: Colima (median of 334 USD/m²).
- Price variation is greater within a state (about 78%) than between states (about 12%).
- Among state and property type combinations, apartments have the highest prices per square meter (apartments in Nuevo León: 1,425 USD/m²).

## Conclusion

Building area has the strongest relationship with price, and this relationship is stronger for houses than for apartments. Location (state) and property type also affect price per square meter, but prices still vary widely within each state. Area, property type, and location may be useful features for a price prediction model.

## Notes and Limitations
- Prices in dataset 2 use a fixed rate of 19 MXN/USD, so they are estimates.
- Removing outliers (about 21% of the data) affects the patterns shown in the charts.
- Results for states with little data should be treated with caution.

## How to Run
Run the notebooks in order (1 to 4). Make sure the raw CSV files are in the `data/` folder.
