# Power Outages and Economic Strength in the United States  
### An Analysis of Outage Severity, Economic Indicators, and Structural Vulnerability (2000–2016)

---

## **1. Introduction**
Power outages disrupt communities, strain infrastructure, and slow economic activity across the United States. While extreme weather and equipment failures are well-known triggers, not every state experiences outages with the same severity. This raises an important question: **do states with stronger economies recover from outages more quickly than those with weaker economies?**

This project explores that question by analyzing **1,534 major U.S. power outage events** recorded from **2000 to 2016**. 
The cleaned dataset contains 1,534 rows, each representing a major power outage event. The primary columns relevant to this analysis include:
- OUTAGE.DURATION: total outage duration in hours
- CUSTOMERS.AFFECTED: number of customers impacted
- PC.REALGSP.STATE: per-capita real gross state product
- CAUSE.CATEGORY: primary cause of the outage
- CLIMATE.REGION: regional climate classification
- STATE: U.S. state in which the outage occurred


Two primary measures define outage severity:

- **Outage Duration** — how long the power remained out  
- **Customers Affected** — the scale of the impact across households and businesses  

### **Guiding Question**

**Do states with lower per-capita economic output experience longer and more severe power outages?**

Investigating this relationship helps reveal how economic conditions, infrastructure investment, and public policy shape the resiliency of the power grid. Understanding these structural patterns is essential for improving disaster response, infrastructure planning, and community preparedness.

---

## **2. Data Cleaning and Exploratory Data Analysis**

Before conducting any analysis, I cleaned the dataset to convert all numerical values stored as strings into proper numeric types, and I parsed all date columns into datetime format. In cases where the dataset did not provide an outage duration, I computed it manually using the start and restoration timestamps. I also created an economic tier variable that groups states into quartiles based on their per-capita real gross state product. These cleaning steps ensure that the analyses and visualizations reflect meaningful relationships in the data.

|   variables |   OBS |   YEAR |   MONTH | U.S._STATE   | POSTAL.CODE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | OUTAGE.START.DATE   | OUTAGE.START.TIME   | OUTAGE.RESTORATION.DATE   | OUTAGE.RESTORATION.TIME   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   HURRICANE.NAMES |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   RES.PRICE |   COM.PRICE |   IND.PRICE |   TOTAL.PRICE |   RES.SALES |   COM.SALES |   IND.SALES |   TOTAL.SALES |   RES.PERCEN |   COM.PERCEN |   IND.PERCEN |   RES.CUSTOMERS |   COM.CUSTOMERS |   IND.CUSTOMERS |   TOTAL.CUSTOMERS |   RES.CUST.PCT |   COM.CUST.PCT |   IND.CUST.PCT |   PC.REALGSP.STATE |   PC.REALGSP.USA |   PC.REALGSP.REL |   PC.REALGSP.CHANGE |   UTIL.REALGSP |   TOTAL.REALGSP |   UTIL.CONTRI |   PI.UTIL.OFUSA |   POPULATION |   POPPCT_URBAN |   POPPCT_UC |   POPDEN_URBAN |   POPDEN_UC |   POPDEN_RURAL |   AREAPCT_URBAN |   AREAPCT_UC |   PCT_LAND |   PCT_WATER_TOT |   PCT_WATER_INLAND |   computed_duration |   final_duration | econ_bin   | duration_missing   |
|------------:|------:|-------:|--------:|:-------------|:--------------|:--------------|:-------------------|----------------:|:-------------------|:--------------------|:--------------------|:--------------------------|:--------------------------|:-------------------|:------------------------|------------------:|------------------:|-----------------:|---------------------:|------------:|------------:|------------:|--------------:|------------:|------------:|------------:|--------------:|-------------:|-------------:|-------------:|----------------:|----------------:|----------------:|------------------:|---------------:|---------------:|---------------:|-------------------:|-----------------:|-----------------:|--------------------:|---------------:|----------------:|--------------:|----------------:|-------------:|---------------:|------------:|---------------:|------------:|---------------:|----------------:|-------------:|-----------:|----------------:|-------------------:|--------------------:|-----------------:|:-----------|:-------------------|
|         nan |     1 |   2011 |       7 | Minnesota    | MN            | MRO           | East North Central |            -0.3 | normal             | 2011-07-01 00:00:00 | 17:00:00            | 2011-07-03 00:00:00       | 20:00:00                  | severe weather     | nan                     |               nan |              3060 |              nan |                70000 |       11.6  |        9.18 |        6.81 |          9.28 |     2332915 |     2114774 |     2113291 |       6562520 |      35.5491 |      32.225  |      32.2024 |         2308736 |          276286 |           10673 |           2595696 |        88.9448 |        10.644  |       0.411181 |              51268 |            47586 |          1.07738 |                 1.6 |           4802 |          274182 |       1.75139 |             2.2 |      5348119 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |                  48 |             3060 | High       | False              |
|         nan |     2 |   2014 |       5 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | 2014-05-11 00:00:00 | 18:38:00            | 2014-05-11 00:00:00       | 18:39:00                  | intentional attack | vandalism               |               nan |                 1 |              nan |                  nan |       12.12 |        9.71 |        6.49 |          9.28 |     1586986 |     1807756 |     1887927 |       5284231 |      30.0325 |      34.2104 |      35.7276 |         2345860 |          284978 |            9898 |           2640737 |        88.8335 |        10.7916 |       0.37482  |              53499 |            49091 |          1.08979 |                 1.9 |           5226 |          291955 |       1.79    |             2.2 |      5457125 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |                   0 |                1 | High       | False              |
|         nan |     3 |   2010 |      10 | Minnesota    | MN            | MRO           | East North Central |            -1.5 | cold               | 2010-10-26 00:00:00 | 20:00:00            | 2010-10-28 00:00:00       | 22:00:00                  | severe weather     | heavy wind              |               nan |              3000 |              nan |                70000 |       10.87 |        8.19 |        6.07 |          8.15 |     1467293 |     1801683 |     1951295 |       5222116 |      28.0977 |      34.501  |      37.366  |         2300291 |          276463 |           10150 |           2586905 |        88.9206 |        10.687  |       0.392361 |              50447 |            47287 |          1.06683 |                 2.7 |           4571 |          267895 |       1.70627 |             2.1 |      5310903 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |                  48 |             3000 | High       | False              |
|         nan |     4 |   2012 |       6 | Minnesota    | MN            | MRO           | East North Central |            -0.1 | normal             | 2012-06-19 00:00:00 | 04:30:00            | 2012-06-20 00:00:00       | 23:00:00                  | severe weather     | thunderstorm            |               nan |              2550 |              nan |                68200 |       11.79 |        9.25 |        6.71 |          9.19 |     1851519 |     1941174 |     1993026 |       5787064 |      31.9941 |      33.5433 |      34.4393 |         2317336 |          278466 |           11010 |           2606813 |        88.8954 |        10.6822 |       0.422355 |              51598 |            48156 |          1.07148 |                 0.6 |           5364 |          277627 |       1.93209 |             2.2 |      5380443 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |                  24 |             2550 | High       | False              |
|         nan |     5 |   2015 |       7 | Minnesota    | MN            | MRO           | East North Central |             1.2 | warm               | 2015-07-18 00:00:00 | 02:00:00            | 2015-07-19 00:00:00       | 07:00:00                  | severe weather     | nan                     |               nan |              1740 |              250 |               250000 |       13.07 |       10.16 |        7.74 |         10.43 |     2028875 |     2161612 |     1777937 |       5970339 |      33.9826 |      36.2059 |      29.7795 |         2374674 |          289044 |            9812 |           2673531 |        88.8216 |        10.8113 |       0.367005 |              54431 |            49844 |          1.09203 |                 1.7 |           4873 |          292023 |       1.6687  |             2.2 |      5489594 |          73.27 |       15.28 |           2279 |      1700.5 |           18.2 |            2.14 |          0.6 |    91.5927 |         8.40733 |            5.47874 |                  24 |             1740 | Highest    | False              |

