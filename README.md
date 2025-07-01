# Workout Data Analysis – README

## Overview

This project analyzes **personal workout data** collected from a Polar watch over about one year. The code ingests, cleans, transforms, and models the data to explore workout patterns, calorie expenditure, and the relationship between workout characteristics and energy burned.

**Note:** The dataset itself is not included for privacy reasons.

---

## Project Structure

- **Data Source:**  
  - Place your JSON files (one per workout, exported from Polar Flow) in the `./data/` directory.
  - Only files containing `'training-session'` in their filename are used.

- **Main Steps:**  
  1. Data ingestion and cleaning  
  2. Feature engineering and transformation  
  3. Exploratory data analysis (EDA)  
  4. Statistical modeling and evaluation  
  5. Visualization (plots saved to `./img/`)

---

## Requirements

- Python 3.x
- Libraries:
    - `pandas`
    - `numpy`
    - `matplotlib`
    - `pylab`
    - `statsmodels`
    - `scipy`
    - `tabulate`
    - `IPython.display`

---

## Data Preparation

- **Features Extracted:**
    - Time: start, stop, duration
    - Sport type (e.g., walking, treadmill_running, strength_training)
    - Heart rate statistics (mean, std, quantiles, min, max)
    - Calories burned (`kiloCalories`)
    - Derived features: total workout time (minutes), workout type (strength/cardio), indoor/outdoor flag

- **Cleaning Steps:**
    - Remove irrelevant or privacy-sensitive columns (e.g., GPS)
    - Convert time columns to `datetime`
    - Extract and normalize heart rate and calorie data
    - Drop rows with missing critical values
    - Remove outliers using interquartile range

---

## Exploratory Data Analysis

- **Time Span:**  
  - 283 workouts from 2019-02-20 to 2020-03-29

- **Descriptive Statistics:**
    - **Total calories burned:** 89,421 kcal (~12 kg of body fat)
    - **Workouts by sport:**

      | Sport             | Workouts | Total kcal | Total kg fat |
      |-------------------|----------|------------|--------------|
      | walking           | 105      | 33,080     | 4.3          |
      | strength_training | 62       | 31,547     | 4.1          |
      | treadmill_running | 90       | 19,825     | 2.6          |
      | cycling           | 24       | 4,029      | 0.5          |
      | running           | 2        | 940        | 0.1          |

    - **Workout timing:** Bimodal (peaks at noon and 8 PM)
    - **Calories vs. Duration:** Strong linear relationship (correlation ≈ 0.92)

---

## Modeling

- **Regression Models:**
    - **Simple Linear Regression:**  
      - `kiloCalories ~ totalTime`  
      - \( R^2 = 0.85 \), RMSE ≈ 79 kcal
    - **By Sport:**  
      - Highest calorie burn per minute: treadmill_running (10.14 kcal/min)
    - **Multiple Regression:**  
      - `kiloCalories ~ totalTime + heartRateQ99`  
      - \( R^2 = 0.92 \), RMSE ≈ 58 kcal
    - **Mixed Linear Model:**  
      - Random effects by workout type (strength/cardio)  
      - RMSE ≈ 61 kcal

- **Model Diagnostics:**
    - Residuals are approximately normal
    - Largest prediction errors occur for strength training sessions

---

## Key Insights

- **Workout duration** is the best predictor of calories burned.
- **Treadmill running** yields the highest calorie burn rate per minute.
- **Strength training** is harder to model accurately due to greater variability.
- **Most workouts** were walking or treadmill running.

---

## Usage

1. **Place your JSON workout files** in the `./data/` directory.
2. **Run the analysis code** (see `paste.txt`).
3. **Review output plots** in the `./img/` folder and model summaries in the console.

---

## Results Summary

| Model                        | RMSE   | R²    | Notes                                  |
|------------------------------|--------|-------|----------------------------------------|
| Calories ~ Duration          | 78.8   | 0.85  | Strong baseline                        |
| Calories ~ Duration + HRQ99  | 58.0   | 0.92  | Best simple model                      |
| Mixed Model (by workout type)| 60.9   | —     | Best for grouped data (strength/cardio)|

---

## Disclaimer

- **Data privacy:** Raw data is not shared.
- **Limitations:** Calorie estimates are based on watch algorithms and may be less accurate for strength training.

---

## Author

Analysis and code by the project owner (see code comments for details).
