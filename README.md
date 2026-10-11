# NutriAI — Personalized Nutrition Analysis and Meal Planning

NutriAI is a data-driven nutrition tracking and analysis system that combines **web scraping, exploratory data analysis, machine learning, mathematical optimization, large language models (LLMs), and interactive data visualization** to support personalized meal planning and nutritional progress tracking.

The system processes nutritional information, analyzes relationships between macronutrients, estimates individual energy requirements, generates personalized meal plans, and monitors progress toward nutritional goals.

## Key Features

- **Automated data collection:** Extracts nutritional information from HTML-based web pages using Python and BeautifulSoup.
- **Data cleaning and preprocessing:** Handles missing values, prepares numerical features, and transforms raw data into a format suitable for analysis.
- **Exploratory data analysis (EDA):** Examines nutrient distributions, descriptive statistics, variability, and correlations.
- **Machine learning:** Uses multiple linear regression to estimate the caloric content of food based on its macronutrient composition.
- **Food clustering:** Applies K-Means clustering to group foods by nutritional characteristics.
- **Dimensionality reduction:** Uses Principal Component Analysis (PCA) to project nutritional data into two dimensions for visualization.
- **Personalized energy estimation:** Calculates Basal Metabolic Rate (BMR) and Total Daily Energy Expenditure (TDEE) from user characteristics and activity level.
- **Macronutrient optimization:** Uses linear programming to identify macronutrient allocations under defined nutritional constraints.
- **LLM-powered meal planning:** Constructs personalized prompts using user preferences, nutritional requirements, and available food products to generate a three-day meal plan.
- **Progress tracking:** Records nutritional intake, visualizes trends, evaluates progress toward targets, and estimates short-term trends using linear regression.
- **Interactive visualizations:** Uses Plotly to explore food clusters, nutrient relationships, dietary intake, and progress over time.

## System Architecture

NutriAI combines several data science components into a single workflow:

```text
Nutritional Data Sources
          |
          v
    Web Scraping
   BeautifulSoup
          |
          v
 Data Cleaning and Preprocessing
          |
          v
 Exploratory Data Analysis
 Statistics and Correlations
          |
          v
   Machine Learning
  Linear Regression
          |
          +--------------------+
          |                    |
          v                    v
 Food Clustering         User Profile
   K-Means              and Preferences
      |                      |
      v                      v
 PCA and Plotly          BMR / TDEE
 Visualization               |
                             v
                   Macronutrient Optimization
                   via Linear Programming
                             |
                             v
                    LLM-Based Meal Planning
                             |
                             v
                    Dietary Progress Tracking
                             |
                             v
                    Interactive Visualization
```

## 1. Data Collection and Preprocessing

NutriAI collects nutritional data from web pages using HTTP requests and HTML parsing. BeautifulSoup is used to extract relevant information from static HTML content and organize it into structured tabular data.

The collected data are inspected for missing values and cleaned before analysis. In the implemented workflow, rows containing missing values are removed using pandas' `dropna()` method.

The preprocessing stage prepares numerical nutritional features for statistical analysis, machine learning, and clustering.

## 2. Exploratory Data Analysis

The system uses descriptive statistics and visualization to investigate the nutritional dataset.

The analysis includes:

- Mean and standard deviation of nutritional variables.
- Histograms and Q–Q plots to inspect distributions.
- Correlation matrices to examine relationships between nutrients.
- Box plots to compare nutrient values and identify variability and potential outliers.

In the analyzed dataset, fat content showed a strong positive correlation with calories (**0.88**), while the correlation between calories and protein was weaker (**0.24**). These values describe the analyzed dataset and should not be interpreted as universal nutritional relationships.

## 3. Calorie Prediction with Linear Regression

NutriAI uses multiple linear regression to estimate the caloric content of food from three input features:

- Protein
- Fat
- Carbohydrates

The model is evaluated using common regression metrics:

- **R² (coefficient of determination):** Measures the proportion of variance explained by the model.
- **MSE (Mean Squared Error):** Measures the average squared prediction error.
- **RMSE (Root Mean Squared Error):** Expresses prediction error on the target variable's scale.
- **MAE (Mean Absolute Error):** Measures the average absolute prediction error.

The reported coefficient of determination on the analyzed dataset was **R² = 0.9917**. This indicates a strong fit to the evaluated data; however, generalization performance depends on the evaluation procedure, data quality, and the relationship between the input features and calorie values.