### Univariate Analysis

The distribution of outage duration shows a heavy right-skew: most outages are relatively short, but a small number last for extremely long periods. This behavior is typical for extreme events.

<iframe 
  src="assets/outage-duration-histogram.html" 
  width="800"
  height="600"
  frameborder="0"></iframe>

### Bivariate Analysis

To explore the relationship between economic conditions and outage severity, I visualized how outage duration and customers affected vary with state economic output. While the relationship is noisy, higher-income states tend to experience slightly less severe outages.

<iframe 
  src="assets/PC.REALGSP.STATE-vs-final_duration.html" 
  width="800"
  height="600"
  frameborder="0"></iframe>

### Interesting Aggregates

To summarize broader trends, I computed the average outage duration and average customers affected for each economic quartile. These aggregated statistics highlight systematic differences that motivate the hypothesis testing that follows.

| econ_bin   |   final_duration |   CUSTOMERS.AFFECTED |
|:-----------|-----------------:|---------------------:|
| Lowest     |          2995.78 |               127790 |
| Low        |          3193.23 |               155391 |
| High       |          1744.5  |               150617 |
| Highest    |          2608.36 |               139920 |

---

## **3. Assessment of Missingness**
To assess whether outage duration is missing at random, I analyzed the dependency of its missingness on other variables using permutation tests.

I believe that OUTAGE.DURATION may be NMAR, because outages that are extremely long or politically sensitive may be less likely to have complete reporting. Additional data such as utility reporting policies or regulatory oversight levels could help explain this missingness and potentially render it MAR.

In contrast, the missingness of outage duration does not appear to depend on the month in which the outage occurred. A permutation test comparing months showed no significant difference, providing evidence that missingness is independent of seasonal timing.

<iframe 
  src="assets/duration_missing_vs_gsp.html" 
  width="800"
  height="600"
  frameborder="0"></iframe>

---

## **4. Hypothesis Testing**

To evaluate whether economic conditions are associated with outage severity, I conducted two permutation-based hypothesis tests comparing low-GDP and high-GDP states. In both tests, the difference in means was used as the test statistic, and a significance level of α = 0.05 was chosen.

