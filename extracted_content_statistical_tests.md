# Chapter 4: Statistical Tests - Extracted Content Analysis

## Document Source
- **PDF**: Aula5_Testes Estatísticos.pdf
- **R Scripts**: aula_3_testes_estatisticos.R, Aula5_testes.R
- **Extraction Date**: 2026-01-28

---

## 1. THEORY AND CONCEPTS FOR HYPOTHESIS TESTING

### 1.1 Fundamental Concepts

#### Hypothesis Testing Framework
Based on the R script documentation (Aula5_testes.R, lines 46-47, 122-123):

**Null Hypothesis (H₀)**: The default assumption that there is no effect or no difference
- Example: "The means of age between sexes are equal"
- Example: "The variables are independent (no association)"

**Alternative Hypothesis (H₁)**: The research hypothesis suggesting an effect or difference exists
- Example: "The means of age between sexes are different"
- Example: "The variables are associated (there is dependence between them)"

#### P-value Interpretation
**Standard Decision Rule** (consistent across all tests):
- **If p > 0.05**: Accept H₀ (null hypothesis) - no statistically significant difference/association
- **If p < 0.05**: Reject H₀ and accept H₁ - statistically significant difference/association exists

#### Confidence Intervals (CI)
From t-test interpretation (lines 48-49):
- **95% CI not including 0**: Reinforces the difference between groups
- **Negative interval**: The first group's mean is lower than the second
- **Positive interval**: The first group's mean is higher than the second

---

## 2. DECISION TREE FOR TEST SELECTION

### 2.1 Master Decision Framework (from PDF Page 1)

The PDF provides a comprehensive decision table structured by:
1. **Research Objective**
2. **Variable Type**
3. **Appropriate Statistical Test**

#### Complete Decision Matrix

| **OBJECTIVE** | **VARIABLE TYPE** | **TEST** |
|---------------|-------------------|----------|
| Compare means/medians (distributions) between 2 groups | Numeric | • Student's t-test (parametric)<br>• Mann-Whitney test (non-parametric) |
| Compare means/medians (distributions) between 3+ groups | Numeric | • ANOVA (parametric)<br>• Kruskal-Wallis (non-parametric) |
| Verify association between variables | Categorical | • Chi-square test<br>• Fisher's exact test (if cells <5) |
| Verify correlation between variables | Numeric | • Pearson (parametric)<br>• Spearman (non-parametric) |

### 2.2 Parametric vs Non-Parametric Decision

**Key Decision Point**: Data normality and variance homogeneity

