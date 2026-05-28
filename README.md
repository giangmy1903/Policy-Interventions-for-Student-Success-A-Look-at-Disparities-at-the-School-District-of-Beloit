# Policy Interventions for Student Success: A Look at Disparities at the School District of Beloit

This repository contains the analysis, modeling code and final case‑study report for the project “Policy Interventions for Student Success: A Look at Disparities at the School District of Beloit.” The goal of the study is to investigate how factors such as race, health, parental involvement and English proficiency relate to students’ academic outcomes, and to propose policy interventions that address observed disparities. The analysis was undertaken as part of the Senior Seminar for Data Science and Data Analytics (DSDA) at Beloit College.

## Overview

The School District of Beloit is racially and socio‑economically diverse. Achievement gaps persist between student groups and there is ongoing debate about whether differences in test scores are driven by school factors (e.g., resources, pedagogy) or family factors (e.g., parental involvement, health, language proficiency). In this case study we analyse a dataset of winter 2018–19 Measures of Academic Progress (MAP) test results and survey responses for 215 students (27 categorical and 3 numeric variables). We model Math and Reading scores using principal component analysis (PCA) and Lasso regression to identify significant predictors and propose targeted interventions.

## Research questions

The study aims to answer:

Do academic performance and MAP test scores differ between racial or ethnic groups? Differences by majority/minority group were evaluated to see whether race correlates with Math or Reading outcomes.
Are health disparities significant determinants of test scores? The dataset included self‑reported child health, which was recoded to compare “very good” health with other levels.
How does English proficiency affect Math and Reading scores? English proficiency categories (e.g., Not Tested, Emerging, Developing, Expanding, Proficient) were consolidated and one‑hot encoded to test whether proficiency predicts achievement.
Key findings
Belonging to a minority group (i.e., non‑white) has a negative and statistically significant effect on Math and Reading scores, even after controlling for parental involvement and other factors.
Health disparities were not found to be statistically significant predictors of Math or Reading scores once other factors were controlled.
Students with higher English‑proficiency levels scored significantly higher in Reading; proficient students scored about 60 points higher on the Reading RIT scale compared with those in lower proficiency levels.
Robinson Elementary School exhibited markedly lower Math and Reading scores than other schools; Math scores were ~60 points lower and Reading scores were ~71 points lower for Robinson students. Todd Elementary also showed lower Reading scores (−50 points).
Caucasian (majority group) students scored higher on average—about 23 points higher in Math and 27.6 points higher in Reading.

These results suggest that race and English proficiency, rather than self‑reported health, are key drivers of test score disparities in the district..

## Data description

The dataset contains 215 student records with 30 explanatory variables, grouped into three domains:

Student demographics: variables such as race, grade, and majority/minority group indicator. A new variable Majority_Group was created to collapse duplicate race categories (e.g., Hispanic, Multi Race and Fed Race were removed due to duplication).
Parental involvement and socio‑economic factors: variables derived from survey questions about parents’ volunteer frequency, bedtime routines, library visits and number of books at home.
Academic information: Math and Reading RIT scores, English proficiency levels, school names and other educational indicators.

Outcome variables: The primary outcomes analysed are the students’ Math and Reading RIT scores from the Winter 2018–2019 MAP test.

## Data cleaning

To prepare the data for analysis, we performed several preprocessing steps:

Variable consolidation: Redundant race variables (Hispanic, Multi Race, Fed Race) were removed and replaced with a single Majority_Group indicator distinguishing majority (Caucasian/Asian) from minority groups. The Eng_Prof variable was replaced with a more detailed Eng_Prof_Full for English proficiency.
Creation of derived variables: Two new variables were constructed: Majority_Group and child_health_level_very_good_compare, which indicates whether the child’s health was reported as “very good”.
Dropping unnecessary columns: Personal identifiers (student ID, date of birth, etc.) and fields with large amounts of missing or imbalanced responses (e.g., job and guardian columns) were removed.
Handling missing values: Observations lacking numeric data were dropped, and missing categorical values were replaced with a NULL category.
Encoding: All categorical variables were one‑hot encoded. Continuous variables were standardised for PCA and regression modelling.

The cleaned dataset is saved in the data/ folder (if provided) for reproducibility.

## Exploratory analysis & methodology

The cleaned data were explored through descriptive statistics and visualisation. Principal component analysis (PCA) was used to address multicollinearity and reduce dimensionality: the first ~40 principal components explained about 80 % of the variance. Lasso regression was then applied to select the most influential predictors for Math and Reading scores. Separate linear models were estimated for Math and Reading scores, and heteroskedasticity tests (White’s and Breusch–Pagan) were conducted. Robust standard errors were used for the Math model because the White test indicated heteroskedasticity (p ≈ 0.0024, not reported in the case study but noted in the accompanying DSDA technical report).

Math performance model

The Lasso procedure selected the following key predictors for Math scores:

Old_School_ROBINSON ELEMENTARY SCHOOL: indicator for attending Robinson Elementary.
Majority_Group_1: student belongs to the majority (Caucasian/Asian).
Eng_Prof_Full_PROFICIENT: English proficiency level.
English_Language_Development_EMP: measure of language development.

The final Math regression model (robust SE) had an R² of 0.248 and the following significant effects:

Predictor	Interpretation
Old_School_ROBINSON	Students attending Robinson Elementary scored ~60 points lower on Math RIT compared with other schools.
Majority_Group (majority)	Majority‑group students scored about 23 points higher than minority students.
English proficiency (Proficient)	Higher English proficiency was associated with higher Math scores (coefficient ~12 points; not as strong as for Reading).
Child health (Very good)	Not statistically significant, suggesting health disparities were not linked to Math performance.
Reading performance model

For Reading scores, the Lasso procedure selected similar and additional variables. Tests for heteroskedasticity indicated no significant issue, so ordinary least squares standard errors were retained. The model explained about 53 % of the variation in Reading scores (R² ≈ 0.532). Significant results include:

Predictor	Interpretation
Old_School_ROBINSON	Students at Robinson Elementary scored ~71 points lower in Reading than those at other schools.
Old_School_TODD	Students at Todd Elementary scored ~50 points lower in Reading.
Majority_Group (majority)	Caucasian/Asian students scored about 27.6 points higher in Reading.
English proficiency (Proficient)	Proficient students scored about 60 points higher in Reading.
Parental involvement variables	Some measures (e.g., number of books at home, library visits) had minor positive effects but were not always statistically significant.
Policy recommendations

Based on the findings, the study proposes targeted interventions:

Tutoring and mentoring programmes for minority students: Provide dedicated tutoring, mentoring and after‑school programmes to support minority students who may face systemic disadvantages.
Culturally responsive pedagogy: Employ teaching methods that reflect students’ cultures and languages, and invest in bilingual or ESL programmes to improve English proficiency.
Revitalise Robinson Elementary School: Allocate resources to improve teaching quality, learning environments and school leadership at underperforming schools like Robinson Elementary.
## Limitations & future work
Sample size & generalisability: The dataset comprises only 215 observations from one testing period, limiting the ability to generalise findings to other years or districts. Future studies could incorporate longitudinal data to assess trends over time.
Measurement error: Self‑reported variables (e.g., child health, parental involvement) may be biased; exploring alternative measures (such as school attendance records or health assessments) could improve model accuracy.
Unobserved confounders: Factors such as school funding, teacher quality or socio‑economic status were not fully captured; including additional variables or applying causal inference techniques could help isolate effects.
Model complexity: Lasso regression selected a limited set of predictors, but exploring non‑linear models or hierarchical (multilevel) structures might better capture relationships between schools and student outcomes.
