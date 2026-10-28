# Mexican Real Estate Analysis 🏠

Are property prices in Mexico City driven more by **size** or by **location**?

This project answers that question using real property listings and Python (pandas).
I built it while completing a short course at **WorldQuant University**.

## What this project covers

| Lesson | Topic | Status |
|--------|-------|--------|
| 1 | Getting to know the data: load, clean, combine, summarize | ✅ Done |
| 2 | Visualizing the data | ✅ Done |
| 3 | Correlation | ✅ Done |
| 4 | Price per square meter and final conclusion | ✅ Done |

## Lesson 1: Getting to Know the Data

The data comes in **three CSV files**, and each one is messy in a different way.

| File | Problem | How I fixed it |
|------|---------|----------------|
| 1 | Prices are text like `"$55,000.00"`. Some rows have no coordinates. | Removed the `$` and `,`, converted to numbers, dropped rows with missing values |
| 2 | Prices are in Mexican pesos | Divided by 19 to get US dollars, dropped the old peso column |
| 3 | Latitude and longitude are in one cell. State is hidden inside a long text. | Split the text into separate `lat`, `lon` and `state` columns |

After cleaning, I checked that all three files had the same columns, stacked them into one
dataset with `pd.concat()`, saved it, and ran `.describe()` on price and area.

### What I learned
- **Tidy data:** one variable per column, one observation per row, one value per cell
- **Five checks for any new dataset:** `.head()`, `.shape`, `.info()`, `.dtypes`, `.isnull().sum()`
- **Method chaining:** doing read, clean and convert in one readable block
- **`.assign()` with `lambda`:** making or changing columns without breaking the chain
- **`pd.concat()`** and why `ignore_index=True` matters
- **Mean vs median:** housing prices are right-skewed, so a few expensive homes pull the mean up

## Lesson 2: Visualizing Housing Data

Summary numbers can hide a lot, so I plotted the data to see what is really going on.

| Chart | What I used it for | What it showed |
|-------|--------------------|----------------|
| Histograms | Shape of price and area | Both are right-skewed. A few very expensive and very large properties pull the mean up |
| Boxplots | Compare property types and the top 3 states | Typical prices differ by state, and each state has a different spread of prices |
| Scatterplot | Size vs price | Bigger properties usually cost more, but there is a lot of scatter |
| Outlier filter | Remove the cheapest 5% and the top 10% of prices, and the top 10% of areas | The main pattern becomes easier to see |
| Regression line (seaborn) | Show the average trend | Price goes up with area, but the confidence band is wide |
| Bar chart (groupby) | Mean price per state | Distrito Federal has the highest mean price |
| Small multiples | Size vs price in each state | The size-price link is different in each state |

### What I learned
- Always plot the data before trusting summary numbers
- Use Matplotlib's explicit style: `fig, ax = plt.subplots()`
- How to read a boxplot (median, box, whiskers, outliers)
- Cutting outliers with quantiles, and why I should say that I did it
- `groupby()` splits the data into groups, calculates for each, and joins the results
- Small multiples show how a relationship changes between groups
- The confidence band shows how sure the trendline is

### Early finding
Location clearly matters. Prices differ a lot between states, and the size-price relationship
is not the same in every state. I test this with numbers in Lessons 3 and 4.

## Lesson 3: Correlation and Variable Relationships

Charts showed a link between size and price. Here I measure it with numbers.

| Step | What I did | Result |
|------|------------|--------|
| Country-wide correlation | Pearson correlation of area and price | r = ___ |
| Correlation matrix + heatmap | Checked all number columns at once | Area and price have the strongest link |
| Correlation by state | Split the data by state, then calculated r for each | The link is strong in some states and weak in others |
| Correlation by property type | Houses vs apartments | Houses: r = ___, Apartments: r = ___ |
| Price per m² | Made a new column: price ÷ area | Larger properties tend to have a lower price per m² |

### What I learned
- Pearson correlation (r) gives a number from -1 to 1 for how strongly two things move together
- Correlation does not mean one thing causes the other
- Pearson r only measures straight-line relationships
- A heatmap makes a correlation matrix easy to read
- One country-wide number can hide big differences between states (Simpson's Paradox)
- States with very few listings can give misleading correlations
- Feature engineering: making a new column (`price_per_m2`) can show new patterns
- `groupby`, `.iloc`, and `.xs()` for pulling one number from a grouped correlation

### Finding
Size is related to price, but not equally everywhere. Location changes how well size
predicts price. I test this further in Lesson 4.

## Lesson 4: Price per Square Meter and Location Effects

Total price depends a lot on size. To compare locations fairly, I made a new column:
`price_per_m2 = price_usd / area_m2`.

| Step | What I did | Result |
|------|------------|--------|
| Price per m² histogram | Looked at its shape, with the median marked | Right-skewed. Median is $___ per m² |
| Stats by state | Mean, median and spread for each state | Most expensive: ___. Least expensive: ___ |
| Boxplot, top 3 vs bottom 3 states | Compared the extremes | ___ |
| Variance split | Between-state vs within-state variation in price | Between: ___%, Within: ___% |
| Property type | Apartments vs houses per m² | Apartments: $___ per m². Houses: $___ per m² |
| State × property type | Median per m² for each combination (at least 5 listings) | ___ |

### What I learned
- Making a new column (`price_per_m2`) lets me compare properties of different sizes
- Use the median to describe skewed data
- `groupby().agg()` for several statistics at once
- Splitting variation into "between groups" and "within groups"
- Small groups give unreliable results, so I filtered out groups with fewer than 5 listings
- How to turn charts and tables into a clear conclusion

## Conclusion: size or location?

Both matter, but my results point to **location** as the bigger driver of price.

- The country-wide area and price correlation is r = ___. Size helps, but leaves a lot unexplained.
- The area and price link changes from state to state (Lesson 3).
- Between-state differences make up ___% of the price variation, compared with ___% within states.
- Price per m² is very different between states: the most expensive state has a median of $___,
  while the least expensive has $___.

### Limits of this analysis
- I only have the **state**, not the neighborhood. Location inside a state matters too.
- The variance split is a **simple method**, not a full statistical test (such as ANOVA).
- The data has no year built, condition or amenities, and no sale dates.
- A fixed rate of 19 pesos per dollar was used for file 2.
- Correlation does not prove cause.

### Ideas for next steps
- Try the log of price to reduce skew
- Use Spearman correlation for the curved link between area and price per m²
- Build a regression model with state, property type and area together

## Tools used
- Python 3
- pandas
- Jupyter Notebook (in VS Code)

## How to run this project

1. Clone the repository
   ```bash
   git clone https://github.com/Gothatso/mexico-real-estate-analysis.git
   cd mexico-real-estate-analysis
   ```
2. Create and activate a virtual environment
   ```bash
   python -m venv venv
   venv\Scripts\activate        # Windows
   source venv/bin/activate     # Mac/Linux
   ```
3. Install the libraries
   ```bash
   pip install -r requirements.txt
   ```
4. Add the data files to the `data/` folder:
   `mexico-real-estate-1.csv`, `mexico-real-estate-2.csv`, `mexico-real-estate-3.csv`
   (provided in the WorldQuant University course)
5. Open `01_getting_to_know_the_data.ipynb` and click **Run All**.

## Project structure
```
├── data/                               # CSV files
├── 01_getting_to_know_the_data.ipynb   # Lesson 1
├── requirements.txt
└── README.md
```

## Credits
Course and dataset: [WorldQuant University](https://www.wqu.edu/).
Code and write-up: Gothatso.