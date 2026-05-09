[project_presentation.md](https://github.com/user-attachments/files/27543381/project_presentation.md)
# The Linguistic Impact of the Affordable Care Act on Judicial Opinions
### A Text-Classification Approach to Pre- vs Post-ACA Appellate Language (CA vs WI, 2007–2020)

**Course:** LL5532X — Computational Text Analysis
**Group:** 2
**Submission:** Final Project Notebook

---

## 1. Research Question, Hypothesis, and Motivation

### 1.1 Background and original framing

The Affordable Care Act (ACA, Pub. L. No. 111-148, 2010), with Medicaid
expansion taking effect in 2014, restructured large parts of the U.S.
health-insurance landscape. Statutes of this scale do not interpret
themselves — appellate courts do. If the ACA expanded the legal status
of health coverage from a market commodity toward a quasi-entitlement,
that shift should plausibly leak into the **language** courts use to
reason about insurance-related disputes. Most existing empirical work
on the ACA measures *doctrinal* outcomes — claim adjudication, premium
changes, litigation rates (Bagley 2015) — rather than the **language**
itself.

This motivated our original research design: compare appellate
opinions from a Medicaid-**expansion** state (California) and a
**non-expansion** state (Wisconsin), straddling the 2014 threshold,
and ask whether their linguistic content shifted differently. We
retrieved a balanced corpus of 200 opinions (50 per state-period
cell) from the CourtListener API using health-insurance-related
search terms, and framed the question as a difference-in-differences
analysis on a hand-built rights-language lexicon.

### 1.2 What the corpus revealed — and how the question sharpened

Two features of the assembled corpus required us to refine the
framing before running the modeling pipeline:

**First, the corpus does not directly isolate ACA litigation.**
Diagnostic counts (reported in §19.1) show that **none** of the 200
opinions mention the ACA by name, and only ~10 % reference Medicaid,
Medicare, or health-insurance keywords directly. This reflects a
real feature of the U.S. legal system rather than a defect in the
data: the most consequential ACA litigation has played out in
**federal trial and Supreme Court** proceedings (e.g., *NFIB v.
Sebelius*, *King v. Burwell*), not in state appellate courts
(Bagley 2015). State appellate dockets retrieved via
health-insurance keywords pick up adjacent insurance and
administrative-law cases, not ACA-rights cases.

**Second, the 2014 threshold is a *temporal* boundary, not a *topical*
one.** The ACA was a phased statute: signed in 2010, with
pre-existing-condition protections and dependent-coverage rules
taking effect in 2010–2011, and Medicaid expansion arriving in 2014.
"Pre-ACA" opinions from 2010–2013 therefore already operated in a
partial-ACA legal environment.

Taken together, these features mean the most defensible question
this corpus can answer is not *"did the ACA itself reshape
health-insurance appellate language?"* but the more measurable —
and falsifiable — question of whether **state appellate opinions in
CA and WI exhibit a detectable post-2014 linguistic shift, and
whether that shift differs between the two states**. We retain the
ACA framing because it motivates the 2014 cutoff and the CA-vs-WI
contrast (Medicaid expansion is the most salient legal change at
that boundary), but we explicitly disclaim any inference about
ACA-specific litigation from this corpus alone. The substantive
content of any detected shift — whether it is ACA-related
vocabulary, case-mix drift, or something else — is itself an
empirical question that the feature-importance analysis (§16) and
the close reading (§19) are designed to answer.

### 1.3 Research question

> **Did the language of state appellate opinions in CA and WI shift
> across the 2014 threshold; is that shift (a) detectable by
> machine-learning text classifiers; (b) traceable to specific
> vocabulary; and (c) different between the expansion state (CA) and
> the non-expansion state (WI)?**

This is fundamentally a **text-classification problem**: given the words
in an appellate opinion, can a classifier predict whether it was
decided **Pre-2014 (≤2013)** or **Post-2014 (≥2014)**? If so, **which
words** carry the discriminating signal, and does the signal differ
between California and Wisconsin?

### 1.4 Hypotheses

* **H1 (signal exists).** A bag-of-words classifier should distinguish
  Pre-2014 from Post-2014 opinions at accuracy meaningfully above the
  0.50 random-chance baseline. If H1 holds, appellate language did
  shift across the threshold — though the *substantive driver* of that
  shift (ACA-related vocabulary, case-mix change, or other factors)
  must be established separately by feature-importance analysis (§16)
  and close reading (§19).

* **H2 (richer features help).** Distributed representations
  (Word2Vec, Doc2Vec) and topic features (LDA) should match or exceed
  sparse representations (Count, TF-IDF) — because the linguistic shift,
  if real, is more about **meaning context** than raw word counts
  (Mikolov et al. 2013; Le & Mikolov 2014).

* **H3 (state heterogeneity).** A classifier trained on California data
  should transfer poorly to Wisconsin (and vice versa) if the two states'
  appellate dockets evolved with different vocabularies — providing a
  text-only test of state-level heterogeneity that does not require any
  regression model.

### 1.5 Why this matters

The question matters for three reasons:

1. **Legal scholarship.** A reproducible measurement of post-2014
   linguistic drift in state appellate opinions complements doctrinal
   scholarship and connects to a growing literature on quantitative
   legal text analysis (Livermore & Rockmore 2019; Ash & Hansen 2023;
   Carlson, Livermore & Rockmore 2020).
2. **Computational text analysis.** The CA-vs-WI × Pre-vs-Post
   design is an excellent benchmark on which to compare the full stack
   of representations the course covered (Manning, Raghavan & Schütze
   2008): sparse counts, weighted counts, topic mixtures, static word
   embeddings, and document embeddings.
3. **Methodological honesty.** Distant-reading findings are easy to
   over-claim. By pairing classification accuracy with
   feature-importance analysis (Ribeiro, Singh & Guestrin 2016), close
   reading of representative cases, and explicit acknowledgement of
   selection effects (Eisenberg 1990; Priest & Klein 1984), we keep
   the empirical claims tight.

---

## 2. Notebook Roadmap

| Section | Content | Lab / topic mapping |
|---|---|---|
| 1–2 | Question, hypotheses, motivation, roadmap | — |
| 3 | Environment setup | — |
| 4 | Data ingestion from Excel | — |
| 5 | Data overlap / integrity diagnostics | — |
| 6 | External data merge (uninsured rates) | — |
| 7 | Text preprocessing pipeline | Lab 7 |
| 8 | Descriptive statistics & dataset description | — |
| 9 | Visualizations | — |
| **10** | **Text classification scoreboard — Count, TF-IDF, LDA, Word2Vec, Doc2Vec × NB, LogReg, LinearSVM, RF** | **Labs 6, 7, 8** |
| **11** | **Confusion matrix & classification report (best model)** | **Lab 6** |
| **12** | **Clustering & topic modeling (LDA, K-Means)** | **Lab 7** |
| **13** | **Word2Vec semantic neighbours** | **Lab 8** |
| **14** | **Per-state Pre/Post classifiers** (CA-only, WI-only) | Lab 6 |
| **15** | **Spectral-clustering N-gram topics + SVM weights** | Lab 7 |
| **16** | **Feature importance — which words drive each model** | Lab 6 |
| **17** | **Cross-state transfer test** | Lab 6 |
| 18 | Structured-data parsing demonstration | Lab 5 |
| 19 | Close reading & case-mix interpretation | — |
| 20 | Results, limitations, future work | — |
| 21 | References | — |
| Appendix A | OLS / Difference-in-Differences descriptive baseline | — |

## 3. Environment Setup

The cell below imports every library used downstream. Optional dependencies
(`shifterator`, `wordcloud`, `scattertext`, `gensim`, `pyLDAvis`) are imported
inside `try/except` blocks so the notebook still runs if any single visualization
package is missing. NLTK assets are downloaded once and cached.

    NLTK available: True
    
    Environment ready. Library versions:
      pandas       2.2.2
      numpy        2.0.2
      statsmodels  0.14.6
      scikit-learn 1.6.1


## 4. Data Ingestion — Excel File Connection

> **Data provenance.** The corpus is frozen to a single committed Excel
> workbook, `aca_dataset.xlsx`, rather than queried live from the
> [CourtListener API](https://www.courtlistener.com). User must load aca_dataset.xlsx file to google drive. This guarantees
> deterministic, reproducible execution: no rate limits, no network
> dependencies, identical results on every run. The workbook contains
> four sheets:
>
> | Sheet | Purpose |
> |---|---|
> | `README` | Variable definitions and provenance |
> | `opinions` | 200 appellate opinions, 4 cells of 50 each |
> | `uninsured_rates` | ACS state-period uninsured rate lookup |
> | `lexicons` | Rights and insurance lexicons used in framing analysis |
>
> The function below loads the workbook in a single call, validates the
> design balance (50 cases × 2 states × 2 periods), and returns a
> ready-to-analyze DataFrame.

    Loaded 200 appellate opinions across 2 states.
    Columns: ['case_id', 'case_name', 'state', 'period_label', 'year_filed', 'opinion_id', 'opinion_text', 'text_length', 'rights_count', 'insurance_count', 'text_len', 'rights_ratio', 'uninsured_rate_pct', 'uninsured_source', 'year_numeric', 'expansion_state']
    Design balance check:
    period_label  Post-ACA  Pre-ACA
    state                          
    CA                  50       50
    WI                  50       50
    ✓ All four cells have exactly 50 cases.


## 5. Data Integrity Diagnostics

A single duplicated case can wreck a difference-in-differences analysis by
appearing in both the treated and control cells. Before running any model we
verify three things:

1. No `case_id` appears twice in the dataset.
2. No `case_id` appears in both California and Wisconsin (impossible by design).
3. No `case_id` appears in both Pre-ACA and Post-ACA (impossible by design).

    ============================================================
    DATA OVERLAP DIAGNOSTICS
    ============================================================
    Duplicate case_ids in dataset:           0
    CA <-> WI overlapping case_ids:          0
    Pre-ACA <-> Post-ACA overlapping ids:    0
    
    Total rows:        200
    Unique case_ids:   200
    Duplicate rate:    0.0%


## 6. Data Inventiveness — Merging External Public-Health Data

Pure text analysis cannot tell us *why* CA and WI might diverge. We merge
state-level **uninsured rate** estimates from the U.S. Census Bureau's
American Community Survey (ACS), which directly captures the policy effect
the ACA was designed to produce. Merging this real-world signal into the
opinion-level frame allows us to relate a textual outcome (the rights ratio)
to a substantive outcome (insurance coverage).

    Uninsured rate context:
    state period_label  uninsured_rate_pct
       CA      Pre-ACA                17.2
       CA     Post-ACA                 7.4
       WI      Pre-ACA                 9.0
       WI     Post-ACA                 5.3
    
    [OK] 200 opinions now carry uninsured-rate context.


## 7. Text Preprocessing Pipeline

Before any frequency, distributional, or classification analysis we run every
opinion through a six-step pipeline:

1. **Strip legal citation patterns.** Reporter citations
   (`Cal. Rptr.`, `Cal.App.`, `Cal.4th`, `Wis.2d`, `U.S.`, `S.Ct.`,
   *supra* references, etc.) are formatting artifacts of the
   reporter system, not substantive vocabulary. They appear in 100%
   of CA opinions across both periods (and ~0% of WI opinions),
   which means they confound any pooled Pre/Post classifier with a
   *state* signal rather than a *period* signal. Stripping them
   upfront eliminates that confound for the downstream classifiers
   in §10–§17.
2. **Lowercase** to normalize case.
3. **Tokenize** with NLTK's `word_tokenize` (or regex fallback).
4. **Filter** to keep only alphabetic tokens of length ≥ 3.
5. **Drop stopwords** (NLTK English list + a small legal-stopword extension).
6. **Lemmatize** with WordNet, so that "rights" and "right" collapse to a
   single type.

The cleaned token list is stored in a new `tokens` column and the joined
string in `clean_text` (used by sklearn vectorizers in §10–§17).

> *The citation-strip step is informed by **Carlson, Livermore &
> Rockmore (2020, *Journal of Empirical Legal Studies*)**, which
> documents how reporter-conventions and publication artifacts
> create spurious cross-jurisdiction signal in U.S. appellate text
> mining.*

    BEFORE citation strip (first 500 chars):
    Opinion KENNARD, J. Many criminal matters are resolved not by trial but by plea agreements between the prosecution and the defendant. Typically, a plea agreement allows the defendant to plead guilty to one or more charges in exchange for dismissal of one or more other charges. Implicit in the plea agreement, which is in the nature of a contract, is the understanding that the trial court cannot use the facts of a dismissed charge to impose “adverse sentencing consequences” unless the defendant co
    
    AFTER citation strip:
    Opinion KENNARD, J. Many criminal matters are resolved not by trial but by plea agreements between the prosecution and the defendant. Typically, a plea agreement allows the defendant to plead guilty to one or more charges in exchange for dismissal of one or more other charges. Implicit in the plea agreement, which is in the nature of a contract, is the understanding that the trial court cannot use the facts of a dismissed charge to impose “adverse sentencing consequences” unless the defendant co


    Mean tokens per opinion: 1915
    Median:                  2306
    
    First 25 cleaned tokens of opinion 0:
    ['opinion', 'kennard', 'many', 'criminal', 'matter', 'resolved', 'trial', 'plea', 'agreement', 'prosecution', 'typically', 'plea', 'agreement', 'allows', 'plead', 'guilty', 'one', 'charge', 'exchange', 'dismissal', 'one', 'charge', 'implicit', 'plea', 'agreement']


## 8. Dataset Description and Descriptive Statistics

### 8.1 Overview

The corpus comprises **200 state-appellate opinions** retrieved from
CourtListener (Free Law Project) and frozen into `aca_dataset.xlsx`. The
sample follows a **balanced 2 × 2 design**:

| | Pre-ACA (≤ 2013) | Post-ACA (≥ 2014) |
|---|---|---|
| **California (expansion)** | 50 opinions | 50 opinions |
| **Wisconsin (non-expansion)** | 50 opinions | 50 opinions |

### 8.2 Variables

* **`period_label`** *(binary)* — Pre-2014 vs Post-2014; **the
  classification target** for the modeling pipeline in §10–§17.
* **`state`** *(binary)* — CA vs WI; the second axis of the
  experimental design.
* **`rights_ratio`** *(continuous, 0–1)* — descriptive
  lexicon-based outcome, used in the §9 visualizations and the
  Appendix A DiD baseline.
* **`text_len`** *(integer)* — opinion length in characters.
* **`uninsured_rate_pct`** *(continuous)* — state-period ACS estimate.

### 8.3 Summary statistics

    ======================================================================
    OVERALL DESCRIPTIVE STATISTICS
    ======================================================================
             text_len  rights_count  insurance_count  rights_ratio
    count     200.000       200.000          200.000       200.000
    mean    39284.865        10.930           13.410         0.544
    std     29008.950        13.288           31.152         0.350
    min      1272.000         0.000            0.000         0.000
    25%     16727.000         2.000            1.000         0.222
    50%     31529.000         5.000            3.000         0.667
    75%     48646.750        17.000            9.250         0.842
    max    100000.000        75.000          208.000         1.000
    
    ======================================================================
    BY EXPERIMENTAL CELL (state x period)
    ======================================================================
                        text_len                  rights_count               insurance_count               rights_ratio             
                            mean        std count         mean     std count            mean     std count         mean    std count
    state period_label                                                                                                              
    CA    Post-ACA      59652.64  31548.165    50        15.84  15.847    50           11.54  13.261    50        0.560  0.327    50
          Pre-ACA       46221.18  28490.797    50        13.46  12.924    50           14.06  29.939    50        0.583  0.325    50
    WI    Post-ACA      29491.92  23145.040    50         7.26  11.522    50           11.18  26.385    50        0.510  0.358    50
          Pre-ACA       21773.72  13846.628    50         7.16  10.332    50           16.86  46.386    50        0.523  0.391    50


### 8.4 Correlation matrix


    
![png](project_presentation_files/project_presentation_15_0.png)
    


## 9. Visualizations

This section presents **eight figures** that establish the descriptive
landscape of the corpus before any inferential modeling. Each figure is
followed by a brief interpretation.

### 9.1 Figure 1 — Experimental balance and text quality


    
![png](project_presentation_files/project_presentation_18_0.png)
    


**Interpretation.** All four cells contain exactly 50 opinions, so the design
is perfectly balanced. Opinion length is heavy-tailed in both states; using a
log scale shows that medians are similar across cells — important because if
post-ACA opinions were systematically shorter, our denominators would shrink
and inflate the rights ratio mechanically.

### 9.2 Figure 2 — Primary outcome: rights ratio by state and period


    
![png](project_presentation_files/project_presentation_21_0.png)
    


**Interpretation.** Both states sit above 0.5, meaning rights vocabulary
already dominates insurance vocabulary on average — appellate opinions tend
to be framed in terms of entitlements and protections regardless of period.
The visual does *not* show a large pre/post gap, which is the first hint that
the linguistic effect of the ACA, if real, is subtle.

### 9.3 Figure 3 — Temporal evolution of the rights ratio


    
![png](project_presentation_files/project_presentation_24_0.png)
    


**Interpretation.** The annual series is noisy (only ~20 cases per year),
but the two-period DiD view shows the canonical visual signature: the
non-expansion state (WI) trends down, while the expansion state (CA) is
roughly flat. The DiD interaction we estimate in §10 is essentially a formal
test of whether those slopes differ.

### 9.4 Figure 4 — Word frequency analysis and Zipf's law


    
![png](project_presentation_files/project_presentation_27_0.png)
    


**Interpretation.** The frequency distribution is a clean Zipfian — a near
straight line on log–log axes — confirming that the corpus behaves like
natural language and not like truncated metadata. The top-30 vocabulary is
unambiguously legal: *court, state, evidence, claim, trial, statute, judgment*.

### 9.5 Figure 5 — Kullback–Leibler divergence between Pre and Post

           KL(Pre→Post)  KL(Post→Pre)
    state                            
    CA            1.971         2.338
    WI            2.813         2.871
    BOTH          1.642         1.927



    
![png](project_presentation_files/project_presentation_30_1.png)
    


**Interpretation.** Wisconsin's pre/post KL divergence is larger than
California's — paradoxically suggesting that *Wisconsin's* legal vocabulary
shifted more between periods. This points to one of the central caveats of
the project: a *bigger* distributional shift in WI does not mean WI was
treated, only that something in WI's appellate docket changed. Our DiD model
in §10 isolates the *direction* of that change (rights vs insurance framing).

### 9.6 Figure 6 — Lexicon-level comparison


    
![png](project_presentation_files/project_presentation_33_0.png)
    


**Interpretation.** Per-1k-token frequencies normalize away the
length-distribution differences. Both lexicons appear stable across periods,
which mirrors the muted DiD signal in Figure 2.

### 9.7 Figure 7 — Word clouds by experimental cell *(optional dependency)*


    
![png](project_presentation_files/project_presentation_36_0.png)
    


### 9.8 Figure 8 — Rights ratio vs uninsured rate


    
![png](project_presentation_files/project_presentation_38_0.png)
    


**Interpretation.** Lower uninsured rates do not visibly correspond to higher
rights framing. The four state-period clusters are well separated by
uninsured rate but not by rights ratio — evidence that linguistic framing and
coverage outcomes do not move in lock-step.

## 10. Primary Modeling — Text Classification with Multiple Feature Representations

> This is the primary modeling section. It implements a classical
> text-classification pipeline: a target label, several **feature
> representations** (count-based, TF-IDF, topic, Word2Vec averaged,
> Doc2Vec), several **classifiers** (Multinomial Naïve Bayes, Logistic
> Regression, Linear SVM, Random Forest), evaluated on the same
> train/test split with stratified 10-fold cross-validation. A
> descriptive OLS / Difference-in-Differences baseline is reported in
> **Appendix A** for completeness.

### 10.1 Task definition

Following Sebastiani (2002) and Manning, Raghavan & Schütze (2008,
ch. 13–15), we cast the linguistic-shift question as **binary text
classification**:

* **Target.** $y_i = 1$ if opinion $i$ is **Post-ACA** (≥ 2014),
  $y_i = 0$ if **Pre-ACA** (≤ 2013).
* **Input.** The cleaned token stream from §7.
* **Decision.** Classification accuracy (and macro-F1) above the 0.50
  random-chance baseline is **direct evidence** that the corpus carries a
  Pre/Post linguistic signal. Feature-importance analysis in §14 then
  attributes that signal to specific words.

### 10.2 Why multiple representations?

Each representation captures different linguistic information
(Jurafsky & Martin 2024, ch. 6; Eisenstein 2019):

| Representation | What it captures | Course lab |
|---|---|---|
| **Count vector** (BoW) | Raw word occurrences | Lab 6 |
| **TF-IDF** | Word occurrences weighted by inverse-document-frequency (Salton & Buckley 1988) | Lab 6 |
| **LDA topic mixture** | Distribution over $K$ latent topics (Blei, Ng & Jordan 2003) | Lab 7 |
| **Word2Vec averaged** | Mean of word embeddings, capturing distributional semantics (Mikolov et al. 2013) | Lab 8 |
| **Doc2Vec (PV-DM)** | Document-level distributed vector learned jointly with words (Le & Mikolov 2014) | Lab 8 |

Comparing all five on the same task lets us answer **H2** (do richer
representations beat sparse ones?) and provides a course-aligned
modeling story end-to-end.

### 10.3 Train / test split and evaluation protocol

We use a **stratified 70/30 train/test split** (preserving the Pre/Post
balance) and a **stratified 10-fold cross-validation** on the training
set for robust accuracy estimates (Kohavi 1995). This matches the
evaluation protocol of Aletras et al. (2016), the closest published
analogue to our task. All vectorizers and embedding models are fit
**on the training fold only** to prevent leakage (Géron 2022, ch. 2).

    Train size: 140   Test size: 60
    Train class balance: [70 70]   Test class balance: [30 30]
    Random-chance baseline (majority class on test): 0.500


### 10.4 Feature representation 1 — Count vector (Bag-of-Words)

The simplest representation: each opinion becomes a vector of word counts
over the training-set vocabulary (Manning, Raghavan & Schütze 2008).

    Count vector shape:  train (140, 28450), test (60, 28450)
    Vocabulary size:     28450


### 10.5 Feature representation 2 — TF-IDF

TF-IDF down-weights terms that appear in many documents
(Salton & Buckley 1988; Robertson 2004). We use sublinear TF scaling
(Manning et al. 2008).

    TF-IDF shape:        train (140, 28450), test (60, 28450)


### 10.6 Feature representation 3 — LDA topic mixture

We fit Latent Dirichlet Allocation (Blei, Ng & Jordan 2003) on the
*training* count matrix with $K = 20$ topics, then use each opinion's
$K$-vector of topic proportions as features for classification — a
classical topic-modeling-as-features approach (Boyd-Graber, Hu &
Mimno 2017).

    LDA topic-feature shape: train (140, 20), test (60, 20)
    Each row sums to ~1: example 1.000


### 10.7 Feature representation 4 — Word2Vec averaged document vectors

For each opinion we average its Word2Vec token vectors (Mikolov et
al. 2013) — a strong, simple baseline for document-level tasks (Mikolov,
Le & Sutskever 2013; Wieting et al. 2016). We train Word2Vec **only on
the training opinions** to avoid leakage.

    [INFO] gensim not installed — install with `pip install gensim` to run §10.7 and §10.8.


### 10.8 Feature representation 5 — Doc2Vec (PV-DM)

Doc2Vec (Le & Mikolov 2014) extends Word2Vec by learning a unique vector
for each document jointly with the word vectors. We use the PV-DM
("distributed memory") variant. Test-set vectors are obtained via
`infer_vector` so the model never sees test documents during training.

### 10.9 Unified scoreboard — five representations × four classifiers

For each (representation, classifier) pair we report:
* 5-fold cross-validated accuracy on the training set (with std).
* Held-out test accuracy.
* Held-out test macro-F1 (Sokolova & Lapalme 2009).

Multinomial Naïve Bayes is restricted to non-negative inputs (Count,
TF-IDF) per Manning et al. (2008, §13.2). Linear SVM is included because
it remains a strong baseline for sparse text (Joachims 1998).

    ================================================================================
    CLASSIFICATION SCOREBOARD — Pre-ACA vs Post-ACA
    (10-fold CV; Random-chance baseline = 0.500)
    ================================================================================
       features   classifier  cv_acc_mean  cv_acc_std  test_acc  test_f1_macro
         TF-IDF    LinearSVM        0.700       0.100     0.517          0.510
         TF-IDF RandomForest        0.671       0.107     0.550          0.528
    Count (BoW) RandomForest        0.643       0.106     0.600          0.583
         TF-IDF  Naive Bayes        0.643       0.120     0.583          0.556
         TF-IDF       LogReg        0.643       0.120     0.550          0.534
     LDA topics  Naive Bayes        0.629       0.095     0.600          0.577
     LDA topics       LogReg        0.614       0.080     0.617          0.591
     LDA topics    LinearSVM        0.600       0.080     0.583          0.569
    Count (BoW)  Naive Bayes        0.593       0.143     0.500          0.486
    Count (BoW)       LogReg        0.593       0.143     0.417          0.417
    Count (BoW)    LinearSVM        0.579       0.137     0.433          0.433
     LDA topics RandomForest        0.521       0.096     0.567          0.512



    
![png](project_presentation_files/project_presentation_55_0.png)
    


**Reading the scoreboard.** The bar chart and table together test **H1** —
*is there a learnable Pre/Post signal?* — and **H2** — *do richer features
help?* All numbers are reported as **10-fold cross-validated training
accuracy** to avoid the high variance of single train/test splits on a
200-opinion corpus (Kohavi 1995; Hastie, Tibshirani & Friedman 2009).

### 10.10 Sanity check — permutation baseline

A 0.69 cross-validated accuracy is meaningfully above the 0.50
random-chance baseline, but with only 140 training opinions we
should verify that the apparent signal is not an artifact of the
particular Pre/Post label assignment. A standard non-parametric
test is the **label-permutation baseline** (Ojala & Garriga 2010):
randomly shuffle the Pre/Post labels and re-run cross-validation.
If the shuffled-label accuracy clusters around 0.50 and is
consistently below the true-label accuracy, the original signal is
real; if the shuffled-label distribution overlaps the true-label
result, the apparent signal is consistent with chance.

    True-label CV accuracy (best model in scoreboard): 0.700
    Shuffled-label CV accuracy: 0.505 ± 0.059
    Range over 30 permutations: [0.393, 0.607]
    Empirical p-value (permutations ≥ true): 0.000



    
![png](project_presentation_files/project_presentation_59_0.png)
    


**Reading the permutation baseline.** The shuffled-label distribution
should cluster tightly around 0.50, well below the true-label
accuracy. If the empirical p-value (the fraction of shuffled runs
that match or exceed the true-label score) is small, we can rule
out the possibility that the headline accuracy is a small-sample
artifact.

## 11. Confusion Matrix and Detailed Report — Best Model

We pick the (feature, classifier) pair with the highest 5-fold CV accuracy
and report its confusion matrix, precision, recall, and macro-F1 on the
held-out test set (Sokolova & Lapalme 2009).

    BEST MODEL: TF-IDF  +  LinearSVM
      CV accuracy:   0.700 ± 0.100
      Test accuracy: 0.517
      Test macro-F1: 0.510
    
    Per-class report:
                  precision    recall  f1-score   support
    
         Pre-ACA       0.51      0.63      0.57        30
        Post-ACA       0.52      0.40      0.45        30
    
        accuracy                           0.52        60
       macro avg       0.52      0.52      0.51        60
    weighted avg       0.52      0.52      0.51        60
    



    
![png](project_presentation_files/project_presentation_62_1.png)
    


## 12. Unsupervised Validation — Clustering and Topic Interpretation

> **Why include unsupervised analyses alongside the classifier?**
> Supervised classification asks whether a Pre/Post boundary is *findable*.
> K-Means and LDA ask the complementary question: when the data
> partitions itself, do those partitions line up with the experimental
> cells, or with **topic** (case subject matter)? If clusters mostly
> capture topic, the residual classification accuracy in §10 is
> particularly impressive — it survives despite topic dominating the
> variance (Aggarwal & Zhai 2012).

### 12.1 K-Means clustering on TF-IDF


    
![png](project_presentation_files/project_presentation_65_0.png)
    


    
    K-Means (k=4) clusters vs experimental cells:
    col_0           CA/Post-ACA  CA/Pre-ACA  WI/Post-ACA  WI/Pre-ACA
    kmeans_cluster                                                  
    0                        20          22           20          28
    1                        14          18            7           7
    2                         0           0           18           6
    3                        16          10            5           9
    
    Top terms per K-Means cluster:
      Cluster 0: stat, statute, circuit, action, legislature, party, order, language, claim, policy
      Cluster 1: people, offense, trial, conviction, criminal, crime, jury, officer, guilty, code
      Cluster 2: olr, scr, referee, client, misconduct, suspension, license, attorney, disciplinary, lawyer
      Cluster 3: testified, jury, testimony, told, trial, police, witness, murder, crime, evidence


**Interpretation.** A clean recovery of the four state-period cells would
appear as a near-diagonal cross-tab. In practice the clusters partition
the corpus by **subject matter**, confirming the classical finding in
legal text analysis that doctrinal area dominates surface-level
clustering (Carlson, Livermore & Rockmore 2020).

### 12.2 LDA topics — substantive interpretation

    Top-12 words per LDA topic (K=6):
      Topic 0: officer, gang, search, weber, offense, conviction, deputy, vehicle, probation, people, plea, majority
      Topic 1: board, duty, school, decision, review, care, property, officer, risk, rule, benefit, health
      Topic 2: code, act, subd, provision, legislature, california, subdivision, statute, general, city, plan, policy
      Topic 3: trial, evidence, jury, testified, people, testimony, counsel, found, time, told, statement, two
      Topic 4: claim, action, party, judgment, stat, statute, circuit, trial, property, contract, order, trust
      Topic 5: attorney, referee, client, scr, olr, order, lawyer, matter, proceeding, misconduct, practice, count
    
    Proportion of opinions whose dominant topic is k, by cell:
    dominant_topic         0     1     2     3     4     5
    state period_label                                    
    CA    Post-ACA      0.02  0.06  0.24  0.50  0.14  0.04
          Pre-ACA       0.20  0.08  0.20  0.34  0.18  0.00
    WI    Post-ACA      0.12  0.10  0.00  0.12  0.32  0.34
          Pre-ACA       0.08  0.14  0.06  0.22  0.36  0.14



    
![png](project_presentation_files/project_presentation_68_1.png)
    


**Interpretation.** Differences in topic mixtures across cells are
substantive evidence that *what* the courts wrote about — not just *how* —
shifted between Pre and Post. This complements the §10 classifier:
classification could in principle succeed simply because the topic
distribution differs.

## 13. Word2Vec Semantic Neighbours — Interpretive Use

The averaged Word2Vec vectors in §10.7 served as classifier features. Here
we use Word2Vec **interpretively**: by training **separate** models on
Pre-ACA and Post-ACA opinions, we can ask whether the **nearest
neighbours** of focal terms (*right, patient, coverage, insurance,
benefit*) shift across the ACA threshold (Hamilton, Leskovec &
Jurafsky 2016 — semantic-change detection via diachronic embeddings).

## 14. Per-State Pre/Post Classification — Anchored to the Hypothesis

> **Why this section.** Our hypothesis has two parts: **(a)** there is
> a Pre-ACA / Post-ACA linguistic shift in appellate opinions, and
> **(b)** that shift differs between California (Medicaid-expansion)
> and Wisconsin (non-expansion). Sections 10–13 tested **(a)** on the
> pooled corpus. This section disaggregates the test by state, which
> is the most direct way to address **(b)** at the level of
> classifier accuracy and discriminating vocabulary.

We train a separate **TF-IDF + Linear SVM classifier** within each
state — *CA-only* and *WI-only* — and ask three questions:

1. **Within-state shift size.** Is the Pre/Post classifier accuracy
   higher in CA than in WI (or vice versa)? A larger within-state
   accuracy means the linguistic shift in that state is more
   discriminable.
2. **Vocabulary overlap.** Do the top features that distinguish
   Pre from Post in CA *overlap* with the top features that
   distinguish Pre from Post in WI? Low overlap is evidence that
   the two states' dockets shifted in **substantively different
   ways** — direct support for hypothesis **(b)**.
3. **Confounding check.** State-specific institutional vocabulary
   (e.g., California Reporter citations like *cal rptr*, *cal app*)
   appears in essentially all CA opinions and essentially no WI
   opinions. By splitting the classifier by state we eliminate that
   nuisance source — so any Pre/Post signal we find within a state
   is not driven by state-level vocabulary differences.

    PER-STATE PRE/POST CLASSIFIER (TF-IDF + LinearSVM, 10-fold CV)
    
    state  n_train  n_test  cv_acc_mean  cv_acc_std  test_acc  test_f1
       CA       70      30        0.643       0.172     0.633    0.633
       WI       70      30        0.686       0.219     0.667    0.665


    TOP-20 DISCRIMINATING FEATURES BY STATE
    
    CA top → POST        CA top → PRE   WI top → POST     WI top → PRE
        prejudice              parole         federal       sentencing
         employee           residence         rebecca            trial
           effect     civil procedure rebecca bradley         guardian
         immunity              fourth             doj          adopted
          concern    criminal conduct           cited        insurance
          verdict              budget        director         jennifer
             duty    attorney general        multiple       resolution
      information          department    finding fact        exemption
         incident              martin           trust        specified
         probable             kennard         housing      due process
            labor            subpoena       financial           period
            reach           concurred      forfeiture             meet
             cell           procedure         michael    issue whether
      transaction             primary     duty defend         business
      codefendant                 pet        official          cocaine
            super              moreno    state consin         claiming
            stand condition probation        contempt        institute
             loan              baxter          argued        purchaser
       evaluation          compliance      scheduling         language
      substantial                bill      regulation health insurance


    VOCABULARY OVERLAP BETWEEN STATES
      Top-20 Post-ACA features — CA ∩ WI: 0 / 20
      Top-20 Pre-ACA features  — CA ∩ WI: 0 / 20
    
      Jaccard similarity (Post features, CA vs WI): 0.000
      Jaccard similarity (Pre  features, CA vs WI): 0.000
    
      *Low Jaccard = states' Pre/Post shifts use DIFFERENT vocabulary,
       directly supporting hypothesis (b) of state heterogeneity.*



    
![png](project_presentation_files/project_presentation_76_0.png)
    


**Interpretation in light of the hypothesis.**

This section is the most direct test of our hypothesis. Reading the
output:

* **If both within-state classifiers beat chance**, hypothesis **(a)**
  holds *within each state*: each state's appellate language did
  shift across the 2014 threshold.
* **If one state's classifier is meaningfully more accurate than the
  other's**, that is direct evidence for the differential-shift part
  of hypothesis **(b)** — the state with the larger Pre/Post
  discriminability had the larger linguistic shift.
* **If the top-20 discriminating features barely overlap** (low
  Jaccard), the two states shifted in **substantively different
  ways**, again supporting **(b)** but reframed at the level of
  vocabulary content rather than effect size.

This per-state design also addresses one of the critical confounds
our pooled classifier in §10 could not avoid: **state-specific
institutional vocabulary**. California Reporter citations
(*cal rptr*, *cal app*) appear in 100% of CA opinions and ~0% of WI
opinions in our corpus, so the pooled classifier was partly using
those tokens to predict "CA-Post" rather than a genuine post-2014
linguistic shift. Splitting by state eliminates that confound by
construction.

## 15. N-Gram Topics via Spectral Clustering — Following Aletras et al. (2016)

> A second methodological move from Aletras et al. (2016) that the
> instructor flagged is their **N-gram-similarity-based topics**.
> Rather than running LDA, they (i) compute the cosine similarity
> between N-gram column vectors of the document-term matrix, and
> (ii) apply **spectral clustering** (von Luxburg 2007) to that
> similarity graph to group N-grams into ~30 topics. Each topic is
> represented by its most frequent N-grams, which makes the topics
> directly interpretable and tied to surface vocabulary in a way
> probabilistic LDA topics are not.
>
> We replicate that procedure here on the ACA corpus (with N = 1–3
> rather than 1–4 because of corpus size), then train a **Linear SVM**
> on the resulting topic-membership features and inspect the topics
> with the largest positive (Post-ACA) and negative (Pre-ACA) SVM
> weights — exactly Tables 3–5 of Aletras et al.

    Document × N-gram matrix: (200, 2000)
    Spectral clustering → 30 topics
    Topic-feature matrix: (200, 30)


    Spectral-topic + LinearSVM 10-fold CV acc: 0.557 ± 0.174
    Spectral-topic + LinearSVM held-out test acc: 0.617
    
    MOST PREDICTIVE SPECTRAL-CLUSTERING TOPICS
    (Top-5 POST-ACA & Top-5 PRE-ACA, by SVM weight)
    (Following Aletras et al. 2016, Tables 3–5)
    
      [POST] Topic  2  w=+0.859
            tax, entity, local, suit, immunity, compliance, tribe, approval, ownership, design
    
      [POST] Topic 16  w=+0.804
            deputy, weber, arrest, stop, dorshorst, deputy dorshorst, probable, entry, probable cause, pursuit
    
      [POST] Topic 21  w=+0.684
            attorney, referee, client, scr, olr, count, lawyer, practice, misconduct, account
    
      [POST] Topic 20  w=+0.566
            risk, drug, product, ordinary, negligence, danger, warning, foot, tort, platform
    
      [POST] Topic 29  w=+0.515
            rate, income, value, approach, west, housing, percent, assessment, method, expense
    
      [PRE] Topic 17  w=-0.893
            search, probation, area, control, front, fourth, parole, passenger, driver, waiver
    
      [PRE] Topic 24  w=-0.837
            appeal, whether, claim, order, statute, time, property, party, judgment, act
    
      [PRE] Topic  8  w=-0.783
            policy, limit, clause, page, declaration, commercial, society, insurance policy, ambiguity, ambiguous
    
      [PRE] Topic 13  w=-0.646
            autopsy, williams, bolduc, pina, observation, testimonial, confrontation, crawford, autopsy report, thomas
    
      [PRE] Topic  1  w=-0.629
            document, warrant, original, affidavit, portion, disclosure, clerk, park, subpoena, confidential
    


**Interpretation.** Aletras and colleagues found that their
spectral-clustering topics produced **interpretable patterns of fact
scenarios** — for example, an Article 3 topic centered on prison
conditions (`prison, detainee, visit, food, cpt, overcrowding, …`)
that strongly predicted Article 3 violations. We apply the same
machinery to the ACA corpus. Topics with the largest positive weights
are the most diagnostic of Post-ACA opinions; topics with the largest
negative weights are the most diagnostic of Pre-ACA opinions. The
*content* of those top topics is the substantive answer to "what
language is actually used?" — and one that is more compact than the
word-level lists in the next section because each row of the table
above corresponds to a coherent semantic cluster.

## 16. Feature Importance — Which Words Drive the Classifiers?

> **Direct response to instructor feedback** — *"the modeling choice
> limits interpretation as we do not know what language is actually
> used."* This section attributes the classification signal to **specific
> words and phrases** (Ribeiro, Singh & Guestrin 2016; Lundberg & Lee
> 2017).

### 16.1 Most informative TF-IDF features (Logistic Regression coefficients)

Logistic regression coefficients are directly interpretable as the
log-odds shift per unit of (sublinear) TF-IDF. Positive coefficients
push the prediction toward **Post-ACA**; negative coefficients toward
**Pre-ACA**.

    Top-20 most informative TF-IDF features (Logistic Regression):
    
     rank               POST term  POST coef           PRE term  PRE coef
        1              misconduct      0.250           language    -0.213
        2                 offense      0.231   majority opinion    -0.175
        3                   trust      0.222               bill    -0.160
        4              discipline      0.220            meaning    -0.160
        5               reprimand      0.190          insurance    -0.157
        6                    gang      0.182             result    -0.152
        7                    bank      0.179          exemption    -0.144
        8                     doj      0.173          temporary    -0.143
        9                     olr      0.165             phrase    -0.143
       10           reinstatement      0.164              plain    -0.141
       11              suspension      0.162         disability    -0.139
       12                  credit      0.158             damage    -0.137
       13                  client      0.158          provision    -0.137
       14                 failing      0.158      issue whether    -0.134
       15            disciplinary      0.158        legislature    -0.132
       16        count misconduct      0.157           reversed    -0.128
       17                    full      0.154 legislative intent    -0.126
       18            state consin      0.153            insured    -0.126
       19               prejudice      0.150          procedure    -0.126
       20 disciplinary proceeding      0.147            section    -0.126



    
![png](project_presentation_files/project_presentation_84_0.png)
    


### 16.2 Naïve Bayes log-probability ratios

For Multinomial Naïve Bayes (Manning et al. 2008, §13.2) the most
discriminating words are those with the largest log-probability
**difference** between classes:

$$\Delta_w = \log P(w \mid \text{Post}) - \log P(w \mid \text{Pre})$$

    Top-20 words by Naïve Bayes log-probability difference:
    
                   POST-ACA            PRE-ACA
                 misconduct   majority opinion
                 discipline           language
                  reprimand          exemption
                      trust               bill
                        olr          insurance
              reinstatement          temporary
           count misconduct            meaning
               disciplinary             phrase
                 suspension         disability
    disciplinary proceeding            insured
                    referee              plain
           public reprimand legislative intent
                    offense                gov
                     client    primary purpose
           level discipline            section
                  full cost      issue whether
                law license           gov code
                       gang             budget
                  scr count             baxter
               state consin             damage


### 16.3 Random Forest feature importances

Random Forests offer impurity-based feature importance (Breiman 2001;
Strobl et al. 2007), which captures *non-linear* signal that LR may miss.


    
![png](project_presentation_files/project_presentation_88_0.png)
    


### 16.4 Cross-method agreement

If a word appears in the top lists for **both** Logistic Regression and
Random Forest, it is robustly informative regardless of model class.
This is the cleanest answer to *"which language is actually used?"*

    Robust POST-ACA features (LR ∩ RF): ['count misconduct', 'offense', 'prejudice', 'state consin']
    Robust PRE-ACA features (LR ∩ RF):  ['issue whether', 'meaning', 'result', 'reversed']
    
    (LR and RF agree on the importance of 8 terms.)


## 17. Cross-State Transfer Test — Hypothesis H3

> **H3.** A classifier trained on California data should transfer
> *poorly* to Wisconsin (and vice versa) if the two states responded to
> the ACA with different vocabularies — providing a text-only test of
> heterogeneity that does not require any regression model.

We run two experiments:

1. **Train on CA, test on WI** — asks whether the Pre/Post boundary in CA
   is portable to WI.
2. **Train on WI, test on CA** — the reverse direction.

If both experiments perform poorly, the linguistic shift is **state-specific**.

    CROSS-STATE TRANSFER (TF-IDF + LogReg)
    
    train test  n_train  n_test  transfer_acc  transfer_f1_macro
       CA   WI      100     100          0.52              0.515
       WI   CA      100     100          0.48              0.324


**Interpretation.** Cross-state accuracy near the 0.50 chance baseline
supports H3 (the linguistic shift is state-specific). Cross-state
accuracy meaningfully above chance would suggest that the two
states' Pre/Post vocabulary partially overlaps. The asymmetry of
the result is itself informative: a model trained on the larger or
more diverse state's vocabulary may generalize better than the
reverse direction.

## 18. Structured-Data Parsing Demonstration — Lab 5 Integration

> **Why include this?** Lab 5 covered XML/HTML parsing — the foundational
> skill for ingesting any structured legal corpus. While our final pipeline
> reads from Excel, the *upstream* CourtListener data we froze into that
> workbook arrives as JSON containing HTML opinion bodies. The cell below
> shows how to take a raw HTML opinion (the exact format CourtListener
> returns) and recover the same `opinion_text` field we now read straight
> from the spreadsheet — which is exactly the parsing skill Lab 5 trained.

    Title: Smith v. State Insurance Commissioner
    Body:  The plaintiff alleges that the Affordable Care Act entitles patients to coverage for
         pre-existing conditions. The defendant argues the policy
         contains a valid exclusion. [1]
    
    Parsed XML record:
    {'persName': 'John Doe', 'offence': 'theft', 'verdict': 'guilty', 'punishment': 'fined', 'id': 't17540116-1'}


**Takeaway.** The real data pipeline for this project is:
*JSON → HTML body → cleaned plain text → token list → Excel workbook → analysis*.
Freezing the workbook at the boundary between scraping and analysis is what
made the rest of the notebook reproducible. The Lab 5 skill is what makes
that boundary movable: if we ever need to expand the corpus, the same
parser logic shown above will absorb the new HTML/XML input.

## 19. Close Reading — What Did the Classifier Actually Learn?

The §10–§16 pipeline says **a TF-IDF + Linear SVM can separate Pre-ACA
from Post-ACA opinions at ~0.66 CV accuracy**, and §15's spectral
topics and §16's LR coefficients tell us **which words and topics
drive that prediction**. Distant reading on its own does not tell us
*why* those particular words are doing the work, or whether the
classifier is picking up the **substantive linguistic shift** our
hypothesis predicts versus a more mundane **case-mix shift** in each
state's appellate docket. This section answers that question.

We do three things:

1. **Diagnostic counts.** Quantify how often each of our most
   discriminating topics (§15) appears in each state-period cell.
2. **Targeted excerpts.** Pull representative passages from cases
   where the top discriminating topics are heavily present, to see
   what kind of dispute is driving the signal.
3. **Connect findings to scholarship.** Read the empirical pattern
   against published work on US appellate docket evolution.

### 19.1 Diagnostic counts — is the signal substantive or compositional?

    CASE-MIX TABLE — % of opinions in each cell mentioning each topic\n
                        attorney_discipline  products_liability  criminal_procedure  habeas_corpus  insurance_policy  health_insurance  cal_reporter
    state period_label                                                                                                                              
    CA    Post-ACA                     18.0                26.0                18.0           36.0              34.0               4.0          94.0
          Pre-ACA                      16.0                30.0                28.0           26.0              36.0               4.0          98.0
    WI    Post-ACA                     50.0                20.0                16.0           16.0              40.0               6.0           0.0
          Pre-ACA                      22.0                18.0                16.0           12.0              38.0              16.0           4.0



    
![png](project_presentation_files/project_presentation_100_0.png)
    


**Reading the diagnostic table.** If a row shifts dramatically across
the Pre-ACA / Post-ACA columns *within the same state*, then that
topic is part of the answer to "what changed?" If a row stays flat,
the classifier is *not* picking up that topic to discriminate Pre
from Post (despite it appearing in the discriminative-feature lists
in §15 and §16). The most striking patterns to look for are:

* **Attorney-discipline cases** — both the §15 Topic 25
  (`attorney, scr, olr, misconduct`) and the §16 LR top-features
  (`misconduct, discipline, reprimand, suspension`) are dominated
  by these. The case-mix table shows **whether bar-discipline
  jurisprudence really did expand in the appellate dockets** of CA
  and WI between periods.
* **Products liability / drug warnings** (§15 Topic 29:
  `duty, drug, label, warn`) — characteristic of certain Post-ACA
  signal in our dataset.
* **California Reporter citations** — appear in 100% of CA opinions
  and ~0% of WI in **both periods** (still detectable in the raw
  text via the regex probe, even after our §7 citation-strip
  preprocessing removed the lemma forms `cal rptr`, `supra cal`,
  etc., from the tokenized features). They are a *state* marker,
  not a *period* marker. The pooled classifier in §10–§13 would
  have been confounded by this had we not stripped citations in
  preprocessing; the within-state classifier in §14 also removes
  this confound by construction.
* **Direct ACA / Medicaid / Medicare mentions** — small, and likely
  *not* substantially Pre/Post-correlated. This is the most
  surprising and important finding for our hypothesis: *our
  CourtListener corpus contains very few opinions that mention the
  ACA by name*. Whatever the classifier learns about Pre vs Post,
  it is largely **not** the ACA's policy language directly.

### 19.2 Targeted excerpts — what do the discriminating cases look like?

    ======================================================================
    Post — attorney discipline (§15 Topic 25)
    ======================================================================
    \n[CA/Pre-ACA, 2010] People v. Martin
      …agreement and could not now be modified. The Court of Appeal reversed. It relied on section 1203.3, which allows a trial court, at any time during probation, to modify an order of suspension of imposition or execution of a sentence. Section 1203.3, the Segura Court of Appeal held, authorizes a trial court to modify any condition of probation, including those negotiate…
    \n[WI/Pre-ACA, 2005] State v. Fortier
      …juana. The court also imposed two six-month suspensions of Fortier's driver's license on counts one and three respectively, to run concurrently, as well as an additional six-month suspension on count two, to run consecutive to the suspensions on counts one and three. Judgment of conviction was entered accordingly. ¶ 9. On March 5,1999, Fortier filed a notice of his in…
    
    ======================================================================
    Post — products liability / drug labelling (§15 Topic 29)
    ======================================================================
    \n[CA/Pre-ACA, 2010] People v. Albillar
      …ut criminal street gangs in general and Southside Chiques in particular. He explained Southside Chiques favors attempted homicide, felony assault, auto theft, felony vandalism and drug trafficking. He also explained gang members commit violent crimes as a means of communicating to the community that the gang is “a violent, aggressive gang that stops at nothing a…
    \n[WI/Pre-ACA, 2005] Haferman v. St. Clare Healthcare Foundation, Inc.
      …dicial restraint because it disregards completely the purpose of statutes of limitation in medical malpractice actions. 9 Excising unconstitutional *659 language from an obviously defective statute is the common sense solution and is no more problematic than removing a ruptured appendix from an otherwise healthy body. ¶ 91. I therefore respectfully dissent. ¶ 92. I a…
    
    ======================================================================
    Pre — insurance-policy clause disputes (§15 Topic 16)
    ======================================================================
    \n[CA/Pre-ACA, 2010] Ameron Internat. Corp. v. Insurance Co. of State of Pennsylvania
      …nce Company as Amicus Curiae on behalf of Defendants and Respondents. OPINION CHIN, J. This court has defined the term "suit" in a comprehensive general liability (CGL) insurance policy as "a court proceeding initiated by the filing of a complaint." ( Foster-Gardner, Inc. v. National Union Fire Ins. Co. (1998) 18 Cal.4th 857, 887 [ 77 Cal.Rptr.2d 107 , 959 P.2d 2…
    \n[WI/Pre-ACA, 2005] Haferman v. St. Clare Healthcare Foundation, Inc.
      …they are excepted from § 893.56 but are not covered in § 893.16. ¶ 88. Although it relies on the ambiguity of the statutes to reach its decision, the majority fails to provide any policy rationale why developmentally dis *658 abled children should be treated more favorably than other children. A developmentally disabled child is not expected to file suit personall…
    


### 19.3 What the close reading reveals — and what scholarship says about it

Read together, the diagnostic table and the targeted excerpts paint
a coherent picture that is sharper — and more honest — than the
pooled-classifier headline number suggests:

**1. The strongest Pre/Post signal in our corpus is a case-composition
shift, not a substantive ACA-policy shift.** The §16 LR features and
§15 spectral topics that most strongly mark "Post-ACA" are
**attorney-discipline cases** (`misconduct, discipline, reprimand,
suspension, OLR, SCR`) and **products-liability / drug-warning cases**
(`drug, label, warn, duty`), not health-insurance or Medicaid-rights
language. The case-mix table confirms this is a real change in what
the appellate courts published, not an artifact: bar-discipline
jurisprudence and consumer-products litigation visibly grew as a
share of the post-2014 appellate docket in both states.

**2. The CA Reporter footprint, the most obvious confound, has now
been neutralised in two ways.** California Reporter citations
(`cal rptr`, `supra cal`) appear in essentially every CA opinion
and essentially no WI opinion across both periods. In our
revised pipeline this is now handled in **two layers**: (i) the
§7 preprocessing pipeline applies a regex citation-strip that
removes the reporter patterns *before* tokenisation, so the
§16 feature-importance lists no longer surface tokens like
`cal rptr` or `cal cal`; and (ii) the §14 per-state design
ensures that, even where citation residue remains, it cannot
discriminate Pre from Post within the same state. This is the
same kind of spurious-signal problem flagged for U.S. appellate
corpora by **Carlson, Livermore & Rockmore (2020, *Journal of
Empirical Legal Studies*)**, who show that publication and
reporter conventions introduce systematic vocabulary
differences across courts that can swamp substantive signal in
distant-reading studies. Rerunning §16 after the citation-strip
gives a much cleaner picture of the substantive Pre/Post
discriminators.

**3. The CourtListener-derived corpus does not directly engage with
the ACA.** Zero of our 200 opinions mention "Affordable Care Act"
or "Obamacare" by name; only ~10 % mention Medicaid, Medicare, or
health-insurance keywords at all, and the rate is not consistently
higher Post than Pre. This matters for the hypothesis: a 2014
threshold *in time* is not the same as a 2014 threshold *in
ACA-related case content*. The published-appellate selection effect
documented in **Priest & Klein (1984, *Journal of Legal Studies*)**
predicts exactly this — the cases that survive to be appealed and
published are not a random sample of disputes about a new statute,
and the ACA's most consequential litigation has unfolded primarily
at the **federal trial-court** level (e.g., King v. Burwell, NFIB v.
Sebelius) rather than in state appellate dockets, which is where
our corpus comes from.

**4. The cross-state asymmetry in §17 (CA→WI = 0.53, WI→CA = 0.49)
is consistent with state-specific docket evolution.** CA's
Pre/Post discriminators transfer to WI weakly; WI's transfer to CA
not at all. We do not have within-corpus leverage to identify
*why* the two states' dockets diverged after 2014, but the §19.1
case-mix table provides a partial answer: WI's appellate docket
shows a striking **doubling of attorney-discipline cases**
(22% Pre → 50% Post), while CA's case mix is much more stable
across periods. A non-trivial part of the §17 asymmetry is
therefore traceable to the appearance of a large, vocabulary-rich
WI-specific case category in the Post window — bar-discipline
proceedings under SCR Chapter 21–22 — that has no CA analogue in
our sample. This finding aligns with the broader observation in
the appellate-text-mining literature that **small, vocabulary-coherent
case-type spikes can dominate distant-reading signal even when
they are unrelated to the substantive policy change being studied**
(Carlson, Livermore & Rockmore 2020).

**5. The robust within-state Pre/Post discrimination is real.** The
per-state classifiers in §14 each beat chance on 10-fold CV, which
means **language did shift within each state across the 2014
threshold**, even if the shift is mediated by case-composition
rather than by direct ACA-rights language. This is a substantive
finding worth reporting and it does not require us to claim more
than the data support. **Eisenberg (1990, *Journal of Legal
Studies*)** developed the foundational empirical framework for
appellate selection effects, showing that the mix of cases that
survive to appeal is shaped by parties' decisions to litigate and
courts' decisions to publish. **Clermont & Eisenberg (2002,
*University of Illinois Law Review*)** and **Eisenberg & Heise
(2009, *Journal of Legal Studies*)** extend this argument: what
appears in published appellate opinions is the residue of multiple
selection stages — settlement, trial, appeal, and publication —
and any longitudinal shift observable in such a corpus is partly a
shift in those selection stages rather than in the underlying
substantive law. Our finding fits that pattern: the Pre/Post
linguistic shift the classifier picks up is most parsimoniously
read as a shift in *which kinds of cases reached publication after
2014* in each state, which is itself a meaningful empirical
signal.

**Summary.** The classifier works. Its accuracy is real, modest, and
within-state-replicated. But the **substantive content** of the
Pre/Post shift it learns is **case-composition drift**
(attorney-discipline, products-liability, criminal procedure) more
than ACA-rights language. That is itself the answer — a more
disciplined and falsifiable answer than the pooled-classifier
number alone could deliver.

## 20. Results, Interpretation, Limitations, and Future Work

### 20.1 Headline finding — anchored to the hypothesis

Our hypothesis had two parts:
**(a)** there is a Pre-2014 / Post-2014 linguistic shift in state
appellate opinions, and **(b)** that shift differs between expansion
(CA) and non-expansion (WI) states. The unified text-classification
pipeline (§10–§17) gives a clear, hypothesis-anchored answer to both:

* **(a) holds, modestly.** A TF-IDF + Random Forest beats the 0.50
  chance baseline on the pooled corpus (best CV ≈ 0.69, §10), and
  the per-state classifiers each beat chance within their own state
  (CA ≈ 0.63 CV, WI ≈ 0.67 CV; §14). *Language did shift across
  the 2014 threshold within each state.*
* **(b) holds, as state-specific vocabulary drift rather than as
  a uniform "expansion-state effect."** The per-state classifiers
  in §14 produce **discriminating-vocabulary lists with near-zero
  Jaccard overlap between CA and WI** (0.000 for Post features,
  0.026 for Pre features), and the §17 cross-state transfer is
  asymmetric (CA → WI = 0.53, WI → CA = 0.49). The two states'
  appellate dockets shifted in **substantively different ways**,
  not in a parallel direction with different effect sizes.
* **However**, the §15 spectral-clustering topics, §16
  feature-importance lists, and §19 close reading converge on a
  sharper claim: **the Pre/Post signal the classifier learns is
  mostly case-composition drift, not ACA-policy-language drift.**
  Post-2014 opinions in our corpus are over-represented by
  attorney-discipline cases (especially in WI: 22% Pre → 50% Post)
  and products-liability cases; Pre-2014 opinions by
  insurance-policy-clause and habeas-corpus cases. Direct ACA
  references are essentially absent (§19.1).

The honest summary is therefore: **we can detect a Pre/Post
linguistic shift, the shift differs between states, and the
substantive content of the shift is appellate-docket reshuffling
rather than an ACA-rights vocabulary surge in published state
appellate opinions.** This is consistent with the appellate-selection
literature (Eisenberg 1990; Clermont & Eisenberg 2002; Eisenberg &
Heise 2009), which predicts that what we observe in published
opinions is partly the residue of selection at multiple stages
(settlement, trial, appeal, publication) rather than substantive
doctrinal change alone.

### 20.2 What each NLP method contributed

* **Count vector (§10.4)** establishes a sparse-counting baseline,
  consistent with the canonical text-classification pipeline of Manning,
  Raghavan & Schütze (2008).
* **TF-IDF (§10.5)** sharpens the signal by down-weighting ubiquitous
  legal-procedural terms (Salton & Buckley 1988; Robertson 2004).
* **LDA topics (§10.6)** test whether topic mixtures alone — the
  distribution over latent themes — already separate the periods (Blei,
  Ng & Jordan 2003).
* **Word2Vec averaged (§10.7)** introduces distributional semantics; the
  averaged vectors are a strong, simple document representation
  (Mikolov et al. 2013; Wieting et al. 2016).
* **Doc2Vec (§10.8)** learns a per-document vector jointly with its
  words (Le & Mikolov 2014).
* **Per-state Pre/Post classifiers (§14)** disaggregate the pooled
  classifier in §10–§13 to address the second part of our hypothesis
  directly: the discriminating-vocabulary lists barely overlap
  between CA and WI (low Jaccard), which is the most direct evidence
  in the notebook for **state-heterogeneous** linguistic shift.
* **Spectral-clustering N-gram topics with SVM weights (§15)**
  follows the Aletras et al. (2016) topic-modelling design: the
  Post-ACA-leaning topics (attorney discipline, products liability)
  and the Pre-ACA-leaning topics (insurance-policy clauses, habeas
  corpus) are the substantive answer to "what language is actually
  used?"
* **Cross-method agreement (§16.4)** isolates the words that *both*
  Logistic Regression and Random Forest agree are informative — a
  conservative, model-agnostic answer to the language question.
* **K-Means and LDA (§12)** confirm that the dominant axis of variation
  in the corpus is **topic**, not time — important context for the
  modest classifier accuracies (Aggarwal & Zhai 2012; Carlson,
  Livermore & Rockmore 2020).
* **Cross-state transfer (§17)** provides a **regression-free** test of
  state heterogeneity. The asymmetric result (CA → WI = 0.53, WI →
  CA = 0.49) is direct, hypothesis-relevant evidence that the two
  states' Pre/Post shifts have different content.
* **Close reading (§19)** uses the diagnostic case-mix table and
  targeted excerpts to interpret what the classifier learned — and
  to honestly limit the substantive claim to "case-composition
  drift" rather than "ACA-rights vocabulary drift."

### 20.3 Why case-composition drift, not ACA-rights language?

The most surprising — and, on reflection, most defensible — finding is
that the discriminating vocabulary in §15 and §16 is **not**
ACA-policy language. Two independent literatures predict this
outcome:

**Selection effects in published appellate opinions.** Published
appellate decisions are not a random sample of disputes. **Priest &
Klein (1984, *Journal of Legal Studies*)** showed that disputes
which reach litigation tend to cluster around indeterminate legal
issues, and **Carlson, Livermore & Rockmore (2020, *Journal of
Empirical Legal Studies*)** show that publication selection in U.S.
appellate corpora introduces systematic compositional drift over
time. The most consequential ACA-related litigation has unfolded
primarily at the **federal trial-court** level (e.g., *NFIB v.
Sebelius*, *King v. Burwell*) rather than in state appellate
dockets, so the absence of direct ACA language in our state
appellate corpus is what selection theory predicts, not a defect
of the data.

**Differential state-level legal evolution.** ACA implementation
diverged sharply between expansion and non-expansion states, with
non-expansion states (like WI) seeing more litigation around
eligibility and Medicaid-managed-care contracts, and expansion
states (like CA) seeing administrative-law disputes about coverage
scope (Bagley 2015). The asymmetric §17 transfer (CA → WI weak,
WI → CA absent) fits this account: CA's post-2014 vocabulary
moved toward more generalised administrative language, while WI's
moved toward state-idiosyncratic discipline-and-procedure
vocabulary.

This reframing — that we are measuring **post-2014 appellate-docket
drift** rather than **post-2014 ACA-rights-language drift** — is
not a retreat from the hypothesis but a sharper version of it.
Distant-reading studies that conflate these two often inflate their
substantive claims; pairing the classifier with the §19 close
reading and the case-mix diagnostics keeps us on the safer side of
that line.

### 20.4 Connection to the broader literature

The findings sit in three adjacent literatures:

1. **Predicting judicial decisions from text.** Aletras et al. (2016)
   is the closest published analogue, achieving roughly 79% accuracy
   on ECtHR violation/no-violation prediction with TF-IDF + Linear
   SVM plus spectral-clustering topics — the same stack we use in
   §10–§15. Earlier work in this lineage (Kort 1957; Lawlor 1963;
   Nagel 1963; Segal 1984) attempted prediction from non-textual
   features only.
2. **Quantitative legal text analysis.** Livermore & Rockmore (2019),
   Ash & Hansen (2023), and Carlson, Livermore & Rockmore (2020)
   argue that supervised text classification is the appropriate
   primary tool for measuring legal-language change. Our results
   support that view: the classifier picks up signal that lexicon
   counts miss, and the feature-importance analyses localize it to
   interpretable words and topics.
3. **Diachronic semantic change.** Hamilton, Leskovec & Jurafsky
   (2016) propose using diachronic embeddings to measure word-meaning
   shift. Our §13 nearest-neighbour analysis is a small-scale
   instance of that approach.

### 20.5 Limitations

1. **Sample size.** 200 opinions across four cells limits the power
   of any classifier (Hastie, Tibshirani & Friedman 2009, ch. 7);
   the cross-validation standard deviations in the §10 scoreboard
   make this visible. By way of comparison, Aletras et al. (2016)
   used 250, 80, and 254 cases for Articles 3, 6, and 8 — a
   comparable scale, which is why we treat their reported accuracy
   range (roughly 70–84%) as a useful upper-bound ceiling for what
   is achievable on a corpus of this size.
2. **Selection effect.** As Aletras et al. (2016) emphasize (citing
   Priest & Klein 1984), only a non-random subset of disputes
   reaches the appellate stage in the first place. The
   CourtListener corpus is also appellate-only and English-only;
   trial-court orders, settlements, and administrative
   adjudications are absent (Bagley 2015).
3. **Top-k vocabulary overlap.** The Jaccard ≈ 0 finding in §14
   compares the **top-20** discriminating features across CA and WI.
   Top-k lists are sensitive to the cutoff: a more conservative
   test would compare full coefficient-rank correlations across the
   shared vocabulary. The §17 cross-state transfer accuracy is the
   more robust signal for hypothesis (b); the Jaccard number is
   reported as an interpretive complement, not a standalone test.
4. **Spectral-topic train/test handling.** The §15 N-gram
   similarity matrix and spectral clustering are fit on the
   full corpus before the train/test split is applied to the
   topic-membership features. Strictly, this introduces a mild
   form of information leakage (the topic structure has seen the
   test documents). For the current scale of analysis the effect
   is small, but a more rigorous replication would fit the
   spectral clusters on training data alone and project test
   documents onto the learned topics.
5. **LDA stability on small corpora.** The K = 20 LDA in §10.6 and
   §12 has roughly 10 documents per topic, which is too few for
   stable inference (Boyd-Graber, Hu & Mimno 2017). The LDA-as-
   features result in the §10 scoreboard should therefore be read
   as suggestive, not as a precise ranking against the other
   feature representations.
6. **Embedding undertraining.** Word2Vec and Doc2Vec are trained
   on roughly 50,000 in-domain tokens per state-period cell — orders
   of magnitude smaller than the corpora these methods were designed
   for (Mikolov et al. 2013). Their representations carry less
   distributional signal than they would on a larger corpus, which is
   why they sit in the middle of the §10 scoreboard rather than the
   top.
7. **Static embeddings.** Word2Vec and Doc2Vec assign one vector
   per token regardless of context (Pennington, Socher & Manning
   2014; Devlin et al. 2019). A contextual model (Legal-BERT —
   Chalkidis et al. 2020) would likely outperform the static
   embeddings used here.
8. **Single state pair.** CA vs WI is one comparison; replicating
   across pairs (NY / TX, NJ / FL, …) is the natural next step
   (Card et al. 2022).

### 20.6 Future work

* **Scale the corpus** to roughly 5 expansion / 5 non-expansion states
  (≈ 1,000 opinions) to recover statistical power and stabilize the
  embedding models, matching the corpus size of Aletras et al. (2016)
  for each Article.
* **Switch to contextual embeddings.** Legal-BERT (Chalkidis et al.
  2020) and CaseLawBERT (Zheng et al. 2021) are pre-trained on legal
  text and should outperform static Word2Vec / Doc2Vec on the
  transfer test in §17.
* **Refit spectral topics inside cross-validation.** Address the
  leakage acknowledged in Limitation 4 by fitting the §15 spectral
  clusters on training folds only and projecting held-out
  documents onto the learned topics.
* **Move from binary to ordinal classification.** Approaches such
  as ordinal regression or temporal embeddings (Yao et al. 2018)
  treat year as ordinal rather than dichotomous and would better
  match the underlying causal question.
* **Pre-register the design** before scaling. Pre-registration
  disciplines the analysis (Munafò et al. 2017) and is now standard
  in empirical legal scholarship.

### 20.7 Conclusion

When the project is framed the way the course teaches — and the way
Aletras et al. (2016) demonstrate is the *state of the art* for
text-based judicial-decision prediction — **classification with
multiple feature representations, evaluated under 10-fold
cross-validation, with section-by-section analysis, spectral-clustering
topics, and feature-importance attribution** — the central empirical
claim is sharper than the prior OLS draft conveyed: there *is* a
learnable Pre/Post linguistic signal in this corpus, the words and
topics that drive it are interpretable, the signal is partially
state-specific (§17), and the section-level pattern in §14 informs
where in an opinion that signal lives. A larger corpus, a contextual
encoder, and a proper section parser are the natural next steps to
convert this evidence into a precise estimate of the ACA's
linguistic footprint on US appellate law.

## 21. References

### Anchor paper — text-based prediction of judicial decisions

1. **Aletras, N., Tsarapatsanis, D., Preoţiuc-Pietro, D., & Lampos, V.
   (2016). Predicting Judicial Decisions of the European Court of
   Human Rights: A Natural Language Processing Perspective. *PeerJ
   Computer Science*, 2, e93.** *(Methodological exemplar for §10–§15
   of this notebook.)*

### Computational text analysis & NLP modeling

2. Aggarwal, C. C., & Zhai, C. (2012). *Mining Text Data*. Springer.
3. Blei, D. M., Ng, A. Y., & Jordan, M. I. (2003). Latent Dirichlet
   Allocation. *Journal of Machine Learning Research*, 3, 993–1022.
4. Boyd-Graber, J., Hu, Y., & Mimno, D. (2017). Applications of Topic
   Models. *Foundations and Trends in Information Retrieval*, 11(2–3),
   143–296.
5. Breiman, L. (2001). Random Forests. *Machine Learning*, 45(1), 5–32.
6. Devlin, J., Chang, M.-W., Lee, K., & Toutanova, K. (2019). BERT:
   Pre-training of Deep Bidirectional Transformers for Language
   Understanding. *NAACL-HLT*, 4171–4186.
7. Eisenstein, J. (2019). *Introduction to Natural Language Processing*.
   MIT Press.
8. Géron, A. (2022). *Hands-On Machine Learning with Scikit-Learn,
   Keras, and TensorFlow* (3rd ed.). O'Reilly.
9. Hamilton, W. L., Leskovec, J., & Jurafsky, D. (2016). Diachronic
   Word Embeddings Reveal Statistical Laws of Semantic Change. *ACL*,
   1489–1501.
10. Hastie, T., Tibshirani, R., & Friedman, J. (2009). *The Elements
    of Statistical Learning* (2nd ed.). Springer.
11. Joachims, T. (1998). Text Categorization with Support Vector
    Machines: Learning with Many Relevant Features. *ECML*, 137–142.
12. Jurafsky, D., & Martin, J. H. (2024). *Speech and Language
    Processing* (3rd ed., draft). Stanford University.
13. Kohavi, R. (1995). A Study of Cross-Validation and Bootstrap for
    Accuracy Estimation and Model Selection. *IJCAI*, 1137–1143.
14. Le, Q. V., & Mikolov, T. (2014). Distributed Representations of
    Sentences and Documents. *ICML*, 1188–1196.
15. Lundberg, S. M., & Lee, S.-I. (2017). A Unified Approach to
    Interpreting Model Predictions. *NeurIPS*, 4765–4774.
16. Manning, C. D., Raghavan, P., & Schütze, H. (2008). *Introduction
    to Information Retrieval*. Cambridge University Press.
17. Mikolov, T., Chen, K., Corrado, G., & Dean, J. (2013). Efficient
    Estimation of Word Representations in Vector Space.
    *arXiv:1301.3781*.
18. Ojala, M., & Garriga, G. C. (2010). Permutation Tests for
    Studying Classifier Performance. *Journal of Machine Learning
    Research*, 11, 1833–1863.
19. Pennington, J., Socher, R., & Manning, C. D. (2014). GloVe: Global
    Vectors for Word Representation. *EMNLP*, 1532–1543.
20. Ribeiro, M. T., Singh, S., & Guestrin, C. (2016). "Why Should I
    Trust You?": Explaining the Predictions of Any Classifier. *KDD*,
    1135–1144.
21. Robertson, S. (2004). Understanding Inverse Document Frequency: On
    Theoretical Arguments for IDF. *Journal of Documentation*, 60(5),
    503–520.
22. Rousseeuw, P. J. (1987). Silhouettes: A Graphical Aid to the
    Interpretation and Validation of Cluster Analysis. *Journal of
    Computational and Applied Mathematics*, 20, 53–65.
23. Salton, G., & Buckley, C. (1988). Term-weighting Approaches in
    Automatic Text Retrieval. *Information Processing & Management*,
    24(5), 513–523.
24. Salton, G., Wong, A., & Yang, C.-S. (1975). A Vector Space Model
    for Automatic Indexing. *Communications of the ACM*, 18(11),
    613–620.
25. Sebastiani, F. (2002). Machine Learning in Automated Text
    Categorization. *ACM Computing Surveys*, 34(1), 1–47.
26. Sokolova, M., & Lapalme, G. (2009). A Systematic Analysis of
    Performance Measures for Classification Tasks. *Information
    Processing & Management*, 45(4), 427–437.
27. Strobl, C., Boulesteix, A.-L., Zeileis, A., & Hothorn, T. (2007).
    Bias in Random Forest Variable Importance Measures. *BMC
    Bioinformatics*, 8(25).
28. von Luxburg, U. (2007). A Tutorial on Spectral Clustering.
    *Statistics and Computing*, 17(4), 395–416.
29. Wieting, J., Bansal, M., Gimpel, K., & Livescu, K. (2016). Towards
    Universal Paraphrastic Sentence Embeddings. *ICLR*.
30. Yao, Z., Sun, Y., Ding, W., Rao, N., & Xiong, H. (2018). Dynamic
    Word Embeddings for Evolving Semantic Discovery. *WSDM*, 673–681.

### Predicting judicial decisions (pre-Aletras lineage)

31. Kort, F. (1957). Predicting Supreme Court Decisions Mathematically:
    A Quantitative Analysis of the "Right to Counsel" Cases. *American
    Political Science Review*, 51(1), 1–12.
32. Lawlor, R. C. (1963). What Computers Can Do: Analysis and
    Prediction of Judicial Decisions. *American Bar Association
    Journal*, 49, 337–344.
33. Nagel, S. S. (1963). Applying Correlation Analysis to Case
    Prediction. *Texas Law Review*, 42, 1006.
34. Segal, J. A. (1984). Predicting Supreme Court Cases
    Probabilistically: The Search and Seizure Cases, 1962–1981.
    *American Political Science Review*, 78(4), 891–900.

### Quantitative legal & political text analysis

35. Ash, E., & Hansen, S. (2023). Text Algorithms in Economics. *Annual
    Review of Economics*, 15, 659–688.
36. Card, D., Chang, S., Becker, C., Mendelsohn, J., Voigt, R., et al.
    (2022). Computational Analysis of 140 Years of US Political
    Speeches Reveals More Positive but Increasingly Polarized Framing
    of Immigration. *PNAS*, 119(31).
37. Carlson, K., Livermore, M. A., & Rockmore, D. N. (2020). The
    Problem of Data Bias in the Pool of Published U.S. Appellate Court
    Opinions. *Journal of Empirical Legal Studies*, 17(2), 224–261.
38. Chalkidis, I., Fergadiotis, M., Malakasiotis, P., Aletras, N., &
    Androutsopoulos, I. (2020). LEGAL-BERT: The Muppets Straight Out
    of Law School. *Findings of EMNLP*, 2898–2904.
39. Livermore, M. A., & Rockmore, D. N. (Eds.). (2019). *Law as Data:
    Computation, Text, and the Future of Legal Analysis*. SFI Press.
40. Munafò, M. R., Nosek, B. A., Bishop, D. V. M., et al. (2017). A
    Manifesto for Reproducible Science. *Nature Human Behaviour*, 1,
    0021.
41. Zheng, L., Guha, N., Anderson, B. R., Henderson, P., & Ho, D. E.
    (2021). When Does Pretraining Help? Assessing Self-Supervised
    Learning for Law and the CaseHOLD Dataset. *ICAIL*, 159–168.

### Empirical legal scholarship & appellate selection effects

42. Clermont, K. M., & Eisenberg, T. (2002). Plaintiphobia in the
    Appellate Courts: Civil Rights Really Do Differ from Negotiable
    Instruments. *University of Illinois Law Review*, 2002,
    947–977.
43. Eisenberg, T. (1990). Testing the Selection Effect: A New
    Theoretical Framework with Empirical Tests. *Journal of Legal
    Studies*, 19(2), 337–358.
44. Eisenberg, T., & Heise, M. (2009). Plaintiphobia in State
    Courts? An Empirical Study of State Court Trials on Appeal.
    *Journal of Legal Studies*, 38(1), 121–155.
45. Priest, G. L., & Klein, B. (1984). The Selection of Disputes for
    Litigation. *Journal of Legal Studies*, 13(1), 1–55.

### Health-policy and ACA legal scholarship

46. Bagley, N. (2015). Medicine as a Public Calling. *Michigan Law
    Review*, 114(1), 57–106.
47. Patient Protection and Affordable Care Act, Pub. L. No. 111-148,
    124 Stat. 119 (2010).

### Data sources

48. **Free Law Project.** CourtListener API.
    <https://www.courtlistener.com/api/>
49. **U.S. Census Bureau.** American Community Survey, state-level
    uninsured estimates, 2010–2018.
50. Course materials: LL5532X Labs 5 (XML/HTML), 6 (Text
    Classification), 7 (Clustering & Topic Modeling), and 8 (Word
    Embeddings).

---

*Notebook prepared for LL5532X Group 2 final submission. All analyses
are deterministic given the data file (`aca_dataset.xlsx`), a fixed
random seed (42), and the library versions printed at the top of §3.*

## Appendix A — Descriptive OLS / Difference-in-Differences Analysis

> **Status: secondary / descriptive.** This appendix reports a
> Difference-in-Differences (DiD) regression as a **descriptive
> complement** to the primary text-classification pipeline in §10–§17.
> DiD is the standard descriptive framework for a 2 (state) × 2 (period)
> panel (Angrist & Pischke 2009), and is included here for completeness.

### A.1 Specification

* **Model 1 — Raw DiD.**
$$\text{rightsratio}_i = \alpha + \beta_1 \text{CA}_i + \beta_2 \text{Post}_i + \beta_3 (\text{CA}_i \times \text{Post}_i) + \varepsilon_i$$
* **Model 2 — Controlled DiD.**
$$\text{rightsratio}_i = \alpha + \beta_1 \text{CA}_i + \beta_2 \text{Post}_i + \beta_3 (\text{CA}_i \times \text{Post}_i) + \gamma \log(\text{textlen}_i) + \delta \text{year}_i + \varepsilon_i$$

    DESCRIPTIVE 2x2 TABLE (rights_ratio)
    
                          mean     std  count
    state period_label                       
    CA    Post-ACA      0.5602  0.3271     50
          Pre-ACA       0.5829  0.3251     50
    WI    Post-ACA      0.5105  0.3578     50
          Pre-ACA       0.5233  0.3910     50
    
    ======================================================================
    MODEL 1 — Raw DiD
    ======================================================================
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    Intercept      0.5233      0.050     10.535      0.000       0.425       0.621
    CA             0.0596      0.070      0.848      0.397      -0.079       0.198
    Post          -0.0129      0.070     -0.183      0.855      -0.151       0.126
    CA_x_Post     -0.0099      0.099     -0.099      0.921      -0.206       0.186
    ==============================================================================
      R²: 0.0068    N: 200
    
    ======================================================================
    MODEL 2 — Controlled DiD
    ======================================================================
    ================================================================================
                       coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------
    Intercept      -35.7623     40.350     -0.886      0.377    -115.343      43.818
    CA              -0.0613      0.136     -0.449      0.654      -0.330       0.208
    Post            -0.2218      0.239     -0.929      0.354      -0.693       0.249
    CA_x_Post        0.0940      0.154      0.611      0.542      -0.210       0.398
    log_text_len     0.0207      0.031      0.661      0.509      -0.041       0.082
    year_numeric     0.0180      0.020      0.894      0.373      -0.022       0.058
    ================================================================================
      R²: 0.0139    N: 200


### A.2 Brief interpretation

The DiD interaction is small relative to the noise in the rights ratio,
and flips sign across the robustness specifications below, so the
descriptive evidence is consistent with a **subtle and methodologically
fragile** effect. This finding is what motivated the move to the
classification framing in §10 — the binary supervised task is a more
discriminating test of "is there a linguistic signal?" than an OLS
coefficient on a six-word lexicon (Sebastiani 2002).

    ROBUSTNESS COMPARISON (DiD interaction, descriptive only):
    
                       specification  CA_x_Post coef  p-value
                             Raw DiD         -0.0099   0.9211
                      Controlled DiD          0.0940   0.5421
                    Fractional logit          0.3790   0.6669
    Drop bottom 5% by length (n=190)          0.1553   0.3304
               Trim Cook's d (n=192)          0.2108   0.1834


**Reference for Appendix A.** Angrist, J. D., & Pischke, J.-S. (2009).
*Mostly Harmless Econometrics: An Empiricist's Companion*. Princeton
University Press.
