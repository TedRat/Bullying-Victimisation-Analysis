# Beyond the Classroom — Bullying Victimisation Among School Students

A research poster by Rathnayake T.M. on bullying victimisation among school students, analysing survey data from the 2018 Argentina Global School-Based Student Health Survey. The poster is [`index.html`](./index.html), styled by [`poster.css`](./poster.css), with the charts in [`figures/`](./figures), the artwork in [`Background images/`](<./Background images>) and locally bundled KaTeX for the typeset formulas.

**Research question:** *Which behavioural and social experiences accompany a higher likelihood of being bullied among school students, and can victimisation be predicted from them?*

## Data

| Item | Detail |
|---|---|
| Survey | WHO Global School-Based Student Health Survey (GSHS), Argentina, 2018 |
| Conducted by | World Health Organization and US Centers for Disease Control and Prevention |
| Distribution | Kaggle, *Bullying in schools* (L. Martinelli), CC0 — [kaggle.com/datasets/leomartinelli/bullying-in-schools](https://www.kaggle.com/datasets/leomartinelli/bullying-in-schools) |
| Size | 32,938 pupils, 18 columns (17 variables), 0 missing cells, 0 duplicate rows |
| Nature | A subset of the 56,981 pupils surveyed; rows containing empty cells were removed |
| Scales | Every variable is categorical, ordinal or a count band — none is continuous |
| Target | `any_bullying` — "Yes" in at least one of three settings: **13,385 pupils (40.64%)**, class balance 1.46 : 1 |

## Analysis

### Background and outcome

Bullying is defined as repeated, deliberate aggression towards a pupil who cannot easily defend themselves. The study documents the survey source and its 17 variables, and justifies `any_bullying` as the outcome: a pupil counts as victimised if they report being bullied on school property, away from school, or online.

### Descriptive analysis

- **Frequency tables** for each setting: on school property 20.86%, away from school 22.20%, cyber-bullied 22.41%, any setting 40.64%, bullied in two or three settings 18.10%. Cyber-only (8.35%) was more common than school-property-only (7.28%).
- **Centre, spread and shape** for every score (mean, median, mode, SD, variance, range, IQR, quartiles, CV), plus percentiles and an outlier rule.
- **Who is bullied:** girls 45.55% against boys 34.94% (a 10.61 percentage-point gap); rates varied little by age or body-weight flag.
- **The victimisation gradient:** mean loneliness rose from 2.079 (no bullying) to 3.024 (all three settings); peer kindness fell from 3.468 to 2.991; parental understanding fell from 3.209 to 2.737, rising slightly to 2.775 in the three-setting group.
- **Association:** Spearman coefficients stayed weak throughout (largest ρ = 0.307; loneliness against parental understanding −0.291).
- **Why rank methods:** severe skewness and excess kurtosis in the behavioural counts (attacked +4.44 skew, +22.64 excess kurtosis) made mean-based tests inappropriate.

### Statistical inference

Ten tests at α = 0.05, each reported beside the Bonferroni threshold of 0.005. Because n = 32,938 makes standard errors tiny, **effect sizes are read before p-values**.

| Test | Statistic | p | Effect size | Decision |
|---|---|---|---|---|
| Victimisation × sex | χ² = 382.04 (df 1) | < 1e-16 | Cramér's V = 0.108 (small) | reject H₀ |
| Victimisation × body-weight flags | χ² = 0.05 / 0.02 / 0.40 | .824 / .893 / .528 | V ≤ 0.003 (negligible) | fail to reject |
| Loneliness by bullying status | Mann-Whitney U = 1.717 × 10⁸ | < 1e-16 | Cohen's d = 0.585 (medium) | reject H₀ |
| Loneliness by severity | Kruskal-Wallis H = 2823.7 (df 3) | < 1e-16 | η² = 0.0856 | reject H₀ |
| Score pairs | Spearman ρ = −0.291 to +0.307 | < 1e-16 | weak throughout | reject H₀ |

Confidence intervals: prevalence **40.11%–41.17%** (±0.53 pp) and mean loneliness **2.333–2.358** (mean 2.346, SD 1.167). The test follows the measurement scale — nominal × nominal uses chi-square, one ordinal score across two groups uses Mann-Whitney, across four ordered groups uses Kruskal-Wallis, and two ordinal scores use Spearman.

### Predictive modelling

A **multiple logistic regression** for `any_bullying`, benchmarked against a pruned decision tree. Least squares was rejected because a binary response lets it predict probabilities outside 0–1; logistic regression models the log-odds and is fitted by maximum likelihood.

| Metric | Baseline (majority class) | Model |
|---|---|---|
| Accuracy | 0.5936 | **0.6554** |
| Sensitivity | 0.0000 | **0.3994** |
| Specificity | 1.0000 | 0.8307 |
| AUC-ROC | 0.5000 | **0.6788** |
| Brier score | — | 0.2187 |
| McFadden / Nagelkerke R² | — | 0.069 / 0.121 |

Odds ratios (95% CI): loneliness 1.594 (1.554–1.636), peer support 0.863 (0.842–0.884), fighting 1.150 (1.122–1.178), truancy 1.104 (1.071–1.139), parental support 0.982 (0.963–1.002), close friends 1.006 (0.974–1.040). The last two intervals contain 1, so their apparent protective effect in isolation disappears once loneliness is held constant (OR 0.854 and 0.858 fitted alone) — confounding, since the three scores are themselves correlated at ρ = −0.291. Sex entered only as a control and moved no coefficient by more than 0.050.

Feature selection followed four written rules: exclude the outcome's own components and the collapsed twins, exclude `Physically_attacked` as a sibling measure, screen the remainder, and drop age and body-weight flags (V ≤ 0.013). The cut-off is treated as a policy choice: lowering it from 0.50 to Youden's optimum of 0.40 lifts victim detection from 39.9% to 63.8% for a small loss of accuracy. Calibration is close (maximum gap 0.046), the top 30% of predicted risks contains 44.6% of all victims and the top 20% contains 31.8%.

### Conclusions and recommendations

Bullying affects 40.64% of pupils across multiple settings; loneliness is the strongest consistent indicator of victimisation; body weight shows no meaningful association; overall effects are modest. Recommendations are to monitor loneliness, use the model as a support guide rather than a labelling tool, address cyberbullying and promote positive peer relationships, and avoid body-weight stigma in anti-bullying campaigns. The data are self-reported and cross-sectional, so the findings describe associations, not causes, and nothing here establishes direction or mechanism.

## References and credits

1. U.S. Department of Health and Human Services, *StopBullying.gov* — definition of bullying. <https://www.stopbullying.gov>
2. L. Martinelli, *Bullying in schools* (dataset), Kaggle, 2018, CC0. <https://www.kaggle.com/datasets/leomartinelli/bullying-in-schools>
3. World Health Organization and US Centers for Disease Control and Prevention, *Global School-Based Student Health Survey (GSHS)*, Argentina, 2018.
4. R Core Team, *R: A Language and Environment for Statistical Computing*, v4.6.1. <https://www.R-project.org>
5. KaTeX (MIT licence), bundled in `katex/` for typesetting the formulas. <https://katex.org>

## Licensing

The original material in this repository — the poster markup and the author's own written text (`index.html`, excluding the third-party material noted in [NOTICE.md](./NOTICE.md)), the stylesheet (`poster.css`), the charts in `figures/`, the poster artwork in `Background images/`, and this README — is licensed under **Creative Commons Attribution 4.0 International (CC BY 4.0)**, to the extent the author holds rights in it. Full legal code: [LICENSE](./LICENSE). Suggested attribution: *"Beyond the Classroom — Bullying Victimisation Among School Students" by Rathnayake T.M., licensed under CC BY 4.0.*

Facts, statistical results, methods and ideas are not copyrightable subject matter and are neither licensed nor restricted here.

That grant does **not** cover everything in the folder. `katex/` is third-party MIT software (notice retained in `katex/LICENSE`); the survey data is CC0 under its own terms; and the WHO/CDC survey and the cited StopBullying.gov definition remain their owners' material. See [NOTICE.md](./NOTICE.md) for the exact scope.

SPDX identifier: `CC-BY-4.0`.