The model's coefficients describe how the predicted calorie value changes with each feature while holding the other features constant.

## 4. Food Clustering and Visualization

The system applies K-Means clustering to group food products according to their macronutrient profiles.

The workflow includes:

1. Selecting the relevant nutritional features.
2. Standardizing the features with `StandardScaler`.
3. Applying K-Means clustering.
4. Reducing the feature space from four dimensions to two using PCA.
5. Visualizing the resulting clusters with interactive Plotly charts.

Interactive charts allow users to inspect individual observations, zoom into regions of interest, and explore nutritional characteristics. Cluster descriptions can include representative nutrient values to make the results easier to interpret than numeric cluster labels alone.

## 5. Personalized Energy Requirements

NutriAI uses user-provided characteristics and preferences, including age, sex, height, weight, and physical activity level, to estimate energy requirements.

Two key quantities are calculated:

- **Basal Metabolic Rate (BMR):** An estimate of the energy required to maintain basic physiological functions at rest.
- **Total Daily Energy Expenditure (TDEE):** An estimate of daily energy expenditure that accounts for physical activity.

The activity level is represented through an activity factor. The resulting TDEE estimate provides an input for subsequent nutritional planning and personalization.

These estimates depend on the selected calculation method and the accuracy of the user-provided information.

## 6. Macronutrient Optimization

Linear programming is used to formulate a constrained macronutrient allocation problem. The optimization stage aims to identify a combination of macronutrients consistent with the target energy intake and the specified minimum intake constraints.

The resulting nutritional targets are then used as inputs for the meal-planning component.

The optimization output depends on the objective function, constraints, and nutritional assumptions defined in the implementation.

## 7. LLM-Based Personalized Meal Planning

NutriAI integrates a large language model to generate a personalized three-day meal plan.

The application prepares a structured prompt containing:

- User characteristics and estimated energy requirements.
- Dietary preferences and reported allergies.
- Available food products and their nutritional values.
- Nutritional targets and desired meal distribution.

The food-product information is serialized into a text representation that can be included in the LLM prompt. The implementation uses **zero-shot prompting**, providing instructions and relevant context without supplying example answers.

The LLM generates a meal plan intended to align with the user's preferences and nutritional targets.

LLM-generated plans are not guaranteed to satisfy every nutritional constraint or to be medically appropriate. Outputs should be checked for factual consistency, allergen safety, and nutritional adequacy before being used in practice.

## 8. Dietary Progress Tracking

The tracking component records daily nutrient intake and compares it with user-defined targets.

The system provides visualizations for:

- Daily calorie intake.
- Macronutrient distribution.
- Progress toward nutritional goals.
- Changes in body weight over time.
- Trends in individual macronutrients.

Linear regression is also used to estimate trends over the following seven days. These estimates are exploratory projections based on recorded data, not reliable guarantees of future dietary or weight outcomes.

## Technology Stack

| Area | Technologies |
|---|---|
| Data processing | Python, NumPy, pandas |
| Web scraping | HTTP requests, BeautifulSoup |
| Statistical analysis | pandas, SciPy and visualization libraries, where applicable |
| Machine learning | scikit-learn |
| Regression | Multiple linear regression |
| Clustering | K-Means |
| Dimensionality reduction | PCA |
| Feature scaling | StandardScaler |
| Optimization | Linear programming |
| Interactive visualization | Plotly |
| LLM integration | Prompt-based generation using a large language model |

The table describes the technologies and methods used in the project.

## Limitations

- Web-scraped nutritional data may contain missing values, inconsistencies, or source-specific measurement conventions.
- Removing incomplete rows can introduce selection bias if missingness is systematic.
- A high regression score on one dataset does not establish generalization to unseen food products. Evaluation on an appropriate held-out test set is necessary.
- K-Means results depend on feature selection, scaling, and the selected number of clusters.
- PCA preserves directions of greatest variance, which do not necessarily correspond to the most meaningful nutritional distinctions.
- BMR and TDEE are estimates and do not replace individualized professional assessment.
- LLM-generated meal plans can contain inaccurate nutritional information or fail to respect constraints.
- Seven-day trend projections are sensitive to the quantity and quality of recorded data.

NutriAI is a data science project for nutritional analysis and planning support. It is not a medical device and does not replace advice from a qualified healthcare or nutrition professional.
