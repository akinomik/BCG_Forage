# BCG Data Science Job Simulation - Customer Churn Analysis

This repository contains exploratory data analysis (EDA) for a customer churn prediction project as part of the BCG Data Science job simulation on Forage.

## Project Overview

The analysis focuses on understanding customer churn patterns in an energy utility company using comprehensive exploratory data analysis techniques. The goal is to identify key factors that influence customer retention and churn behavior.

## Repository Structure

```
BCG/
├── Task 3 - eda_mik.ipynb                    # Main EDA notebook
├── Task 3 - feature_engineering.ipynb        # FE task notebook
├── Task 4 - modeling_starter.ipynb           # FE task notebook
├── Task 4 - Model Answer - Modeling.ipynb    # FE task notebook
├── client_data.csv                           # Customer data
├── price_data.csv                            # Pricing data
├── clean_data_after_eda.csv                  # Extra customer data for feature engineering specifically
├── data_for_predictions.csv                  # Extra customer data for RF modelling specifically
└── README.md                                 # This file
```

## Analysis Components

### 1. Data Loading & Overview
- **Datasets**: Customer data (`client_data.csv`) and pricing data (`price_data.csv`)
- **Data types analysis** using `.info()` method
- **Descriptive statistics** using `.describe()` method

### 2. Custom Visualization Functions
- **`plot_stacked_bars()`**: Creates annotated stacked bar charts for categorical analysis
- **`annotate_stacked_bars()`**: Adds value annotations to bar charts
- **`plot_distribution()`**: Generates stacked histograms with log scale for churn/retention comparison

### 3. Key Analysis Areas

#### Churn Overview
- **Overall churn rate**: ~10% (industry standard)
- Baseline understanding of customer retention vs. churn distribution

#### Consumption Analysis
- **12-month consumption patterns**
- **Gas consumption analysis** (for customers with gas contracts)
- **Recent consumption trends** (last month)
- **Imputed consumption data** analysis
- Identification of highly skewed, right-tailed distributions with significant outliers

#### Contract Type Impact
- **Gas vs. non-gas contracts**: Customers with gas are marginally less likely to churn
- Contract type as a potential retention factor

#### Financial Metrics
- **Margin analysis**: Gross vs. net power margins (virtually identical)
- **Outlier identification** in financial metrics
- Revenue impact assessment

#### Customer Behavior Patterns
- **Product portfolio analysis**: Higher churn for customers with 1 product and 4-5 products
- **Customer tenure effects**: Peak churn in year 2 (~27%), lowest churn at year 9
- **Subscribed power consumption** patterns

#### Forecasting Variables
- **Future consumption predictions**
- **Pricing forecasts** (off-peak, peak, power)
- **Discount and meter rent projections**
- Identification of threshold points where churn becomes problematic

## Key Findings

### Critical Churn Patterns
1. **Tenure-based churn**: Year 2 customers show highest churn rate (27%)
2. **Product portfolio effect**: Both single-product and multi-product (4-5) customers are high-risk
3. **Consumption thresholds**: Specific consumption levels correlate with increased churn probability
4. **Gas customers**: Slightly more loyal than electricity-only customers

### Data Quality Insights
- **Highly skewed consumption data** with long right tails
- **Significant outliers** present across consumption metrics
- **Missing data patterns** in some categorical variables (origin labels)

## 🛠️ Technical Implementation

### Libraries Used
```python
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
import matplotlib.ticker as ticker
```

### Visualization Techniques
- **Horizontal boxplots** for outlier identification
- **Stacked histograms** with log scale for distribution comparison
- **Annotated bar charts** for categorical analysis
- **Multi-panel figures** for comprehensive comparison

### Data Processing
- **Categorical grouping** and percentage calculations
- **Conditional filtering** (e.g., gas customers only)
- **Missing value handling** with `.dropna()`
- **Scientific notation formatting** control

## 📈 Business Implications

