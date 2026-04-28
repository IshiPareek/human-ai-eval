# human-ai-eval
### What Linguistic Patterns Characterize Human-Preferred AI Responses?

**Author:** Ishika Pareek   
**Tools:** Python, R, Jupyter  
**Dataset:** [LMSYS Chatbot Arena](https://huggingface.co/datasets/lmsys/chatbot_arena_conversations)

---

## Research Question

> *What linguistic patterns characterize human-preferred AI responses across different subject domains — and what do these patterns imply about the type of training data and precision needed per domain?*

---

## Overview

This project analyzes 33,000+ human preference judgments from the LMSYS Chatbot Arena dataset to understand what makes an AI response win a human evaluation. Rather than treating human preference as a black box, this study systematically examines the role of response length, domain, linguistic structure, tone, and writing style in determining which AI model humans prefer.
The project sits at the intersection of **quantitative UX research**, **human-AI interaction**, and **AI evaluation design** — applying statistical methods to a real-world human preference dataset.

---

## Dataset

- **Source:** LMSYS Chatbot Arena (Hugging Face)
- **Size:** 33,000+ conversations
- **Structure:** Head-to-head model comparisons rated by human judges
- **Models:** 20 LLMs including GPT-4, Claude-v1, Claude-instant-v1, GPT-3.5-turbo, Vicuna, LLaMA, and more
- **Languages:** Filtered to English for linguistic analysis

---

## Methodology

### Data Preparation (Python)
- Loaded and inspected raw parquet dataset
- Identified and corrected for **exposure bias** — models appeared at different frequencies, making raw win counts misleading
- Computed **win rates** (wins / total appearances) as the primary metric
- Extracted winning and losing responses into structured CSV sets
- Applied LLM-based topic classification across 35 domains
- Engineered linguistic features: response length, structure, tone, writing style

### Data Segmentation
| Set | Win Rate Range | Rows |
|-----|---------------|------|
| Set 2 | > 50% (winning models) | 8,101 |
| Set 2.1 | 25–50% (mid-tier models) | 7,648 |
| Set 2.2 | 0–25% (low-tier models) | 4,921 |
| Set 3 | Ties (both tie and tie both bad) | 3,184 |

### Statistical Analysis (R)
**Descriptive Statistics**
- Mean, median, SD of response length by model and domain
- Domain frequency distribution
- Win rate distribution across models

**Regression Analysis**
- Simple linear regression: response length → win rate (R² = 0.021)
- Multiple regression: response length + domain → win rate (R² = 0.034)
- Multiple regression: structure + tone + writing style → win rate (R² = 0.024)
- Domain-level regression: linguistic features → win rate per domain

**Logistic Regression Analysis**
To be done...
---

## Key Findings

### 1. Exposure Bias in AI Evaluation
Models are not equally represented in the dataset (SD = 1396.80, range = 4897 appearances). Raw win counts are misleading — win rate normalized for exposure is the only fair metric.

### 2. Model Performance
After normalizing for exposure:
- **GPT-4** leads with a 68% win rate
- **Claude-v1** follows at 61%
- Open-source models cluster below 25%

Only 4 of 20 models achieve >50% win rate.

### 3. Response Length is Significant but Weak
Response length is statistically significant (p < 2.2e-16) but explains only 2.1% of win rate variation. Longer responses don't reliably win.

### 4. Linguistic Features Matter More Than Length
Structure, tone, and writing style together explain 2.4% of win rate variation with several statistically significant predictors:

| Feature | Effect | p-value |
|---------|--------|---------|
| Instructional structure | **+0.021** ↑ wins more | 0.0000 |
| Markdown formatting | **+0.013** ↑ wins more | — |
| Humorous tone | **+0.004** ↑ wins more | — |
| Bullet points | **-0.016** ↓ wins less | 0.0000 |
| Conversational tone | **-0.013** ↓ wins less | 0.0000 |
| Reasoning structure | **-0.007** ↓ wins less | 0.0015 |

### 5. Domain Distribution Reflects User Demographics
The dataset is heavily skewed toward General Knowledge (28%) and Programming/Coding (18%), suggesting Chatbot Arena users are predominantly developers — which may bias what "human preference" means.

---

## Project Structure

```
human-ai-eval/
├── data/                          # Raw and processed datasets (not tracked)
├── notebooks/
│   ├── data_exploration & visualisation.ipynb   # Python: EDA, cleaning, feature engineering
│   └── day01_r_analysis.ipynb                   # R: Statistical analysis and regression
├── r-scripts/                     # Standalone R scripts
├── outputs/                       # Generated visualizations (not tracked)
├── all notes.md                   # Research log and observations
└── README.md
```

---

## Statistical Methods Applied

- Descriptive statistics (mean, median, SD)
- Win rate normalization for exposure bias correction
- Simple and multiple linear regression
- Interaction term regression
- Domain-level subgroup regression
- Response length distribution analysis
- Frequency analysis and domain segmentation

---

## Limitations

- Win rate variance is narrow within the >50% set (0.54–0.68), limiting regression power
- Linguistic features were detected via keyword matching — NLP-based methods would be more precise
- Topic classification was LLM-assisted and may contain labeling noise
- Dataset reflects Chatbot Arena user demographics, not general population preferences

---

## Next Steps

- Logistic regression: predict win/loss as binary outcome across all three sets
- Inter-rater reliability analysis (Cohen's Kappa) on tie data
- Word frequency analysis: what words appear more in winning vs losing responses per domain
- Sentiment analysis: do positive/negative toned responses rate differently?
- Final interactive visualization of all findings

---

## All visualisations + observations 
https://ishikapareek-folio.squarespace.com/projects/human-preferences-with-llm-responses


## Author

**Ishika Pareek**  
Product Designer & UX Researcher | M.S. Information Science (HCI), Cornell University  
[GitHub](https://github.com/IshiPareek) · [LinkedIn](https://linkedin.com/in/ishikapareek)

---

*Dataset: LMSYS Chatbot Arena Conversations — Zheng et al., 2023*
