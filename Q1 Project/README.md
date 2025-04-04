# NHL Shot Outcome Prediction Model

## Team Members
- Aidan Lee  
- Max Weinstein  
- Date: October 21, 2024

## 📌 Project Goal
This project aims to develop a multi-class classification model to predict NHL shot outcomes—whether a shot becomes a **goal**, **misses the target**, or is **blocked by the goalie**. Using data from every shot taken during the 2021–2022 NHL season, we explore how in-game context and shot-specific data influence outcomes.

### Applications:
- **Training**: Coaches can simulate various shot situations for teaching players optimal decision-making.
- **In-Game Strategy**: Live analysis of shot quality and outcomes for tactical adjustment.
- **Post-Game Review**: Analyze shot choices with players using model feedback to reinforce or adjust behaviors.

---

## 📊 Dataset Overview
- **Source**: SCORE Network Sports Data Repository  
- **Size**: 160,573 instances with 21 attributes  
- **Season**: NHL 2021–2022  
- **Target Class**: `shot_outcome` with three final classes:
  - `SHOT` – saved by the goalie  
  - `GOAL` – results in a goal  
  - `MISSED_SHOT` – misses the net

---

## 🧹 Data Preprocessing
### Removed Irrelevant Attributes:
Removed IDs, player/team names, and text descriptions that introduced noise or redundancy.

### Handled Missing Data:
- Dropped 38k+ rows missing key spatial data (`shot_distance`, `shot_angle`)
- Dropped 300+ rows with missing `strength_code`
- Imputed 93k+ missing `empty_net` values using **random sampling based on true/false distribution**

### Transformations:
- **Booleans**: Converted string `TRUE`/`FALSE` → Python `True`/`False`
- **Normalization**:
  - `game_seconds_remaining` & `period_seconds_remaining` → Min-Max Normalized  
  - `x_fixed`, `y_fixed`, `shot_distance` → Z-score Normalized
- Removed negative time values

---

## 🧪 Train/Test Split
- **Split Ratio**: 70% training, 15% validation, 15% testing  
- **Random Seed**: `20241002`  
- **Stratified Split**: Preserved class distribution across all splits

---

## 📌 Final Feature Set
| Feature                  | Type      |
|--------------------------|-----------|
| period                   | Categorical |
| period_seconds_remaining | Numeric (Normalized) |
| game_seconds_remaining   | Numeric (Normalized) |
| home_score               | Numeric |
| away_score               | Numeric |
| empty_net                | Boolean |
| strength_code            | Categorical |
| x_fixed                  | Numeric (Z-score) |
| y_fixed                  | Numeric (Z-score) |
| shot_distance            | Numeric (Z-score) |
| shot_angle               | Numeric |

**Class Attribute**: `shot_outcome` (SHOT, GOAL, MISSED_SHOT)

---

## 🧠 Model & Attribute Selection
Models and techniques used (details can be filled out further based on your report's later sections):
- Attribute selection performed using various evaluators (e.g., InfoGain, GainRatio)
- Classifiers evaluated using WEKA or Python-based ML libraries
- Performance measured with accuracy, precision, recall, F1 score

---

## 🔍 Future Work
- Incorporate spatial player positioning or puck movement (if available)
- Experiment with advanced models (e.g., XGBoost, Deep Learning)
- Deploy as an interactive tool for coaches and analysts

---
