<div align="center">

<img src="https://upload.wikimedia.org/wikipedia/commons/0/08/Netflix_2015_logo.svg" alt="Netflix Logo" width="200"/>

# 🎬 Netflix Content Analysis

**A complete end-to-end data analysis project on the Netflix titles dataset**  
*8,790 titles · Data Cleaning · EDA · 10 Netflix-Themed Visualizations*

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Pandas](https://img.shields.io/badge/Pandas-2.0-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org)
[![Matplotlib](https://img.shields.io/badge/Matplotlib-3.7-11557c?style=for-the-badge)](https://matplotlib.org)
[![Seaborn](https://img.shields.io/badge/Seaborn-0.12-4C72B0?style=for-the-badge)](https://seaborn.pydata.org)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=for-the-badge&logo=jupyter&logoColor=white)](https://jupyter.org)

</div>

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Dataset](#-dataset)
- [Project Structure](#-project-structure)
- [Data Cleaning](#-data-cleaning)
- [Exploratory Data Analysis](#-exploratory-data-analysis)
- [Visualizations](#-visualizations-preview)
- [Key Insights](#-key-insights)
- [Getting Started](#-getting-started)
- [Technologies Used](#-technologies-used)

---

## 🎯 Project Overview

This project performs a comprehensive analysis of Netflix's content library using Python. It covers the full data science workflow — from raw data ingestion and cleaning, through exploratory analysis, to publishing 10 publication-quality charts styled with the **official Netflix color palette** (`#E50914`).

> **Goal:** Understand Netflix's content strategy through data — what types of content dominate, which countries produce the most, how content has grown over time, and what audiences are targeted.

---

## 📦 Dataset

| Property | Value |
|---|---|
| **File** | [Netflix.csv](https://github.com/user-attachments/files/26836430/netflix1.csv)|
| **Rows** | 8,790 titles |
| **Columns** | 12 (show_id, type, title, director, cast, country, date_added, release_year, rating, duration, listed_in, description) |
| **Content Types** | Movies & TV Shows |
| **Date Range** | 2008 – 2021 |

---

## 🗂 Project Structure

```
netflix-analysis/
│
├── netflix_analysis.ipynb      # Main Jupyter Notebook
├── netflix1.csv                # Raw dataset
├── README.md                   # This file
│
└── plots/
    ├── plot_01_type_distribution.png
    ├── plot_02_content_per_year.png
    ├── plot_03_top_countries.png
    ├── plot_04_top_genres.png
    ├── plot_05_ratings.png
    ├── plot_06_movie_duration.png
    ├── plot_07_tv_seasons.png
    ├── plot_08_monthly_heatmap.png
    ├── plot_09_release_year_trend.png
    └── plot_10_top_directors.png
```

---

## 🧹 Data Cleaning

The raw Netflix dataset contains several quality issues that were addressed systematically:

### Missing Values
| Column | Missing Count | Strategy |
|--------|--------------|----------|
| `director` | 2,634 | Filled with `"Unknown"` |
| `country` | 831 | Filled with `"Unknown"` |
| `rating` | 10 | Filled with `"Unknown"` |
| `date_added` | 10 | Dropped (NaT after parse) |

```python
for col in ['director', 'country', 'rating']:
    df[col] = df[col].fillna('Unknown')
```

### Feature Engineering

**Date parsing** — Converting `date_added` from string to `datetime` and extracting time components:

```python
df['date_added']  = pd.to_datetime(df['date_added'].str.strip(), errors='coerce')
df['year_added']  = df['date_added'].dt.year
df['month_added'] = df['date_added'].dt.month
df['month_name']  = df['date_added'].dt.strftime('%b')
```

**Duration extraction** — Pulling the numeric part from strings like `"90 min"` or `"2 Seasons"`:

```python
df['duration_int'] = df['duration'].str.extract(r'(\d+)').astype(float)
```

**Duplicate removal & whitespace stripping:**

```python
df.drop_duplicates(inplace=True)

str_cols = df.select_dtypes('object').columns
for col in str_cols:
    df[col] = df[col].str.strip()
```

---

## 📊 Exploratory Data Analysis

### Content Split
- **Movies:** ~69.6% of all titles
- **TV Shows:** ~30.4% of all titles

### Top Producing Countries
United States leads by a large margin, followed by India and the United Kingdom.

### Genre Landscape
Genres are multi-valued (one title can belong to several). After exploding on `,` separator:
- **International Movies** and **Dramas** dominate
- **Comedies**, **Action & Adventure**, and **Documentaries** round out the top 5

### Movie Duration
- **Mean duration:** ~99 minutes
- **Median duration:** ~98 minutes
- Distribution is roughly normal with a slight right skew (a few very long films)

### TV Show Seasons
- Most TV shows (>60%) have only **1 season**
- Very few shows reach beyond 5 seasons

---

## 📈 Visualizations Preview

All charts use the **Netflix official color theme** (`#E50914` red on `#141414` dark background).

| # | Chart | Description |
|---|-------|-------------|
| 1 | 🍕 **Pie Chart** | Movies vs TV Shows split |
| 2 | 📊 **Grouped Bar** | Content added to Netflix per year (2010–2021) |
| 3 | 📊 **Horizontal Bar** | Top 10 countries by number of titles |
| 4 | 📊 **Bar Chart** | Top 15 genres by frequency |
| 5 | 📊 **Bar Chart** | Distribution of content ratings (TV-MA, PG-13, etc.) |
| 6 | 📉 **Histogram** | Movie duration distribution with mean & median lines |
| 7 | 📊 **Bar Chart** | TV Shows grouped by number of seasons |
| 8 | 🌡️ **Heatmap** | Monthly content additions (2015–2021) |
| 9 | 📈 **Line Chart** | Release year trend — Movies vs TV Shows (1990–2021) |
| 10 | 📊 **Horizontal Bar** | Top 10 most prolific Netflix directors |

---

## 💡 Key Insights

```
═══════════════════════════════════════════════════════
         🎬  NETFLIX DATASET — KEY INSIGHTS
═══════════════════════════════════════════════════════
  Total Titles         :  8,790
  Movies               :  6,131  (69.7%)
  TV Shows             :  2,659  (30.3%)
  Date Range Added     :  2008 – 2021
  Release Year Range   :  1925 – 2021
  Avg Movie Duration   :  ~99 minutes
  Top Country          :  United States
  Most Common Rating   :  TV-MA
  Most Common Genre    :  International Movies
  Peak Year Added      :  2019
═══════════════════════════════════════════════════════
```

**Strategic takeaways:**
- Netflix **heavily favors movies** over TV shows, but TV Show additions have been growing faster in recent years
- The **2019 content peak** aligns with Netflix's major global expansion push before pandemic disruptions
- **TV-MA** dominance suggests Netflix targets adult audiences as its primary demographic
- The **monthly heatmap** reveals Q4 (Oct–Dec) consistently sees the highest content additions, likely timed around holiday viewing

---

## 🚀 Getting Started

### Prerequisites

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

### Run the Notebook

```bash
# Launch Jupyter
jupyter notebook netflix_analysis.ipynb
```

> **Note:** Make sure `netflix1.csv` is in the same directory as the notebook before running.

---

## 🛠 Technologies Used

| Library | Version | Purpose |
|---------|---------|---------|
| `pandas` | 2.0+ | Data loading, cleaning, manipulation |
| `numpy` | 1.24+ | Numerical operations |
| `matplotlib` | 3.7+ | Core charting engine |
| `seaborn` | 0.12+ | Advanced visualizations (Heatmap, Barplot) |
| `collections.Counter` | stdlib | Fast genre frequency counting |

---

## ⚖️ License & Intellectual Property

**Copyright © 2026 Mahmoud Shamoun.** All rights reserved.

This project, including all code (`.ipynb`), data cleaning strategies, and custom visualizations, is the exclusive intellectual property of **Mahmoud Shamoun**.

* **Usage:** This repository is published for portfolio demonstration purposes only. 
* **Restrictions:** Unauthorized copying, modification, or redistribution of this work, in whole or in part, for commercial or personal use without prior written consent from the author is strictly prohibited.

---

*Netflix Analysis | Mahmoud Shamoun, Data Analyst Specialist*

---

<div align="center">

**⭐ Star this repo if you found it helpful!**

</div>

---

## 🎯 Final Note

This project serves as a comprehensive demonstration of data cleaning, exploratory analysis, and storytelling through visualization. Beyond just numbers, it highlights the shift in global streaming trends and the strategic move toward diverse, mature content. 

As a **Data Analyst Specialist**, I am constantly looking for ways to bridge the gap between raw data and actionable business insights. This analysis is a step toward understanding how digital platforms evolve to meet global demand.

---
