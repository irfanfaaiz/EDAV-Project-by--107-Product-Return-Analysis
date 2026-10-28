# Product Return Pattern Analysis

Exploratory Data Analysis & Visualization (EDAV) — Course-End Project

An end-to-end EDA pipeline analyzing product return behaviour using NumPy, Pandas, Matplotlib, and Seaborn. The notebook walks through all five units of the EDAV syllabus (NumPy basics, Pandas foundations, Cleaning & Wrangling, Aggregation, and Visualization) applied to a single retail dataset.

---

## Dataset Overview

The project uses one CSV — `large_dataset.csv` — with 9 columns covering user-product interactions:

| Column | Type | Description |
|---|---|---|
| `User_ID` | int | Unique customer identifier |
| `Product_ID` | string | Product SKU (P100 – P999 range) |
| `Category` | string | Product category — Sports, Beauty, Home, Fashion, Electronics |
| `Price` | int | Product price in local currency |
| `Wishlist_Count` | int | Number of times this product was wishlisted |
| `Return_History` | int | Whether the user had returned items before (0 / 1) |
| `Days_to_Return` | int | Days taken to return (or -1 if invalid) |
| `Product_Rating` | float | Average rating (1.0 – 5.0) |
| `Return_Status` | int | Whether the product was returned in this record (0 / 1) |

**Size:** ~10,000 rows (adjust if your file is different)  
**Return rate:** ~50% overall

A note on `Days_to_Return`: some entries are `-1`, which we treat as invalid / missing rather than a true value. These are converted to `NaN` during the cleaning phase.

---

## Notebook Structure

The notebook has 50 short programs, 10 per unit, following the same style as the reference projects:

### Unit I — NumPy Basics
- Array construction and reshaping
- Arithmetic operations on price and wishlist arrays
- Axis reductions and percentiles per category
- Boolean masking and conditional replacement (`np.where`)
- Min-Max and Z-score normalization via broadcasting
- Clipping, peak-to-peak range
- Column / vertical stacking
- Numerical diffs and gradients
- Cumulative sums
- Index lookups with `argmax`, `argmin`, `np.where`

### Unit II — Pandas Foundations
- Schema audit with `.info()` and memory profiling
- Positional vs label indexing (`.iloc` / `.loc`)
- Multi-criteria boolean filtering
- Feature engineering (Value Score, High Wishlist flag, Price Band)
- Descriptive stats with percentiles and group-wise sorting
- Column renaming and pruning
- Value counts and cardinality checks
- Binning with `pd.cut` and `pd.qcut`
- Query engine (`.query()`)
- Vectorized string operations

### Unit III — Cleaning & Data Wrangling
- Missing value audit (`-1` → `NaN` conversion)
- Duplicate detection and string sanitization
- Datetime-like type casting and memory downcasting
- Concatenation of split batches
- Pivot ↔ Melt reshaping
- Winsorization on price
- Crosstab contingency tables with margins
- Categorical value mapping (category → sector)
- Downcasting for memory reduction
- Row-wise lambda apply for risk labelling

### Unit IV — Aggregation & Group Operations
- Return-speed bucketing on `Days_to_Return` (fast / medium / slow)
- Multi-tier groupby with custom aggregation names
- Daily bucket aggregation as a stand-in for time-series resampling
- Rolling window means on sorted prices
- Cumulative returns and rank within category
- Expanding window statistics
- EWMA smoothing on sorted wishlist counts
- Shift / lag simulation
- Pivot table with grand totals
- Group-wise Z-score anomaly detection

### Unit V — Plotting & Visualization
- Histogram + KDE of Price
- Boxplot of Price by Category
- Bar chart: return rate by category vs overall
- Scatter: Price vs Wishlist colored by return status
- Correlation heatmap
- Violin plots of ratings per category
- FacetGrid of price distributions across categories
- Grouped bar chart with confidence intervals
- Joint hexbin plot: Price vs Rating
- Bubble plot of category-level summary

---

## Key Findings

After running all 50 programs, here is what actually stands out:

- **The overall return rate sits around 50%** across all five categories. Sports and Beauty lean slightly higher; Fashion and Home lean slightly lower. The gap between best and worst is only a few percentage points.
- **Price doesn't drive returns.** The price distribution of returned items is nearly identical to non-returned items.
- **Rating doesn't drive returns.** Mean rating is essentially the same in both groups.
- **Prior return history is a weak predictor.** Users with a return history are only marginally more likely to return again — well within noise for this dataset.
- **Days-to-return is spread evenly** between 1 and 30 with no obvious clustering per category.
- **Correlation matrix confirms this.** Every correlation with `Return_Status` is under 0.1 in magnitude.

The honest takeaway: no single variable in this dataset explains returns. In a real analysis, the next step would be customer-level features (purchase frequency, tenure), product-content signals (size, brand), or multi-variable interactions like combining price and rating into a risk score.

---

## Setup & Execution

### Option 1 — Run on Google Colab (recommended)

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Upload the notebook: **File → Upload notebook → choose `.ipynb`**
3. Run the imports cell
4. Run the `files.upload()` cell and select `large_dataset.csv` when the dialog appears
   - Alternative: use the folder icon in the left sidebar → upload icon → select the CSV
5. Run all remaining cells (`Runtime → Run all`)

### Option 2 — Run locally with Jupyter

```bash
# clone
git clone https://github.com/<your-username>/Product_Return_Pattern_Analysis.git
cd Product_Return_Pattern_Analysis

# create a virtual environment (optional but recommended)
python -m venv venv
source venv/bin/activate        # on Windows: venv\Scripts\activate

# install dependencies
pip install numpy pandas matplotlib seaborn jupyter

# launch the notebook
jupyter notebook Product_Return_Pattern_Analysis.ipynb
