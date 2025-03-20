# Hotel Booking Prediction Project: Expedia

## Overview
This project analyzes hotel booking data to predict which hotel cluster a user is likely to book based on various features. Using exploratory data analysis and visualization techniques, I identified key patterns and relationships within the booking data that could help predict user preferences. 

## Data Exploration

### 1. Initial Correlation Analysis

![Correlation Heatmap](https://github.com/OlidiaTL/Expedia/blob/main/correlation_matrix.png?raw=true)

#### Key Finding 1: No strong linear correlations with target variable
- The correlation analysis revealed no significant linear relationships between hotel_cluster (our target variable) and any other features in the dataset.
- This suggests that linear models alone might not be sufficient to capture the relationships in this data.

#### Key Finding 2: Strong correlations between features
- Strong negative correlation (-0.767) between posa_continent and site_name 
- This indicates that specific point-of-sale sites are strongly associated with particular continents
- This relationship makes intuitive sense as users often use region-specific Expedia sites

#### Key Finding 3: Moderate correlation between room and adult counts
- Moderate positive correlation (0.521) between srch_rm_cnt (number of rooms) and srch_adults_cnt (number of adults) 
- This is expected as larger groups typically require more rooms
 
Since the target variable doesn't show strong linear correlations with individual features, we should explore non-linear relationships

### 2. Continent and Site Relationship Analysis

![Relationship Between posa_continent and site_name](https://github.com/OlidiaTL/Expedia/blob/main/continent_sitename.png?raw=true)

#### Key Finding: Clear geographical site specialization
The heatmap reveals a strong pattern of geographical specialization among Expedia's sites:
- posa_continent = 3 shows a very strong association with site_name = 2, indicating that this point of sale is heavily dominant for a specific continent (likely North America based on Expedia's business structure)
- posa_continent = 1 shows moderate associations with multiple site_names, but has notable presence with site_names 13 and 37
- There is very minimal overlap between sites serving different continents, confirming the strong negative correlation (-0.767) found earlier

#### Statistical validation:
- p-value of 0.0: This extremely low p-value indicates that the relationship between posa_continent and site_name is statistically significant.
- Cramer's V of 0.9998: This measure of association ranges from 0 (no association) to 1 (perfect association). The value of 0.9998 indicates an almost perfect association between posa_continent and site_name.

#### Implications for modeling:
1. These two features provide redundant information, suggesting we could potentially drop one without losing predictive power
2. The clear separation suggests regional preferences may be important for predicting hotel clusters

### 3. Relationship Between Room and Adult Counts

![Relationship Between Number of Adults and Number of Rooms](https://github.com/OlidiaTL/Expedia/blob/main/srchrm_srchadults.png?raw=true)
![Distribution of Adults per Room](https://github.com/OlidiaTL/Expedia/blob/main/adultscnt_srchrmcnt.png?raw=true)

#### Correlation Analysis:
To verify the correlation between the number of adults and number of rooms booked, I conducted both visual and statistical analyses:

- The scatter plot shows a clear positive relationship between the number of adults and rooms
- The histogram of adults per room shows that most bookings have 1-2 adults per room, with the highest frequency around 2 adults per room

- **Pearson correlation of 0.5206**: This indicates a moderate positive linear relationship between number of adults and number of rooms.
- **Spearman correlation of 0.4415**: The slightly lower Spearman coefficient suggests that the relationship has some non-linear components or contains outliers.
- **p-values of 0.0000000000**: The extremely low p-values for both tests confirm that the correlation is statistically significant and not due to random chance.
- **R² of 0.2710**: This means that 27.1% of the variance in one variable is explained by the other. While significant, this also indicates that the variables contain substantial unique information not captured by the other.

#### Interpretation:
These statistical results confirm our visual observations - there is a moderate but highly significant positive relationship between the number of adults and rooms. The R² value suggests that while these variables are related, they each still contain unique information that could be valuable for predicting hotel_cluster.

#### Feature Engineering Implications:
Based on this analysis, I created an 'adults_per_room' feature that could potentially capture booking patterns more effectively than the individual features alone.

### 4. Hotel Cluster Analysis

#### Hotel Clusters by Continent
![Hotel Cluster Distribution by Continent](https://github.com/OlidiaTL/Expedia/blob/main/continent_cluster.png?raw=true)

The heatmap shows a non-uniform distribution of hotel clusters across different continents:
- Certain hotel clusters (e.g., 3, 57, 66) show stronger representation in posa_continent 0
- Some clusters (e.g., 9, 45, 63) appear more strongly associated with posa_continent 4

#### Statistical validation:
- The chi-square test confirms a statistically significant association between hotel_cluster and posa_continent
- Cramer's V of 0.1907 indicates a medium effect size, suggesting a moderate but meaningful relationship

#### Hotel Clusters by Top Sites
![Hotel Cluster Distribution by Top Sites](https://github.com/OlidiaTL/Expedia/blob/main/sitename_cluster.png?raw=true)

The heatmap reveals varied distribution patterns:
- Certain site_names (e.g., 18, 23, 24) show strong associations with specific hotel clusters
- Site_name 18 shows notable associations with clusters 12, 36, and 62
- Site_name 23 shows stronger associations with clusters 30 and 82

#### Statistical validation:
- The chi-square test confirms a statistically significant association
- Cramer's V of 0.0876 indicates a small effect size, suggesting a weaker relationship than with posa_continent

#### Key insights:
1. Despite not showing strong linear correlations in the initial analysis, there are significant non-linear relationships between geographical features and hotel clusters
2. The point of sale continent appears to be a stronger predictor of hotel cluster than the specific site name
3. These patterns suggest clear regional preferences for certain types of hotel destinations

### 5. Room and Adult Count Analysis

![Hotel Cluster Distribution by Room Count](https://github.com/OlidiaTL/Expedia/blob/main/roomcnt_cluster.png?raw=true)

The room count heatmap reveals distinct patterns across hotel clusters:

- **Higher room counts (4+)**: Clusters 16, 48, 81, and 91 show notably stronger associations with bookings of 4+ rooms, suggesting these clusters likely represent destinations popular for larger groups or families.

- **Single room preferences**: Clusters 41, 48 and 91 show a strong association with single-room bookings, indicating these may represent destinations popular with solo travelers or couples.

- **Mid-size accommodations**: Clusters 12, 40, 42,91 and 98 demonstrate stronger associations with 3-room bookings, suggesting they may represent destinations appealing to medium-sized groups.

- **Balanced distributions**: Many clusters (particularly in the 20-40 range) show a more even distribution across different room counts, suggesting these represent more versatile destinations that accommodate various group sizes.

![Hotel Cluster Distribution by Adult Count](https://github.com/OlidiaTL/Expedia/blob/main/adultcnt_cluster.png?raw=true)

Analyzing the adult count heatmap reveals interesting patterns:

- **Large group bookings (6-10 adults)**: Clusters 12, 65, 85, and 91 show notably stronger preferences for large group bookings. This suggests these clusters likely represent destinations that cater to conferences, retreats, or large family gatherings.

- **Solo traveler patterns**: Clusters 48 and 91 display heightened activity for single-adult bookings, indicating these may be popular business travel destinations or locations appealing to solo tourists.

- **Couple and small family bookings**: Many clusters show stronger associations with the 2-3 adult range, which is expected as this represents the typical family or couple travel arrangement. Clusters 18, 21, and 72 are particularly noteworthy here.

- **Consistent large family preference**: Cluster 91 shows strong association across multiple adult count categories, suggesting it might represent versatile destinations that accommodate various group sizes, from solo travelers to large groups.

#### Statistical Analysis:
1. **Significant variation in mean room counts**: The ANOVA test demonstrates that the average number of rooms booked varies significantly across different hotel clusters (p-value: 2.56e-49). This indicates that certain hotel clusters consistently attract bookings with higher or lower room counts than others.
2. **Even stronger variation in mean adult counts**: The even lower p-value for adult counts (1.42e-121) and higher F-statistic (8.717) shows that the number of adults per booking is an even stronger differentiator between hotel clusters than room count.
3. **Non-linear relationships**: These ANOVA results confirm that while our initial correlation analysis didn't show strong linear relationships, there are significant patterns in how these features relate to hotel clusters.

#### Implications for modeling:
1. These features should be included in predictive models despite not showing strong linear correlations with hotel_cluster in our initial analysis.

### 6. Temporal Patterns

![Monthly Booking Patterns](https://github.com/OlidiaTL/Expedia/blob/main/cluster_month.png?raw=true)

#### Time-Based Feature Engineering:
We created several time-based features from the check-in (srch_ci) and check-out (srch_co) dates:
- trip_duration: Number of days between check-in and check-out
- search_month: Month of the check-in date
- search_day_of_week: Day of week for check-in (0=Monday to 6=Sunday)
- is_weekend: Binary indicator if check-in falls on weekend (Saturday or Sunday)

#### Monthly Distribution Patterns:
The heatmap reveals several significant temporal patterns:
1. **Seasonal Preferences by Cluster**: 
   - Certain months show stronger preferences for specific hotel clusters
   - Month 9 (September) shows particularly strong preferences for clusters 27, 52 and 74 (bright yellow spots)
   - Month 8 (August) shows elevated booking rates across many clusters
   - Month 12 (December) shows increased activity across most clusters
2. **Year-Round vs. Seasonal Clusters**: 
   - Some hotel clusters maintain relatively consistent booking patterns throughout the year
   - Others show strong seasonal variation, with clear high and low seasons
3. **Off-Season Patterns**: 
   - Certain clusters that are popular in summer months show reduced activity in winter months
   - This suggests some clusters may represent seasonal destinations (e.g., beach resorts, ski resorts)

#### Key Insights:
1. The temporal booking patterns differ significantly across hotel clusters, suggesting that time-of-year is an important predictor of hotel cluster preference.
2. The seasonality patterns likely reflect different types of destinations - some being year-round destinations while others are highly seasonal.
3. December (month 12) shows higher activity across most clusters, likely reflecting holiday travel patterns.

### 7. Non-linear Relationships with Hotel Cluster

To investigate non-linear relationships between numerical features and hotel clusters, I analyzed the average values and distributions of key numerical features across different hotel clusters.

![Numerical Features Mean by Hotel Cluster](https://github.com/OlidiaTL/Expedia/blob/main/orig_cluster.png?raw=true)

#### Analysis of Average Values Across Hotel Clusters

The bar charts reveal several important patterns:

1. **Origin-Destination Distance**: 
   - Substantial variation in average distances exists across hotel clusters
   - Several clusters (8, 46, 67, and 70) show distinctly higher average distances (>4000 units)
   - Cluster 46 has the highest average distance at over 5000 units
   - Other clusters (like 18, 45, 50, and 94) consistently show shorter average distances (<1500 units)
   - This suggests certain hotel clusters represent long-distance destinations while others are more local/regional

2. **Adult Count Patterns**: 
   - The adult count shows subtle but consistent differences across clusters
   - Most clusters have average adult counts between 1.8-2.2
   - Cluster 71 stands out with the highest average adult count (around 2.4)
   - Clusters 62-74 generally show higher adult counts compared to other ranges

3. **Children Count Variation**:
   - More pronounced differences appear in children counts across clusters
   - Cluster 71 shows a dramatic spike with an average of over 0.8 children per booking
   - Clusters 30-35 also show elevated children counts
   - This suggests certain clusters specifically cater to family travel with children

4. **Room Count Consistency**:
   - Room counts show less dramatic variation but still have cluster-specific patterns
   - Most clusters maintain averages between 1.0-1.2 rooms

5. **Click Count (cnt) Feature**:
   - Cluster 66 shows the highest average click count at over 2.0
   - Clusters 45, and 46 also show elevated click counts
   - This could indicate destinations that require more research before booking

#### Distribution Analysis (Box Plots)

![Numerical Features Distribution by Hotel Cluster](https://github.com/OlidiaTL/Expedia/blob/main/cluster_orig2.png?raw=true)

The box plots provide additional insights beyond averages:

1. **High Variability in Distance**:
   - The origin-destination distance shows extreme variability within clusters
   - Many outliers (circles) indicate long-distance bookings within otherwise local-focused clusters
   - Clusters in the 55-70 range show both higher medians and wider distributions
   - Some clusters like 40-50 have primarily short distances with occasional long-distance outliers

2. **Discrete Nature of Booking Features**:
   - Adult, children, and room counts show distinct "banding" patterns due to their discrete nature
   - For adult counts, the majority of bookings across all clusters center around 2 adults
   - Children count distributions show most bookings have 0 children, with occasional outliers
   - Room counts similarly cluster around 1 room across most hotel clusters

3. **Click Count Outliers**:
   - The cnt feature shows considerable outliers across all clusters
   - Some bookings require 15-20+ clicks, while the majority need fewer than 5
   - This suggests a small subset of users who conduct extensive research before booking

#### Key Insights:
1. Different hotel clusters capture meaningful patterns in traveler behavior and preferences that aren't apparent from simple correlation analysis
2. Distance is the strongest differentiator between clusters, with clear patterns of regional versus long-distance destinations
3. The existence of significant outliers in all features suggests that while clusters have general tendencies, they accommodate diverse booking patterns
4. These numerical features show clear non-linear relationships with hotel clusters that would be valuable in predictive models

This analysis reinforces that despite weak linear correlations, numerical features contain valuable predictive information when examined through the lens of non-linear relationships and cluster-specific patterns.

### 8. Categorical Feature Analysis

#### Chi-Square Test Results:
The chi-square tests demonstrate statistically significant relationships between all categorical features and hotel_cluster:

#### Hierarchy of Predictive Power: 
- Hotel location features (market, country, continent) are the strongest predictors
- User location features (country, region, city) form the next strongest group
- Booking characteristics (site_name, posa_continent, is_package) show moderate association
- Device and channel characteristics (is_mobile, channel) show the weakest, though still significant, relationships

### 9. Feature Importance Analysis

![Decision Tree for Hotel Cluster Prediction](https://github.com/OlidiaTL/Expedia/blob/main/decision_tree.png?raw=true)

#### Key findings:
1. **Top Predictors**: 
   - posa_continent emerges as the most important feature (32.6%), confirming our earlier hypothesis about the importance of geographical origin
   - srch_adults_cnt is the second most important feature (25.1%), confirming the value of booking size information
   - is_package shows high importance (22.0%), validating our earlier observation about the distinct booking patterns for package vs. non-package trips
2. **Moderate Predictors**: 
   - user_location_country (9.9%) provides additional geographical value
   - site_name (4.9%) offers some predictive power, though less than its correlated feature posa_continent
3. **Weaker Predictors**: 
   - channel, srch_children_cnt, and srch_rm_cnt show limited importance
   - is_mobile and cnt show no meaningful importance in this tree configuration

#### Interpretation:
1. The decision tree model confirms that geographical features (particularly point of sale continent) are the strongest predictors of hotel cluster, which aligns with our chi-square analysis.
2. The high importance of srch_adults_cnt validates our ANOVA findings that different hotel clusters show significantly different patterns in adult count distributions.
3. The importance of is_package confirms our observation from the categorical analysis that package vs. non-package bookings show distinctly different hotel cluster preferences.

## Methodology
Based on the exploratory data analysis, I determined that tree-based ensemble methods would be most appropriate for this prediction task given the non-linear relationships identified. The model development process included:

1. Feature selection based on statistical significance and feature importance scores
2. Feature engineering, particularly for time-based and geographical features
3. Model training using gradient boosting techniques
4. Hyperparameter tuning to optimize prediction accuracy

## Results
The final model achieved strong predictive performance, with geographic features and booking characteristics providing the most predictive power. The top 5 features (posa_continent, srch_adults_cnt, is_package, user_location_country, and site_name) together provided the majority of the model's predictive capability.

## Conclusion
This analysis demonstrates the importance of exploring non-linear relationships and feature interactions in predictive modeling. While initial correlation analysis showed limited linear relationships with the target variable, deeper statistical analysis revealed significant patterns that could be leveraged for prediction.

Key findings:
1. Geographic features (where users are booking from and where they're going) are the strongest predictors of hotel cluster preferences
2. Booking characteristics (package status, number of adults) provide substantial additional predictive power
3. Time-based patterns, particularly seasonality, show significant associations with hotel clusters
4. Tree-based models are well-suited to capture the complex, non-linear relationships in this data

This project provides valuable insights for travel booking platforms looking to improve their recommendation systems by better understanding the factors that influence user preferences for different types of hotel destinations.

---

## How to View the Complete Analysis

The full Jupyter notebook with detailed code and additional visualizations is available:

[View this analysis in nbviewer](https://nbviewer.org/github/OlidiaTL/Expedia/blob/main/Expedia.ipynb)

## Image Credits

All visualizations were created as part of this analysis using Python's data visualization libraries including Matplotlib, Seaborn, and Plotly.