### Risk Factors Identified
1. **Early customer lifecycle** (year 2) requires enhanced retention strategies
2. **Product portfolio optimization** needed for single and high-volume customers
3. **Consumption-based segmentation** could improve targeted interventions
4. **Contract bundling** (adding gas) may improve retention

### Recommendations for Further Analysis
1. **Feature engineering** based on consumption thresholds
2. **Predictive modeling** using identified churn patterns
3. **Customer segmentation** strategies
4. **Intervention timing** optimization (focusing on year 1-2 customers)

## Next Steps

- [ ] Feature engineering based on EDA insights
- [ ] Outlier treatment
- [ ] Predictive model development
- [ ] Model validation and testing
- [ ] Business case development for retention strategies

## Requirements

```
pandas
matplotlib
seaborn
numpy (implicit dependency)
```

## About This Project

This analysis is part of the BCG Data Science & Advanced Analytics job simulation on Forage, demonstrating:
- **Comprehensive EDA methodology**
- **Business-focused data analysis**
- **Professional visualization techniques**
- **Actionable insights generation**

The project showcases real-world data science skills applied to customer analytics and churn prediction challenges faced by consulting firms like BCG.
It also demonstrates practical application of data science techniques in customer analytics and business consulting contexts.

---

## Edit after EDA & feature engineering tasks:

The business case analysis accompanying a feature engineering part has accomplished:
- ✅ Robust feature engineering - Created 31 new features based on EDA insights
- ✅ Price sensitivity analysis - Built on the Dec-Jan price difference hypothesis (to gauge its feasibility)
- ✅ Customer segmentation - Identified high-risk vs low-risk customers
- ✅ Business case validation - Quantified the discount strategy opportunity
- ✅ ROI calculation - Clear financial justification (0.6x ROI with 50% churn prevention)

The analysis shows:
- Clear targeting strategy (year-2 customers, single-product, price-sensitive)
- Quantified risk (€249K already lost, €2.2M at risk)
- Actionable recommendations (€220K discount budget, 12-18 month payback)
- Conservative assumptions that make the business case credible

In short - this analysis shows how raw data can yield actionable business insights alongside clear financial implications.

---

## Edit after RF modelling and optimisation:

RandomSearchCV method, although limited in scope versus GridSearchCV, yielded much better results, particularly re recall.

The tuned Random Forest is now catching 36.6% of churners (vs 8.7% before) - that's a 4x improvement in recall!

📊 **Current Performance Analysis:**

✅ Major improvements:

- Recall: 36.6% (was 8.7%) - Now catching over 1/3 of churners
- F1 Score: 0.284 (was 0.158) - Better balance
- Business impact: €26,520 revenue saved vs €8,620 false alarm costs = Net positive €17,900
  
⚠️ Trade-offs:

- Accuracy dropped to 82% (was 91%) - Expected with class imbalance handling and random search CV which I know produces volatile results.
- More false alarms: 431 customers (but still profitable)

We could now run XGB to compare but as this is just exercise, it doesn't need more algos thrown at the problem at hand.


The threshold optimization reveals some powerful insights:

🎯 **Key Findings from Threshold Optimization:**

Optimal Business Threshold: 0.40

- Recall jumps to 65.1% (from 36.6%) - Now catching nearly 2/3 of churners
- Net Benefit: €22,484 (vs €17,900 at default threshold)
- Trade-off: Lower precision (15.8%) but much higher business value

📊 Feature Importance Analysis - validates the EDA:

The top features perfectly align with my earlier analysis:

1. margin_gross_pow_ele & margin_net_pow_ele - Financial metrics (my margin analysis paid off)
2. cons_12m & cons_last_month - Consumption patterns (from your EDA insights)
3. forecast_meter_rent_12m - Contract/billing features
4. months_activ & months_modif_prod - Lifecycle features (my year-2 customer insight)
5. net_margin - Direct profitability measure
6. off_peak_peak_var_mean_diff - The engineered price volatility feature
