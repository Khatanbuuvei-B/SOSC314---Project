# The Framing Narratives of the “Green Data Center” Project in Mongolia

**Author:** Khatanbuuvei Battulga  
**Course:** SOSC314 — Computational Social Science, Duke Kunshan University

📄 **[Read the project report](https://khatanbuuvei-b.github.io/SOSC314---Project/)**

## Project Overview

This project examines how Mongolia’s proposed Green Data Center initiative is framed in news coverage. It combines human-coded frame assignments with dictionary matching and supervised machine learning to investigate:

1. Which frames dominate coverage of the initiative?
2. How does frame prevalence differ between government, publicly funded, and independent sources?
3. How accurately can automated approaches reproduce the human-coded frame labels?

## Data

The final sample contains 50 articles published between May 13th, 2026 and October 9th, 2026. Sources include Mongolian news websites and government portals.

The unit of analysis is one article. Articles were checked for relevance, study-period eligibility, missing text, and exact duplication.

Each article received eight binary frame labels:

- Economic opportunity
- Technological advancement
- Data sovereignty
- Environmental benefit
- Environmental risk
- Energy constraint
- Water constraint
- Governance

A value of `1` indicates that a frame is present; `0` indicates that it is absent. Articles can contain multiple frames.

Source URLs and article metadata are documented in the project data and notebooks.

## Methods

Four approaches were compared:

| Approach | Description |
|---|---|
| Majority baseline | Predicts the most common training label for each frame |
| Dictionary baseline | Matches predefined Mongolian keywords and phrases |
| Binary BoW + logistic regression | Uses unigram presence as input features |
| TF-IDF + logistic regression | Uses weighted unigrams and bigrams as input features |

The learned models use one logistic regression classifier per frame through `OneVsRestClassifier`.

Mongolian stopwords were removed from both representations. Vocabulary and TF-IDF weights were fitted within each training fold.

## Evaluation and Diagnostics

The learned models and majority baseline were evaluated using five-fold cross-validation with shuffled splits and `random_state=123`.

With 50 articles, each round used 40 articles for training and 10 for validation. Every article received one out-of-fold prediction.

Evaluation included:

- Precision, recall, and F1 for each frame
- False-positive and false-negative analysis
- An exploratory sensitivity check comparing standard and balanced class weights using the same folds

The dictionary was developed using the study sample. Its scores therefore represent development-sample agreement rather than an independent evaluation on unseen articles.

## Main Findings

- Technological advancement, economic opportunity, environmental benefit, and governance dominated the sampled coverage.
- Data sovereignty and energy constraints appeared less frequently.
- Environmental risk and water constraints were rare and appeared only among sources classified as independent.
- Binary BoW with logistic regression performed better than TF-IDF with logistic regression on most frames.
- The dictionary identified some rare-frame occurrences that both learned models missed.
- Balanced class weights did not consistently improve classification performance.

These findings describe media coverage in the sample. They do not measure public opinion or establish the initiative’s actual economic or environmental impacts.

## Repository Structure

- `data/` — Article data and metadata
- `notebooks/` — Source notebooks, including previous weekly work
- `figures/` — Exported charts
- `docs/index.html` — Rendered final report hosted through GitHub Pages
- `docs/.nojekyll` — Disables Jekyll processing for the report
- `requirements.txt` — Python dependencies
- `README.md` — Project overview and reproduction instructions

## Reproducing the Analysis

### Environment

Use Python with Jupyter Notebook or Google Colab.

Install the dependencies:

    pip install -r requirements.txt

### Run Order

1. Download or clone this repository.
2. Open `notebooks/SOSC314_Final_Project.ipynb`.
3. Load the saved article dataset from `data/sosc314_final_data.csv`.
4. Run the notebook cells from top to bottom to reproduce preprocessing, frame summaries, model evaluation, diagnostics, and figures.

Any required local data paths should be adjusted to match your environment.

## Limitations

The sample is small and unevenly distributed across frames and source types. Environmental risk and water constraint each have only three positive examples.

Human coding involves interpretation, and independent intercoder agreement was not assessed. Similar reporting across outlets may also inflate cross-validation performance when related texts appear in different folds.

Dictionary matching and sparse text representations have limited ability to distinguish context, negation, and general COP17 discussion from framing specifically related to data centers.

## References and Acknowledgment

Academic and software references are provided in the final report. Original article sources are documented in the dataset and notebooks.
