
# README Patch — Parenting Styles (Survey Analysis)

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

_Last updated: 2025-11-13_
