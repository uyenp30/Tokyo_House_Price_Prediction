# 🏙️🇯🇵 Tokyo House Price Prediction (Kaggle)
Achieved **2nd Place** in a Kaggle competition focused on predicting Tokyo house prices, where strategic feature engineering was the key to success.

---

## 🥈 Competition Results

- **Competition:** Tokyo Housing Price Prediction Challenge (Kaggle)
- **Rank:** **2nd Place / 19 Teams**
- **Submissions:** 7
- **Evaluation Metric:** Root Mean Squared Error (RMSE)
- **Key Challenge:** The competition was explicitly designed to reward exploratory data analysis (EDA) and feature engineering over complex model selection. My final model proves this hypothesis.

---

## 📌 Project Objectives

- Analyze a complex dataset with 105+ features to identify key drivers of Tokyo real estate prices.
- Engineer new, high-impact features from raw data (e.g., location-based aggregates, age-based features) while carefully avoiding data leakage.
- Build a robust and reproducible machine learning pipeline to preprocess data and train a predictive model.
- Optimize the model to achieve the lowest possible Root Mean Squared Error (RMSE) on unseen data.

---

## 📁 Dataset

- **Source**: [Kaggle Competition Dataset](https://www.kaggle.com/competitions/predicting-tokyo-house-prices/overview)
- **Training Data**: ~2,376 listings
- **Test Data**: ~1,168 listings
- **Features**: 105+ columns, including:
  - **Location:** `Ward`, `DistanceToStation_m`, `FloodRiskLevel`
  - **Property:** `TotalFloorArea_sqm`, `YearBuilt`, `ConstructionMaterial`
  - **Amenities:** `HasParking`, `PetFriendly`, `HasGym`
  - **Cultural:** `TatamiRoomPresent`, `NearbyShrineOrTemple`
  - **Economic:** `NeighborhoodAvgIncome_JPY`, `AverageDaysOnMarket_in_Ward`
- **Target Variable**: `Price_JPY` (Property sale price in Japanese Yen)

---

## 🛠️ Tools & Libraries

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)
![scikit-learn](https://img.shields.io/badge/Scikit--Learn-F7931E?style=flat&logo=scikitlearn&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=flat&logo=matplotlib&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-2D6EB5?style=flat&logo=seaborn&logoColor=white)

---

## 📈 Workflow Summary

### 1️⃣ Initial Feature Engineering
- **Handled Missing/Invalid Data Logically:**
  - Filled `ParkingType` as 'None' where `HasParking` was 0, correctly interpreting the data relationship.
  - Corrected `YearRenovated` by setting it to `YearBuilt` if it was 0 or invalid (i.e., before the build year).
- **Created Age-Based Features:**
  - Engineered `BuildingAge` (CurrentYear - `YearBuilt`) and `YearsSinceRenovation` (CurrentYear - `YearRenovated`) to capture property depreciation and modernization.
- **Captured Non-Linearity:**
  - Added `TotalFloorSquare_sqm_squared` to allow the linear model to capture the non-linear relationship between property size and price.

### 2️⃣ Advanced (Leak-Proof) Feature Engineering
This was the most critical step for model performance and was done *after* the train-validation split to prevent data leakage.
- Created a temporary `PricePerSqm` feature on the training set.
- Calculated **location-based aggregates** (`Ward_Avg_PricePerSqm`, `Ward_Median_BuildingAge`) *only* from the training data.
- Merged these aggregates back into the `X_train`, `X_val`, and `df_test` sets.
- Filled any new wards in the validation/test sets with the *training set's overall mean* to handle unseen categories.

### 3️⃣ Robust Preprocessing Pipeline
- Built an end-to-end `sklearn.pipeline.Pipeline` with a `ColumnTransformer` to automate all data preparation.
- **Numerical Features:** Imputed missing values with `median` and scaled using `RobustScaler` to effectively handle outliers common in real estate data.
- **Ordinal Features:** Imputed with `most_frequent` and encoded using `OrdinalEncoder` with a custom-defined order (e.g., `['Low', 'Medium', 'High']`).
- **Nominal Features:** Imputed with `most_frequent` and encoded using `OneHotEncoder` (with `drop='first'`) to create dummy variables for categorical features like `Ward`.

### 4️⃣ Modeling & Submission
- Fed the preprocessed data directly into a `LinearRegression` model, all within the same pipeline.
- Trained the model on the `X_train` and `y_train` data.
- Evaluated performance on the validation set (`X_val`, `y_val`) to confirm model accuracy.
- Retrained the final, optimized pipeline on the *entire* dataset (`X`, `y`) before predicting on `df_test` to create the final `submission.csv` file.

---

## 🧠 Results

| Model | Validation RMSE (JPY) | Competition Rank |
| :--- | :--- | :--- |
| **Linear Regression** | **~3,228,617** | **2nd Place / 19 Teams** |

✅ **Key Insight:** This project was a powerful case study in the "EDA over model complexity" hypothesis. A simple, interpretable `LinearRegression` model, when powered by meticulous data cleaning and intelligent feature engineering (especially the leakage-proof `Ward_Avg_PricePerSqm` feature), outperformed more complex, black-box models.

---

## 💼 Potential Business Impact

A model like this is directly applicable to the real estate industry:
- **For Sellers/Agents:** Accurately set competitive listing prices based on a property's specific features and its neighborhood's pricing trends.
- **For Buyers/Investors:** Identify potentially undervalued properties or forecast a property's market value post-renovation.
- **For Developers:** Analyze how features like `BuildingAge`, `TotalFloorArea_sqm`, and `YearsSinceRenovation` impact final sale price, informing future building plans and ROI calculations.

---

## ✨ Key Learnings

- **Feature Engineering is Paramount:** Creating `BuildingAge`, `YearsSinceRenovation`, and especially the `Ward_Avg_PricePerSqm` aggregates was far more impactful than any model tuning.
- **Preventing Data Leakage is Critical:** Calculating aggregates *after* the train/test split and merging them carefully is a non-negotiable step for building a valid model that generalizes to new data.
- **`sklearn` Pipelines are Essential:** For a project with over 100 features and multiple preprocessing steps (imputing, scaling, encoding), a `Pipeline` is the best tool to ensure reproducibility and prevent mistakes.
- **Interpretability Matters:** Using `LinearRegression` not only achieved a top-2 result but also provided clear insights into *why* prices were changing (e.g., the high importance of neighborhood and age).

---

## 🌱 About Me

I'm a self-taught data enthusiast and economics student passionate about using data to solve real-world problems.
This project was part of my learning journey in Data Analytics, and you can find more of my work [here](https://github.com/uyenp30/Data-Projects) 💻🌻

---

*Thank you for reading!* ✨
*If you found this useful or have feedback, feel free to connect with me on [LinkedIn](https://www.linkedin.com/in/uyen-pham-data/).*
