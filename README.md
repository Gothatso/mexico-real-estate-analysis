# Mexican Real Estate Analysis 🏠

Are property prices in Mexico City driven more by **size** or by **location**?

This project answers that question using real property listings and Python (pandas).
I built it while completing a short course at **WorldQuant University**.

## What this project covers

| Lesson | Topic | Status |
|--------|-------|--------|
| 1 | Getting to know the data: load, clean, combine, summarize | ✅ Done |
| 2 | Visualizing the data | Coming soon |
| 3 | Correlation | Coming soon |
| 4 | Price per square meter and final conclusion | Coming soon |

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