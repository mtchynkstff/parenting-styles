# 🍼 Parenting Survey Analysis 

This repository contains a Jupyter Notebook that explores and visualizes responses from a parenting survey. The goal is to better understand the experiences, challenges, and needs of parents with children in two age groups: **0–24 months** and **2–12 years**.

## 📊 Overview

The notebook includes:

- **Data cleaning and transformation**
  - Multi-select questions split by semicolon
  - Age-based grouping:
    - *0–24 Months*: Includes 0–12 and 12–24 months
    - *2–12 Years*: Includes older children
- **Bar charts**
  - Response counts and percentages
  - Separated by age group for visual comparison
  - Horizontal bar charts for clarity
  - Consistent color mapping for repeated answers
- **Open-ended response handling**
  - Organized for potential qualitative analysis

## 📁 Files

- `Parenting-Survey.ipynb`: Main analysis notebook with code and visualizations  
  *(Data file not included for privacy reasons)*

## 📌 Key Features

- Clear visualization of parenting challenges, information sources, and desired product features
- Highlights differences in needs between new parents and those with older children
- Customizable charts with grouped and labeled responses
- Excludes open-ended enjoyment question from visuals, as per design choice

## How to Run

```bash
pip install -r requirements.txt
jupyter notebook "parenting-survey.ipynb"
```

## Open in Colab

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](
https://colab.research.google.com/github/mtchynkstff/parenting-styles/blob/main/parenting-survey.ipynb)

## Data Schema

| column | type | notes |
|---|---|---|
| respondent_id | string | anonymized |
| child_age_group | category | "0–24 months", "2–12 years" |
| q_sources_info | string | values separated by `;` |
| q_top_challenges | string | values separated by `;` |
| q_desired_features | string | values separated by `;` |
| submitted_at | datetime | optional |

## Embedded Figures

```markdown
![Sources](reports/figures/sources_info.png)
![Challenges](reports/figures/challenges_by_agegroup.png)
```

Absolute path version:

```markdown
![Sources](https://github.com/mtchynkstff/parenting-styles/blob/main/reports/figures/sources_info.png?raw=true)
```

## Helper Functions

```python
def explode_multiselect(df, col, sep=";"):
    s = (df[col].fillna("")
                .astype(str)
                .str.split(sep)
                .explode()
                .str.strip())
    return df.assign(**{col: s}).query(f"{col} != ''")

def percent_by_group(df, col, group_col=None):
    if group_col:
        ct = (df.groupby([group_col, col])
                .size()
                .groupby(level=0)
                .apply(lambda x: x / x.sum() * 100)
                .rename("pct")
                .reset_index())
    else:
        ct = (df[col].value_counts(normalize=True)*100)\
              .rename_axis(col).reset_index(name="pct")
    return ct
```

## 🚧 To Do

- Incorporate sentiment or thematic analysis of open-ended responses
- Develop interactive dashboard version for sharing results with stakeholders

## 🧠 Insights

Some highlights revealed by the survey:

- Parents of infants prioritize sleep and feeding support
- Those with older kids are more focused on behavior and education
- Support product interest leans heavily on flexibility and expert guidance

## 🙌 Acknowledgments

- Thanks to the parents who participated and shared their thoughts.

## 📦 Requirements

The notebook uses:

- `pandas`
- `matplotlib`
- `seaborn`

Install dependencies with:

```bash
pip install pandas matplotlib seaborn

_Last updated: 2025-11-13_