### Outage Duration

The observed difference in mean outage duration between low-GDP and high-GDP states was approximately 387 hours, with outages in lower-GDP states lasting longer on average. However, the permutation test resulted in a p-value of 0.181, indicating that this difference is not statistically significant. Therefore, there is insufficient evidence to conclude that outage duration differs systematically based on state economic output.

<iframe
  src="assets/hypothesis1_duration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Customers Affected

The observed difference in mean customers affected was approximately −12,131, suggesting that outages in higher-GDP states may affect more customers. However, the permutation test yielded a p-value of 0.682, providing strong evidence that such a difference could occur by chance alone. As a result, there is no statistical evidence that customer impact differs by economic tier.

<iframe
  src="assets/hypothesis2_customers.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

## **5. Framing a Prediction Problem**

The prediction task in this project is to determine whether a power outage will be **high severity**, defined as affecting more than 10,000 customers. This is framed as a binary classification problem, where the objective is to identify outages that are likely to cause widespread disruption.

This task was chosen because classifying outages by severity is more actionable than predicting exact outage duration or customer counts. Early identification of high-impact outages can help utilities and emergency planners prioritize resources, allocate response teams efficiently, and mitigate downstream effects.

The response variable is a binary indicator of outage severity:

- Severe outage: affects more than 10,000 customers  
- Non-severe outage: affects 10,000 or fewer customers  

Model performance is evaluated using **F1-score**, which balances precision and recall. This metric is more appropriate than accuracy because severe outages are relatively rare, and both false positives and false negatives carry meaningful consequences for planning and response. F1-score was chosen over accuracy because severe outages are relatively rare, and accuracy would be dominated by the majority non-severe class.

Only features that would be known at the start of an outage are used for prediction. These include economic indicators, geographic information, climate region, and outage cause. Variables that describe the outcome of the outage itself are intentionally excluded to ensure a realistic and fair prediction setup.


---

## **6. Baseline Model**
The baseline model is a logistic regression classifier trained using the following features known at the time of prediction, per-capita GSP (quantitative), outage cause (nominal), climate region (nominal), and state economic quartile (ordinal). Nominal features were one-hot encoded, while quantitative features were standardized.

The model performs surprisingly well, achieving a high F1-score of **0.98** on unseen data. The model is particularly effective at identifying severe outages, with both high precision and recall. This strong performance suggests that key predictors such as outage cause and state-level economic indicators already contain substantial information about outage severity.

However, this model still serves as a baseline because it uses a limited set of features and assumes a linear relationship between predictors and the outcome. In the final model, I explore whether additional feature engineering and more flexible modeling approaches can further improve performance and provide more robust predictions.

---

## **7. Final Model**

To improve upon the baseline model, I trained a more flexible final model using a Random Forest classifier. Unlike linear models, Random Forests can capture nonlinear relationships and interactions between features, making them well-suited for modeling complex systems such as power grid failures.

In addition to the baseline features, I engineered two new predictors. First, I applied a log transformation to the per-capita real gross state product to reduce skew and emphasize relative economic differences between states. Second, I incorporated climate region as a categorical feature, providing environmental context that may influence infrastructure vulnerability and outage severity.

All preprocessing and modeling steps were implemented within a single `sklearn` Pipeline to prevent data leakage and ensure reproducibility. I tuned key hyperparameters—including the number of trees, maximum tree depth, and minimum leaf size—using `GridSearchCV`, with F1-score as the evaluation metric. The best-performing model used 300 trees, a maximum tree depth of 10, and a minimum leaf size of 5.

The final model achieves a higher F1-score on the same held-out test set used for the baseline model, indicating improved performance in identifying high-severity outages. This improvement suggests that outage severity is influenced by complex, nonlinear interactions between economic conditions, climate, and outage characteristics—relationships that arise naturally from how infrastructure resilience, environmental stress, and economic capacity jointly shape outage outcomes and are not fully captured by linear models.


---

## **8. Fairness Analysis**

To evaluate whether the final model performs equitably across different economic contexts, I conducted a fairness analysis comparing its performance on outages occurring in low-income versus high-income states. States were grouped using economic quartiles, and the lowest and highest quartiles were compared.

The evaluation metric for this analysis was precision for predicting high-severity outages. Precision is particularly relevant in this setting because incorrectly predicting a severe outage may lead to unnecessary allocation of resources.

A permutation test was used to compare the difference in precision between the two groups. At a significance level of α = 0.05, the resulting p-value indicates that the observed difference in precision is consistent with random variation. As a result, there is insufficient evidence to conclude that the model performs worse for outages in low-income states.


<iframe
  src="assets/fairness_precision_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

---

## **Conclusion**

This project examined how economic conditions relate to power outage severity across U.S. states. While lower-income states tended to experience longer outages on average, these differences were not statistically significant. A predictive model demonstrated strong performance in identifying high-severity outages using information available at the onset of an event, without exhibiting measurable fairness disparities across economic groups. Together, these findings highlight the complexity of outage dynamics and the importance of considering both structural and environmental factors when planning for grid resilience.

