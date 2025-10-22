# show-missing-value-challenge

Team (Anna Yaremko, Marta Samoilenko, Bohdan Pavliuk, Anastasia Pelekh)

### Approach

We analyze missing values at the column level, calculating both counts and percentages for each field. For numeric columns, we check for both NULL and NaN values to ensure complete coverage. The results are visualized using a sorted bar chart with a red gradient color scheme, where darker colors indicate better data quality.
The implementation uses PySpark's columnar operations for efficient distributed processing, then converts aggregated statistics to Pandas for visualization with matplotlib. This balance allows us to handle large-scale data while producing clear, interpretable visualizations.

### Key Visualizations

**Primary Bar Chart**: Shows missing percentages for the top N columns, sorted by severity. Includes both percentage and absolute count views to provide context for decision-making.
**Optional Extensions**: Pattern heatmaps for row-column missing patterns, correlation matrices to identify related missing fields, and data type summaries to understand missingness by field type.

## Limitations

**Scalability**: Converting full Spark DataFrames to Pandas can cause memory issues with very large datasets. Consider sampling for exploratory analysis or pre-aggregating statistics for production monitoring.
**Detection Scope**: Only identifies NULL and NaN values, not placeholder values like -999 or "N/A" strings. Additional profiling may be needed for comprehensive missing value detection.
**Dataset Context**: The Airbnb dataset contains many legitimately optional fields (license, host_about) with naturally high missing rates. These aren't data quality issues but rather reflect the platform's design.

### Design Decisions

**Color Scheme**: Red gradient chosen for intuitive warning association, with reverse ordering so darker shades represent better quality.
**Layout**: 45-degree rotated labels balance readability with space efficiency. Standard figure size (12x6) works well for most displays.
**Persistence**: Results are stored in Delta tables (`monitoring.missing_stats_airbnb`) for historical tracking and dashboard integration.

### Usage Guidelines
Run this analysis immediately upon receiving new data to establish baseline quality metrics. Use the results to set thresholds for acceptable missing rates based on your specific use case. For monitoring, schedule regular runs to detect quality drift over time.
For the Airbnb dataset specifically, focus on columns with unexpected missing patterns rather than known optional fields. Consider subsetting to relevant features if the full dataset visualization becomes cluttered.


## Visuals in dashboards:
![](https://github.com/user-attachments/assets/cace4f63-6aa6-4dbb-9f9b-431fd06cf8e2)


### Implementation
The code provides both a quick analysis function for rapid inspection and a comprehensive analyzer class for detailed reporting. Statistics collection and visualization are separated to allow flexible deployment across different environments.
