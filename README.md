# Housing Price Prediction in Buenos Aires

This repository shows a step-by-step workflow for modeling apartment prices in Buenos Aires. The notebooks move from a simple size-only baseline to richer models that incorporate geography and neighborhood information, finishing with a full pipeline that uses imputation and regularization.

Summary findings (short): size is the strongest predictor, location and neighborhood add meaningful improvements, and regularization helps when many categorical features are present.

---

## Data understanding

- Source: multiple CSV files in `data/` combining Buenos Aires listings.  
- Key columns: `price`, `area` (sq meters), `latitude`, `longitude`, `neighbourhood`, plus other listing attributes.  
- Initial distribution: prices are right-skewed; area typically ranges from ~30–100+ sq meters.

## Cleaning

- Missing numeric values were imputed with medians inside modeling pipelines.  
- Missing categorical values were filled with a `missing` label for one-hot encoding.  
- Extreme outliers were inspected and excluded from modeling experiments where they would distort metrics.

## Exploratory data analysis (EDA)

### Univariate
- Look at distributions for `price` and `area`. The following scatter (and fitted line) shows the relationship between area and price.

![Price vs area (scatter + linear fit)](visualizations/price%20vs%20area.png)

Key point: larger apartments generally cost more; area captures substantial variance but not all.

### Bivariate / Geographic
- Price plotted over geographic coordinates highlights spatial clusters and neighborhood-driven price differences.

![Price vs location (map)](visualizations/price%20vs%20location.png)

Key point: central and northern neighborhoods show price premiums compared with periphery.

### Neighborhood effects
- Compare price distributions or aggregated means across neighborhoods to quantify local premiums/discounts.

![Price vs neighbourhood](visualizations/price%20vs%20neigbourhood.png)

Key point: certain neighborhoods (e.g., Palermo, Recoleta) show consistently higher prices after controlling for size.

### Multivariate / Full view
- A combined visualization shows how multiple features relate to price and model outputs.

![Price vs everything](visualizations/price%20vs%20everything.png)

Key point: combining size, location, and neighborhood yields the best predictive performance; feature importance confirms the relative weights.

## Modeling

- Baseline: linear regression on `area` alone—fast, interpretable baseline.  
- Location model: add `latitude` and `longitude`—improves fit and reveals spatial trends.  
- Neighborhood model: one-hot encode `neighbourhood` for interpretable local effects.  
- Full pipeline: numerical imputation, one-hot encoding, and `Ridge` regression to control overfitting.

Evaluation: compare MAE across models (use cross-validation). In this dataset the full model reduces MAE substantially compared to the area-only baseline.

## Reproducing

1. Ensure the `visualizations/` folder is present and contains the referenced images.  
2. Open the notebooks in order: `021-price-and-size.ipynb`, `022-price-and-location.ipynb`, `023-price-and-neighborhood.ipynb`, `024-price-and-everything.ipynb`, `025-assignment.ipynb`.  
3. Install dependencies (example):

```powershell
pip install -r requirements.txt
```

or

```powershell
pip install pandas scikit-learn matplotlib seaborn plotly category_encoders ipywidgets
```

## Files

- `021-price-and-size.ipynb` — baseline and area analysis.  
- `022-price-and-location.ipynb` — geographic models and maps.  
- `023-price-and-neighborhood.ipynb` — categorical neighborhood effects.  
- `024-price-and-everything.ipynb` — full pipeline with Ridge regularization.  
- `025-assignment.ipynb` — Mexico City practice notebook.  
- `visualizations/` — folder with static PNG exports used in the README.


