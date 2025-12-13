# Power Outages and Economic Strength in the United States  
### An Analysis of Outage Severity, Economic Indicators, and Structural Vulnerability (2000–2016)

---

## **1. Introduction**
Power outages disrupt communities, strain infrastructure, and slow economic activity across the United States. While extreme weather and equipment failures are well-known triggers, not every state experiences outages with the same severity. This raises an important question: **do states with stronger economies recover from outages more quickly than those with weaker economies?**

This project explores that question by analyzing **1,534 major U.S. power outage events** recorded from **2000 to 2016**. The dataset provides information on outage duration, customers affected, causes, climate regions, and key economic indicators—including *per-capita real gross state product (PC.REALGSP.STATE)*. By combining outage characteristics with economic strength, this project examines why some outages escalate into major disruptions while others remain contained.

Two primary measures define outage severity:

- **Outage Duration** — how long the power remained out  
- **Customers Affected** — the scale of the impact across households and businesses  

### **Guiding Question**

**Do states with lower per-capita economic output experience longer and more severe power outages?**

Investigating this relationship helps reveal how economic conditions, infrastructure investment, and public policy shape the resiliency of the power grid. Understanding these structural patterns is essential for improving disaster response, infrastructure planning, and community preparedness.

---

## **2. Data Cleaning and Exploratory Data Analysis**

Before conducting any analysis, I cleaned the dataset to convert all numerical values stored as strings into proper numeric types, and I parsed all date columns into datetime format. In cases where the dataset did not provide an outage duration, I computed it manually using the start and restoration timestamps. I also created an economic tier variable that groups states into quartiles based on their per-capita real gross state product. These cleaning steps ensure that the analyses and visualizations reflect meaningful relationships in the data.

### Univariate Analysis

The distribution of outage duration shows a heavy right-skew: most outages are relatively short, but a small number last for extremely long periods. This behavior is typical for extreme events.

*(Embed your Plotly histogram iframe here)*

### Bivariate Analysis

To explore the relationship between economic conditions and outage severity, I visualized how outage duration and customers affected vary with state economic output. While the relationship is noisy, higher-income states tend to experience slightly less severe outages.

*(Embed your Plotly scatter or boxplot iframe here)*

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

The missingness of `OUTAGE.DURATION` appears to depend on state-level economic output (`PC.REALGSP.STATE`). A permutation test comparing mean per-capita GSP between outages with missing and non-missing duration values produced a statistically significant result, suggesting that this missingness mechanism is MAR rather than MCAR.

In contrast, the missingness of outage duration does not appear to depend on the month in which the outage occurred. A permutation test comparing months showed no significant difference, providing evidence that missingness is independent of seasonal timing.


---

## **4. Hypothesis Testing**

To evaluate whether economic conditions are associated with outage severity, I conducted two permutation-based hypothesis tests comparing low-GDP and high-GDP states. In both tests, the difference in means was used as the test statistic, and a significance level of α = 0.05 was chosen.

### Outage Duration

The observed difference in mean outage duration between low-GDP and high-GDP states was approximately 387 hours, with outages in lower-GDP states lasting longer on average. However, the permutation test resulted in a p-value of 0.181, indicating that this difference is not statistically significant. Therefore, there is insufficient evidence to conclude that outage duration differs systematically based on state economic output.

<iframe
  src="assets/hypothesis_duration.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Customers Affected

The observed difference in mean customers affected was approximately −12,131, suggesting that outages in higher-GDP states may affect more customers. However, the permutation test yielded a p-value of 0.682, providing strong evidence that such a difference could occur by chance alone. As a result, there is no statistical evidence that customer impact differs by economic tier.

<iframe
  src="assets/hypothesis_customers.html"
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

Model performance is evaluated using **F1-score**, which balances precision and recall. This metric is more appropriate than accuracy because severe outages are relatively rare, and both false positives and false negatives carry meaningful consequences for planning and response.

Only features that would be known at the start of an outage are used for prediction. These include economic indicators, geographic information, climate region, and outage cause. Variables that describe the outcome of the outage itself are intentionally excluded to ensure a realistic and fair prediction setup.


---

## **6. Baseline Model**

The baseline logistic regression model performs surprisingly well, achieving a high F1-score on unseen data. The model is particularly effective at identifying severe outages, with both high precision and recall. This strong performance suggests that key predictors such as outage cause and state-level economic indicators already contain substantial information about outage severity.

However, this model still serves as a baseline because it uses a limited set of features and assumes a linear relationship between predictors and the outcome. In the final model, I explore whether additional feature engineering and more flexible modeling approaches can further improve performance and provide more robust predictions.

---

## **7. Final Model**

---

## **8. Fairness Analysis**

---
