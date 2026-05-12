# 🧹 Tidy Messy Data using tidyr in R

![Language](https://img.shields.io/badge/Language-R-276DC3?style=flat-square&logo=r&logoColor=white)
![Library](https://img.shields.io/badge/Library-tidyr-FF6B35?style=flat-square)
![Library](https://img.shields.io/badge/Library-dplyr-2E7D32?style=flat-square)
![Library](https://img.shields.io/badge/Library-ggplot2-6A0DAD?style=flat-square)
![IDE](https://img.shields.io/badge/IDE-RStudio-75AADB?style=flat-square&logo=rstudio&logoColor=white)
![Focus](https://img.shields.io/badge/Focus-Data%20Wrangling-0052CC?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=flat-square)

---

## 📌 Project Overview

This project demonstrates **comprehensive data wrangling and tidying 
techniques** using the `tidyr` package in R — a core skill in any 
data science workflow.

It covers all major `tidyr` functions across **13 tasks** using 
**6 real-world datasets** — from Netflix content data to planetary 
science data — showing how to transform messy, untidy data into 
clean, analysis-ready formats.

**Domain:** Data Wrangling & Tidying  
**Language:** R  
**Libraries:** tidyr · dplyr · ggplot2 · readr · readxl  

---

## 📂 Project Structure

```
tidy-data-r-tidyr/
│
├── tidy-messy-data-using-tidyr-in-r.R   # Main R script (13 tasks)
│
├── Datasets/
│   ├── netflix_data.csv          # Netflix shows/movies with cast
│   ├── netflix_directors.csv     # Netflix content with directors
│   ├── movies_duration.csv       # Movie/show durations
│   ├── planet-data.csv           # Planet metrics (long format)
│   ├── planet_wide.csv           # Planet metrics (wide format)
│   ├── nukes.csv                 # Nuclear bomb counts by country/year
│   ├── DIETS.csv                 # Diet weight loss data
│   └── drink.xlsx                # Cocktail ingredients
│
├── Rplot.png                     # Sample output visualization
└── README.md
```

---

## 🛠️ Tech Stack

| Tool / Library | Purpose |
|---|---|
| **R** | Core programming language |
| **RStudio** | Development environment |
| **tidyr** | Data pivoting, separating, uniting |
| **dplyr** | Data manipulation — filter, group_by, count |
| **ggplot2** | Data visualization |
| **readr** | CSV file reading |
| **readxl** | Excel file reading |
| **tibble** | Modern data frames |

---

## 🚀 Project Workflow — 13 Tasks

---

### ✅ Task 1 — Getting Started
Install and load all required packages — tidyverse, readxl, tidyr, dplyr, ggplot2

---

### ✅ Task 2 — pivot_longer()
Transform wide data to long format:
```r
table1 %>%
  pivot_longer(-country, names_to = 'year', values_to = 'n_case')

# Legacy equivalent
table1 %>% gather(-country, key = 'year', value = 'n_cases')
```

---

### ✅ Task 3 — pivot_wider()
Transform long data to wide format using planet data:
```r
planet_df %>%
  pivot_wider(planet, names_from = "metric", values_from = "value")

# Legacy equivalent: spread()
```

---

### ✅ Task 4 — Practice: Nuclear Bombs Data
Pivot nuclear bomb counts from wide to long format:
```r
nukes_df %>%
  pivot_longer(-year, names_to = "country", values_to = "n_bombs")
```

---

### ✅ Task 5 — Plot Long Data
Replace NA values + visualize nuclear bomb counts over time:
```r
nukes_df %>%
  pivot_longer(-year, names_to = "country", values_to = "n_bombs") %>%
  replace_na(list(n_bombs = 0L)) %>%
  ggplot(aes(x = year, y = n_bombs, colour = country)) +
  geom_line()
```

---

### ✅ Task 6 — Unstack Data
Separate stacked data into columns by group:
```r
# PlantGrowth built-in dataset
unstack(PlantGrowth)

# DIETS dataset — weight loss by diet type
diet.data <- unstack(diet, WTLOSS ~ DIET)
```

---

### ✅ Task 7 — Practice Assessment
Applied tidyr knowledge to solve real-world data problems:
- Replace missing values in `mpg` dataset
- Fix `unstack()` usage for survey age-gender data

---

### ✅ Task 8 — separate_rows()
Split multi-value cells into separate rows using Netflix data:
```r
# Separate actors in cast column
net_data %>% separate_rows(cast, sep = ",")

# Find top 6 actors by appearance count
net_data %>%
  separate_rows(cast, sep = ",") %>%
  rename(actor = cast) %>%
  count(actor, sort = TRUE) %>%
  head()
```

---

### ✅ Task 9 — separate() & unite()
Split one column into multiple + merge columns:
```r
# Split duration into value + unit
movies_data %>%
  separate(duration, into = c("value", "unit"), sep = " ", convert = TRUE) %>%
  group_by(type, unit) %>%
  summarise(mean_duration = mean(value))

# Unite title and type columns
movies_data %>% unite(title_type, title, type, sep = " - ")
```

---

### ✅ Task 10 — Practice: Netflix Directors
Count movies per director using `separate_rows()` + `drop_na()`:
```r
director_df %>%
  drop_na(director) %>%
  separate_rows(director, sep = " , ") %>%
  count(director, sort = TRUE)
```

---

### ✅ Task 11 — Combine separate_rows() + separate()
Parse cocktail recipe ingredients from drink.xlsx:
```r
drink_df %>%
  separate_rows(ingredients, sep = "; ") %>%
  extract(ingredients,
          into = c("ingredient", "quantity", "unit"),
          regex = "^(.*)\\s([0-9.]+)\\s(.*)$",
          convert = TRUE) %>%
  group_by(ingredient, unit) %>%
  summarize(total_quantity = sum(quantity, na.rm = TRUE))
```

---

### ✅ Task 12 — Visualization of Tidy Data
Plot planet temperature vs distance to sun:
```r
planet_df %>%
  pivot_wider(planet, names_from = "metric", values_from = "value") %>%
  ggplot(aes(x = distance_to_sun, y = temperature)) +
  geom_point(aes(size = diameter)) +
  geom_text(aes(label = planet), vjust = -1) +
  labs(x = "Distance to sun (million km)", y = "Mean temperature (Celsius)")
```

---

### ✅ Task 13 — Portfolio Activity
Full pipeline: wide → long → wide → visualize for planet data:
```r
tidy_planet <- planet_wide %>%
  pivot_longer(-metric, names_to = "planet") %>%
  pivot_wider(planet, names_from = "metric", values_from = "value")

write_csv(tidy_planet, "tidy_planet_data.csv")
```

---

## 📊 Datasets Used

| Dataset | Domain | Key Operation |
|---|---|---|
| `netflix_data.csv` | Entertainment | `separate_rows()` — cast column |
| `netflix_directors.csv` | Entertainment | `separate_rows()` + `drop_na()` |
| `movies_duration.csv` | Entertainment | `separate()` + `unite()` |
| `planet-data.csv` | Science | `pivot_wider()` |
| `planet_wide.csv` | Science | `pivot_longer()` + `pivot_wider()` |
| `nukes.csv` | History/Politics | `pivot_longer()` + `replace_na()` |
| `DIETS.csv` | Healthcare | `unstack()` |
| `drink.xlsx` | Food | `separate_rows()` + `extract()` |

---

## 🔑 Key tidyr Functions Covered

| Function | Purpose |
|---|---|
| `pivot_longer()` | Wide → Long format |
| `pivot_wider()` | Long → Wide format |
| `gather()` | Legacy wide → long |
| `spread()` | Legacy long → wide |
| `separate_rows()` | Split multi-value cells into rows |
| `separate()` | Split one column into multiple |
| `unite()` | Merge columns into one |
| `extract()` | Extract with regex pattern |
| `replace_na()` | Replace missing values |
| `drop_na()` | Remove rows with NAs |
| `unstack()` | Separate stacked data by group |

---

## 🎓 Skills Demonstrated

- Data wrangling with tidyr — all major reshape functions
- Wide ↔ Long format transformations
- Multi-value cell parsing with `separate_rows()`
- Column splitting and merging with `separate()` + `unite()`
- Regex-based extraction with `extract()`
- Missing value handling — `replace_na()` + `drop_na()`
- Multi-dataset workflows with 6 different domains
- dplyr pipeline integration — `group_by()`, `summarise()`, `count()`
- ggplot2 visualization of tidy data
- Excel file reading with readxl
- Real-world data: Netflix, Movies, Planets, Nuclear weapons, Diets, Cocktails

---

## 📜 Certifications

| Certification | Issuer | Platform |
|---|---|---|
| IBM Data Science Professional Certificate | IBM | Coursera |
| IBM Generative AI Professional Certificate | IBM | Coursera |
| IBM RAG and Agentic AI Professional Certificate | IBM | Coursera |

---

## 🤝 Connect with Me

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Leela%20A-0077B5?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/leela-a)
[![Gmail](https://img.shields.io/badge/Gmail-attotaleelaissak@gmail.com-D14836?style=flat-square&logo=gmail&logoColor=white)](mailto:attotaleelaissak@gmail.com)
[![GitHub](https://img.shields.io/badge/GitHub-Leelaissakattaota-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/Leelaissakattaota)