**Use PARAMETRIC tests when**:
- Data follows normal distribution (verified by Shapiro-Wilk or Kolmogorov-Smirnov tests)
- Variance homogeneity exists (verified by Levene's test)
- Sample size is adequate

**Use NON-PARAMETRIC tests when**:
- Data does not follow normal distribution
- Large differences in variances between groups
- Small sample sizes
- Presence of significant outliers

---

## 3. DETAILED TEST DESCRIPTIONS

### 3.1 STUDENT'S T-TEST

#### Purpose
Compares means between 2 groups with normal distribution (lines 9-13)

#### Assumptions (lines 10-13)
1. **Dependent variable**: Continuous (numeric) - Examples: VAL_TOT, IDADE, DIAS_PERM
2. **Independent variable**: Categorical (BINARY) - Examples: SEXO, MORTE
3. **Data must have normal distribution**
4. **Homogeneity of variances** (comparable variances)

#### Verification Steps (lines 23-38)

**Step 1: Check for Normal Distribution**
- Visual inspection: Histogram
- Statistical tests:
  - **Shapiro-Wilk test**: `shapiro.test(df$IDADE)`
  - **Kolmogorov-Smirnov test**: `ks.test(df$IDADE, "pnorm")`
- **Interpretation**: Extremely small p-value indicates statistically significant difference from normal distribution (data is NOT normal)

**Step 2: Check Homogeneity of Variances**
- **Levene's test**: `leveneTest(IDADE ~ SEXO, data = df)`
- Tests if variance is comparable across groups

**Step 3: Apply t-test**
- **Equal variances**: `t.test(IDADE ~ SEXO, data = df, var.equal = TRUE)`
- **Unequal variances**: `t.test(IDADE ~ SEXO, data = df, var.equal = FALSE)` (Welch's t-test adjustment)

#### Interpretation (lines 45-49)
- **Degrees of freedom (df)**: Depends on sample sizes
- **P-value interpretation**:
  - p > 0.05: H₀ (means between sexes are equal)
  - p < 0.05: H₁ (means between sexes are different)
- **95% CI interpretation**:
  - Does not include 0: Reinforces difference between ages
  - Negative: First group's mean is lower than second group
- **Practical interpretation example**: "Men are hospitalized, on average, 1.41 years older than women (46.5 - 45.1 = 1.41 years)"

#### When to Use
- Comparing means between exactly 2 groups
- Data meets normality and variance assumptions
- Research question: "Is there a difference in age (at hospitalization) between sexes?" (line 20)

---

### 3.2 MANN-WHITNEY TEST (WILCOXON RANK-SUM TEST)

#### Purpose (lines 52-53)
Non-parametric alternative to t-test when data is not normal or has large variance differences

#### When to Use
- Comparing distributions between 2 groups
- Data does NOT follow normal distribution
- Large differences in variances between groups
- Alternative when t-test assumptions are violated

#### Implementation (line 55)
```r
wilcox.test(IDADE ~ SEXO, data = df)
```

#### Interpretation (lines 57-72)
- **p < 0.05**: Reject null hypothesis (H₀) that ages between sexes are equal
- **Directionality**: Requires additional descriptive statistics
  - Calculate means and medians by group
  - Create boxplots to visualize distributions
  - Example code (lines 60-66): Group by category, calculate mean, median, and count

#### Visualization
Boxplots recommended for showing distribution differences (lines 68-71)

---

### 3.3 ANOVA (ANALYSIS OF VARIANCE)

#### Purpose (lines 74-76)
Compares means of 3 or more groups

#### Assumptions
1. **Dependent variable**: Continuous
2. **Independent variable**: Categorical (with 3+ levels)
3. **Data requirements for validity** (lines 93-99):
   - Residuals must follow normal distribution
   - Groups must have similar variance
   - Observations within each group must be independent

#### Implementation (lines 80-81)
```r
anova_result <- aov(IDADE ~ RACA_COR, data = df)
summary(anova_result)
```

#### Limitation (line 83)
ANOVA only indicates IF there is a difference, but NOT WHICH specific groups are different

#### Post-hoc Testing: Tukey HSD (lines 84-91)
**Purpose**: Identify which specific group pairs are significantly different

**Interpretation of Tukey HSD output**:
- **diff**: Mean difference between compared groups
- **lwr**: Lower limit of 95% confidence interval
- **upr**: Upper limit of 95% confidence interval
- **p adj**: P-value adjusted for multiple comparisons
- **Decision rules**:
  - p adj < 0.05: Groups have significantly different means
  - If confidence interval (lwr - upr) includes 0: Difference is NOT statistically significant

#### Assumption Verification (lines 93-99)

**Test 1: Normality of Residuals**
```r
shapiro.test(residuals(anova_result))
ks.test(residuals(anova_result), "pnorm")
```
Note: Residuals indicate how much each observation deviates from the estimated group mean

**Test 2: Homogeneity of Variance**
Use Levene's test (referenced but not explicitly shown for ANOVA)

**Test 3: Independence**
Must be ensured by study design

#### Research Question Example (line 78)
"Does the average age vary between different race groups?"

---

### 3.4 KRUSKAL-WALLIS TEST

#### Purpose (lines 102-103)
Non-parametric alternative to ANOVA when assumptions are violated

#### When to Use
- Comparing distributions across 3+ groups
- When ANY ANOVA assumption is violated:
  - Non-normal residuals
  - Unequal variances across groups
  - Small sample sizes

#### Implementation (line 105)
```r
kruskal.test(IDADE ~ RACA_COR, data = df)
```

#### Post-hoc Testing: Dunn Test (line 107)
```r
dunnTest(IDADE ~ RACA_COR, data = df, method = "bonferroni")
```
- Uses Bonferroni correction for multiple comparisons
- Identifies which specific groups differ from each other

#### Package Required
`library(FSA)` for `dunnTest()` function (line 6)

---

### 3.5 CHI-SQUARE TEST (QUI-QUADRADO)

#### Purpose (lines 110-111)
Analyzes relationships between categorical variables

#### Requirements
- Large samples (adequate sample size needed)
- All expected cell frequencies should be ≥5

#### When to Use (from PDF)
- Verifying association between categorical variables
- Research question: "Is there a relationship between SEX and mortality (DEATH)?" (line 114)

#### Implementation (lines 116-118)
```r
tabela <- table(df$SEXO, df$MORTE)  # Create contingency table
tabela
chisq.test(tabela)                   # Apply Chi-square test
```

#### Interpretation (lines 120-123)
**Output components**:
- **X-squared**: Test statistic
- **df**: Degrees of freedom
- **p-value interpretation**:
  - p > 0.05: H₀ (variables are independent - no association)
  - p < 0.05: H₁ (variables are associated - dependence exists between them)

#### Visualization
- Contingency tables (line 116)
- Mosaic plots available: `mosaic(~ sexo + morte, data = dados_limpos, shade = TRUE, legend = TRUE)` (from aula_3 script, line 42)

#### Related Functions
- `sjt.xtab()` for formatted cross-tabulation tables with percentages (line 39-40 in aula_3)

---

### 3.6 FISHER'S EXACT TEST

#### Purpose (lines 126-127)
Used for categorical variable association when sample sizes are small

#### When to Use (from PDF and script)
- Small samples (cell frequency <5 in contingency table)
- Alternative to chi-square when sample size assumptions are violated
- Exact probability calculation (not an approximation)

#### Implementation (lines 129-132)
```r
tabela2 <- table(df$RACA_COR, df$SEXO)          # Create contingency table
fisher.test(tabela2)                             # Apply Fisher's test

# For computational limitations:
fisher.test(tabela2, workspace = 2e8)            # Increase calculation space
fisher.test(tabela2, simulate.p.value = TRUE)   # Alternative: simulate p-value
```

#### Computational Considerations
- May require increased workspace for larger tables
- P-value simulation is an alternative for complex calculations

---

### 3.7 CORRELATION TESTS

#### Purpose (lines 135-136)
Indicates the strength and direction of the relationship between two numeric variables

#### Correlation Coefficient Range (lines 138-141)
- **+1**: Perfect positive correlation
- **0**: No correlation
- **-1**: Perfect negative correlation

#### Correlation Strength Interpretation (lines 150-155)
| Coefficient Range | Interpretation |
|-------------------|----------------|
| 0.00 to 0.10 | Negligible correlation |
| 0.10 to 0.30 | Weak correlation |
| 0.30 to 0.50 | Moderate correlation |
| 0.50 to 0.70 | Strong correlation |
| 0.70 to 1.00 | Very strong correlation |

#### Test Selection (line 143)
- **Normal data**: Pearson correlation
- **Non-normal data**: Spearman correlation

#### Implementation (lines 146-147)
```r
cor.test(df$IDADE, df$VAL_TOT, method = "pearson")   # Normal data (Pearson)
cor.test(df$IDADE, df$VAL_TOT, method = "spearman")  # Non-normal data (Spearman)
```

#### Interpretation (line 149)
- **Positive coefficient**: When one variable increases, the other tends to increase
- **Negative coefficient**: When one variable increases, the other tends to decrease

#### Visualization: Scatter Plot (lines 158-164)
```r
ggplot(df, aes(x = IDADE, y = VAL_TOT)) +
  geom_point() +
  geom_smooth(method = "loess", col = "red") +
  labs(x = "Age (in years)", y = "Hospitalization Cost (R$)")
```

#### Limitations and Considerations (lines 166-167)
- Many outliers: Affects the analysis
- Nearly flat curve: Relationship between variables is not strong (verify with coefficient)

#### Alternative Implementation (from aula_3 script, lines 22-30)
Using ggpubr package for enhanced visualization:
```r
ggscatter(dados_limpos, x = "val_tot", y = "idade",
          add = "reg.line",                          # Add regression line
          add.params = list(color = "navyblue"),     # Customize regression line
          conf.int = TRUE,                           # Add confidence interval
          cor.coef = TRUE,                           # Add correlation coefficient
          cor.method = "spearman")                   # Correlation method
```

---

## 4. COMPREHENSIVE ASSUMPTIONS SUMMARY

### 4.1 Parametric Test Assumptions

#### Student's T-Test
1. Continuous dependent variable
2. Binary categorical independent variable
3. Normal distribution of data
4. Homogeneity of variances (comparable variances)

#### ANOVA
1. Continuous dependent variable
2. Categorical independent variable (3+ levels)
3. Normal distribution of residuals
4. Homogeneity of variance across groups
5. Independence of observations within groups

#### Pearson Correlation
1. Both variables continuous/numeric
2. Normal distribution of both variables
3. Linear relationship between variables

### 4.2 Non-Parametric Test Advantages

#### When Assumptions are Violated
- **Mann-Whitney**: Use when t-test assumptions fail
- **Kruskal-Wallis**: Use when ANOVA assumptions fail
- **Spearman**: Use when Pearson correlation assumptions fail

#### Benefits
- No normality requirement
- More robust to outliers
- Applicable to smaller sample sizes
- Can handle ordinal data

### 4.3 Categorical Test Requirements

#### Chi-Square Test
1. Categorical variables
2. Large sample size
3. Expected cell frequencies ≥5
4. Independent observations

#### Fisher's Exact Test
1. Categorical variables
2. Small sample sizes acceptable
3. Any expected cell frequency (especially <5)
4. Independent observations

---

## 5. INTERPRETATION GUIDELINES

### 5.1 General Interpretation Framework

#### Step 1: Statistical Significance
- Check p-value against significance level (typically 0.05)
- Determine if result is statistically significant

#### Step 2: Effect Size and Direction
- For mean comparisons: Calculate actual difference
- For correlations: Interpret coefficient magnitude and sign
- For associations: Examine contingency table proportions

#### Step 3: Confidence Intervals
- Check if CI includes null value (0 for differences, 1 for ratios)
- Examine CI width (narrow = more precise estimate)

#### Step 4: Practical Significance
- Translate statistical findings into real-world context
- Consider clinical, economic, or practical importance
- Example (line 49): "Men are hospitalized 1.41 years older on average"

### 5.2 Test-Specific Interpretation

#### T-Test Output Components
- **t-statistic**: Test statistic value
- **df**: Degrees of freedom
- **p-value**: Probability of observing results under H₀
- **95% CI**: Range for true mean difference
- **Sample estimates**: Means for each group

#### ANOVA Output Components
- **F-statistic**: Ratio of between-group to within-group variance
- **df**: Degrees of freedom (between groups, within groups)
- **p-value**: Overall test of any group differences
- **Requires post-hoc**: Tukey HSD to identify specific differences

#### Chi-Square Output Components
- **X-squared**: Test statistic
- **df**: Degrees of freedom
- **p-value**: Test of independence
- **Contingency table**: Observed frequencies

#### Correlation Output Components
- **r or rho**: Correlation coefficient (-1 to +1)
- **p-value**: Test of correlation ≠ 0
- **95% CI**: Range for true correlation
- **Direction**: Positive or negative relationship

### 5.3 Reporting Results

#### Complete Reporting Should Include
1. Test name and type (parametric/non-parametric)
2. Test statistic and degrees of freedom
3. P-value
4. Effect size or actual difference
5. Confidence interval (when applicable)
6. Sample sizes
7. Practical interpretation

---

## 6. EXAMPLES AND PRACTICAL APPLICATIONS

### 6.1 Example 1: T-Test Application (lines 20-49)

**Research Question**: "Is there a difference in age (at hospitalization) between sexes?"

**Variables**:
- Dependent: IDADE (age, continuous)
- Independent: SEXO (sex, binary)

**Analysis Steps**:
1. Visual inspection: Histogram of age distribution
2. Normality test: Shapiro-Wilk or Kolmogorov-Smirnov
3. Variance homogeneity: Levene's test
4. Apply t-test (equal or unequal variances based on Levene's result)

**Interpretation Example**:
- H₀: Mean age at hospitalization is equal between sexes
- H₁: Mean age at hospitalization differs between sexes
- Result: Men hospitalized 1.41 years older on average (46.5 vs 45.1 years)
- Practical meaning: Sex-based age difference in hospitalization patterns

### 6.2 Example 2: Mann-Whitney Application (lines 52-72)

**Same Research Question**: When t-test assumptions are violated

**Analysis Steps**:
1. Apply non-parametric alternative: `wilcox.test()`
2. If p < 0.05: Reject H₀
3. Calculate descriptive statistics by group to determine direction
4. Create boxplots for visualization

**Interpretation Focus**:
- Statistical significance from test
- Directionality from descriptive statistics
- Visual confirmation from boxplot

### 6.3 Example 3: ANOVA Application (lines 78-99)

**Research Question**: "Does the average age vary between different race groups?"

**Variables**:
- Dependent: IDADE (age, continuous)
- Independent: RACA_COR (race/color, categorical with 3+ levels)

**Analysis Steps**:
1. Fit ANOVA model: `aov(IDADE ~ RACA_COR)`
2. Check overall significance: `summary(anova_result)`
3. If significant, run post-hoc: `TukeyHSD(anova_result)`
4. Verify assumptions:
   - Normality of residuals
   - Homogeneity of variance
   - Independence

**Interpretation**:
- ANOVA: Overall test if ANY groups differ
- Tukey HSD: Identifies WHICH specific group pairs differ
- Examine confidence intervals and adjusted p-values

### 6.4 Example 4: Kruskal-Wallis Application (lines 102-107)

**Same Research Question**: When ANOVA assumptions violated

**Analysis Steps**:
1. Apply non-parametric alternative: `kruskal.test()`
2. If significant, run post-hoc: Dunn test with Bonferroni correction
3. Identify specific group differences

### 6.5 Example 5: Chi-Square Application (lines 114-123)

**Research Question**: "Is there a relationship between SEX and mortality (MORTE)?"

**Variables**:
- Both categorical (SEXO and MORTE)

**Analysis Steps**:
1. Create contingency table: `table(df$SEXO, df$MORTE)`
2. Apply chi-square test: `chisq.test(tabela)`
3. Examine contingency table proportions
4. Create mosaic plot for visualization

**Interpretation**:
- H₀: Sex and mortality are independent (no association)
- H₁: Sex and mortality are associated
- Examine observed vs expected frequencies in contingency table

### 6.6 Example 6: Fisher's Exact Test Application (lines 129-132)

**Research Question**: Association between race and sex with small sample sizes

**Variables**:
- Both categorical (RACA_COR and SEXO)

**When to Use**:
- When contingency table has cells with frequency <5
- Chi-square assumptions not met

**Analysis Steps**:
1. Create contingency table
2. Apply Fisher's exact test
3. If computational issues: increase workspace or simulate p-value

### 6.7 Example 7: Correlation Application (lines 145-167)

**Research Question**: "Is there a correlation between age and hospitalization cost?"

**Variables**:
- Both continuous (IDADE and VAL_TOT)

**Analysis Steps**:
1. Check normality of both variables
2. If normal: Pearson correlation
3. If non-normal: Spearman correlation
4. Create scatter plot with regression line
5. Interpret coefficient magnitude and direction

**Interpretation Considerations**:
- Correlation coefficient: Strength and direction
- P-value: Statistical significance
- Scatter plot: Visual inspection for linearity, outliers
- Practical meaning: "When age increases, hospitalization cost tends to increase"

**Cautions** (lines 166-167):
- Many outliers affect analysis reliability
- Nearly flat curve indicates weak relationship despite significant p-value

---

## 7. REQUIRED R PACKAGES

### Core Packages (from scripts)
```r
library(car)        # For Levene's test (variance homogeneity)
library(ggplot2)    # For visualization
library(dplyr)      # For data manipulation
library(FSA)        # For Dunn test (Kruskal-Wallis post-hoc)
library(tidyverse)  # Comprehensive data science package
library(sjPlot)     # For formatted tables (sjt.xtab)
library(vcd)        # For mosaic plots
library(ggpubr)     # For correlation scatter plots with statistics
```

### Package Functions Summary
- **car**: `leveneTest()`
- **FSA**: `dunnTest()`
- **sjPlot**: `sjt.xtab()`
- **vcd**: `mosaic()`
- **ggpubr**: `ggscatter()`

---

## 8. VISUAL SUMMARY FROM PDF

### Decision Tree Visualization (PDF Page 1)

The PDF includes a visual representation showing:
- **Header**: "TESTES ESTATÍSTICOS" (Statistical Tests)
- **Visual element**: Normal distribution curve (bell curve) labeled "Distribuição normal"
- **Structured table**: Organizing tests by objective, variable type, and appropriate test

**Key Visual Insight**:
The normal distribution curve emphasizes the importance of normality in test selection, serving as the dividing line between parametric and non-parametric approaches.

---

## 9. KEY PEDAGOGICAL INSIGHTS

### 9.1 Systematic Approach to Statistical Testing

The materials emphasize a **structured 3-step approach**:
1. **Verify assumptions** (normality, homogeneity)
2. **Select appropriate test** (based on assumption checks)
3. **Interpret results** (statistical + practical significance)

### 9.2 Paired Tests Structure

The content systematically presents **parametric-nonparametric pairs**:
- T-test ↔ Mann-Whitney
- ANOVA ↔ Kruskal-Wallis
- Pearson ↔ Spearman

This pairing helps learners understand alternatives when assumptions fail.

### 9.3 Progressive Complexity

Tests are presented in logical order:
1. Simple comparisons (2 groups: t-test/Mann-Whitney)
2. Multiple comparisons (3+ groups: ANOVA/Kruskal-Wallis)
3. Categorical associations (Chi-square/Fisher's)
4. Continuous relationships (Correlation)

### 9.4 Integration of Theory and Practice

Each test includes:
- Theoretical assumptions
- Practical R implementation
- Real-world research questions
- Interpretation guidelines
- Visualization approaches

---

## 10. RECOMMENDATIONS FOR CHAPTER 4

### 10.1 Suggested Chapter Structure

1. **Introduction to Hypothesis Testing**
   - Null and alternative hypotheses
   - P-values and significance levels
   - Type I and Type II errors

2. **Test Selection Framework**
   - Decision tree from PDF
   - Parametric vs non-parametric decision logic

3. **Comparing Two Groups**
   - T-test (assumptions, implementation, interpretation)
   - Mann-Whitney (when to use, implementation)
   - Practical example with complete workflow

4. **Comparing Multiple Groups**
   - ANOVA (assumptions, implementation, post-hoc testing)
   - Kruskal-Wallis (when to use, post-hoc testing)
   - Practical example with complete workflow

5. **Categorical Variable Association**
   - Chi-square test (assumptions, interpretation)
   - Fisher's exact test (when to use)
   - Practical example with contingency tables

6. **Correlation Analysis**
   - Pearson correlation (assumptions, interpretation)
   - Spearman correlation (when to use)
   - Practical example with visualization

7. **Assumption Checking**
   - Normality tests (Shapiro-Wilk, Kolmogorov-Smirnov)
   - Variance homogeneity (Levene's test)
   - Practical workflows

8. **Comprehensive Case Studies**
   - Multi-step analyses
   - Decision-making process
   - Common pitfalls

### 10.2 Integration with Existing Chapters

**Links to Chapter 1 (Introduction to R)**:
- Reference R packages and functions
- Build on data manipulation skills

**Links to Chapter 2 (Data Manipulation)**:
- Group_by and summarise for descriptive statistics
- Data preparation for testing

**Links to Chapter 3 (Exploratory Analysis)**:
- Visual inspection before testing
- Descriptive statistics as complement to tests
- Distribution assessment

### 10.3 Practical Examples to Include

All examples should use the hospitalization dataset:
1. Age comparison between sexes (t-test/Mann-Whitney)
2. Age comparison across race groups (ANOVA/Kruskal-Wallis)
3. Sex and mortality association (Chi-square)
4. Race and sex association (Fisher's exact)
5. Age and hospitalization cost correlation (Pearson/Spearman)

### 10.4 Learning Objectives

By the end of Chapter 4, readers should be able to:
1. Formulate appropriate null and alternative hypotheses
2. Select the correct statistical test based on variable types and assumptions
3. Check test assumptions using R
4. Implement tests correctly in R
5. Interpret test output including p-values, confidence intervals, and effect sizes
6. Translate statistical results into practical conclusions
7. Create appropriate visualizations for each test type
8. Recognize when to use parametric vs non-parametric alternatives

---

## 11. CODE TEMPLATES FOR CHAPTER

### Template 1: Complete T-Test Workflow
```r
# Research Question: Is there a difference in [VARIABLE] between [GROUPS]?

# Step 1: Visual inspection
ggplot(data, aes(x = VARIABLE)) + geom_histogram()

# Step 2: Test normality
shapiro.test(data$VARIABLE)

# Step 3: Test variance homogeneity
leveneTest(VARIABLE ~ GROUP, data = data)

# Step 4: Apply appropriate t-test
t.test(VARIABLE ~ GROUP, data = data, var.equal = TRUE)  # or FALSE

# Step 5: Calculate descriptive statistics
data %>%
  group_by(GROUP) %>%
  summarise(Mean = mean(VARIABLE), SD = sd(VARIABLE), n = n())

# Step 6: Visualize
ggplot(data, aes(x = GROUP, y = VARIABLE, fill = GROUP)) +
  geom_boxplot()
```

### Template 2: Complete ANOVA Workflow
```r
# Research Question: Does [VARIABLE] differ across [GROUPS]?

# Step 1: Fit ANOVA model
anova_result <- aov(VARIABLE ~ GROUP, data = data)

# Step 2: Check overall significance
summary(anova_result)

# Step 3: Post-hoc testing (if significant)
TukeyHSD(anova_result)

# Step 4: Check assumptions
shapiro.test(residuals(anova_result))
leveneTest(VARIABLE ~ GROUP, data = data)

# Alternative: Kruskal-Wallis if assumptions violated
kruskal.test(VARIABLE ~ GROUP, data = data)
dunnTest(VARIABLE ~ GROUP, data = data, method = "bonferroni")
```

### Template 3: Chi-Square Workflow
```r
# Research Question: Is there an association between [VAR1] and [VAR2]?

# Step 1: Create contingency table
table_result <- table(data$VAR1, data$VAR2)
table_result

# Step 2: Apply chi-square test
chisq.test(table_result)

# Step 3: Check cell frequencies
chisq.test(table_result)$expected

# Alternative: Fisher's exact test if cells <5
fisher.test(table_result)

# Step 4: Visualize
mosaic(~ VAR1 + VAR2, data = data, shade = TRUE, legend = TRUE)
```

### Template 4: Correlation Workflow
```r
# Research Question: Is there a correlation between [VAR1] and [VAR2]?

# Step 1: Check normality
shapiro.test(data$VAR1)
shapiro.test(data$VAR2)

# Step 2: Apply appropriate correlation test
cor.test(data$VAR1, data$VAR2, method = "pearson")   # or "spearman"

# Step 3: Visualize
ggplot(data, aes(x = VAR1, y = VAR2)) +
  geom_point() +
  geom_smooth(method = "lm", col = "blue")

# Alternative: Enhanced visualization
ggscatter(data, x = "VAR1", y = "VAR2",
          add = "reg.line",
          conf.int = TRUE,
          cor.coef = TRUE,
          cor.method = "pearson")
```

---

## 12. SUMMARY TABLE: QUICK REFERENCE GUIDE

| Test | Purpose | Variable Types | Assumptions | R Function | Post-hoc |
|------|---------|---------------|-------------|------------|----------|
| **Student's t-test** | Compare 2 group means | Numeric + Binary categorical | Normal distribution, homogeneous variance | `t.test()` | N/A |
| **Mann-Whitney** | Compare 2 group distributions | Numeric + Binary categorical | None (non-parametric) | `wilcox.test()` | N/A |
| **ANOVA** | Compare 3+ group means | Numeric + Categorical (3+ levels) | Normal residuals, homogeneous variance, independence | `aov()` | `TukeyHSD()` |
| **Kruskal-Wallis** | Compare 3+ group distributions | Numeric + Categorical (3+ levels) | None (non-parametric) | `kruskal.test()` | `dunnTest()` |
| **Chi-square** | Test association | Categorical + Categorical | Large sample, expected frequencies ≥5 | `chisq.test()` | N/A |
| **Fisher's exact** | Test association (small samples) | Categorical + Categorical | None (works with small samples) | `fisher.test()` | N/A |
| **Pearson correlation** | Measure linear relationship | Numeric + Numeric | Normal distributions, linear relationship | `cor.test(method="pearson")` | N/A |
| **Spearman correlation** | Measure monotonic relationship | Numeric + Numeric | None (non-parametric) | `cor.test(method="spearman")` | N/A |

---

## 13. COMMON PITFALLS AND SOLUTIONS

### Pitfall 1: Not Checking Assumptions
**Solution**: Always verify assumptions before applying parametric tests. Use visual inspection + formal tests.

### Pitfall 2: Confusing Correlation with Causation
**Solution**: Emphasize that correlation indicates association, not causal relationship.

### Pitfall 3: Multiple Testing Without Correction
**Solution**: Use adjusted p-values (Bonferroni, Tukey HSD) for multiple comparisons.

### Pitfall 4: Ignoring Practical Significance
**Solution**: Always interpret effect sizes and confidence intervals, not just p-values.

### Pitfall 5: Using Chi-Square with Small Samples
**Solution**: Check expected frequencies; use Fisher's exact test when cells <5.

### Pitfall 6: Misinterpreting P-values
**Solution**: P-value indicates probability of observing results under H₀, not probability H₀ is true.

### Pitfall 7: Assuming Normality Without Testing
**Solution**: Always test for normality, especially with smaller samples or presence of outliers.

---

## CONCLUSION

This extracted content provides a comprehensive foundation for Chapter 4 (Statistical Tests) of the biostatistics book. The materials emphasize:

1. **Systematic approach**: Clear decision frameworks for test selection
2. **Assumption checking**: Explicit verification before applying tests
3. **Paired alternatives**: Parametric tests with non-parametric backups
4. **Practical interpretation**: Connecting statistical results to real-world meaning
5. **Complete workflows**: From research question to visualization
6. **Real examples**: Using hospitalization dataset throughout

The integration of PDF decision tree, detailed R script comments, and practical examples creates a coherent learning path for students to master statistical hypothesis testing in R.
