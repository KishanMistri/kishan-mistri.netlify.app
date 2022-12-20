---
title: Customer Segmentation Report
date: 2022-09-14T09:37:32.191Z
draft: false
featured: false
image:
  filename: https://researchamericainc.com/_img/segmentation-illustration.png
  focal_point: Smart
  preview_only: false
  caption: customer segmentation banner caption
---
Customer segmentation is a way to identify groups of similar customers. Customers can be segmented on a wide variety of characteristics, such as demographic information, purchase behaviour, and attitudes. This template provides an end-to-end report for processing and segmenting customer purchase data using a K-means clustering algorithm. It also includes a snake plot and heatmap to visualize the resulting clusters and feature importance.

- Multiple numerical variables that you can use for clustering.
- No NaN/NA values. You can use [this to impute missing values](https://app.datacamp.com/workspace/templates/recipe-python-impute-missing-data) if needed.

The dataset consists of customer data, including purchase recency, frequency, and monetary value. Each row represents a different customer with a distinct customer ID.

## 1. Loading packages and Inspecting the Data
The code below imports the packages necessary for data manipulation, visualization, pre-processing, and clustering. It also sets up the visualization style and loads in the data.

Finally, it inspects the data types and missing values with the [`.info()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.info.html) method from `pandas`.


```python
# Load packages
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans

# Set visualization style
sns.set_style("darkgrid")

# Load the data and replace with your CSV file path
df = pd.read_csv("data/customer_data.csv")

# Preview the data
df
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CustomerID</th>
      <th>Recency</th>
      <th>Frequency</th>
      <th>MonetaryValue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12747</td>
      <td>3</td>
      <td>25</td>
      <td>948.70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>12748</td>
      <td>1</td>
      <td>888</td>
      <td>7046.16</td>
    </tr>
    <tr>
      <th>2</th>
      <td>12749</td>
      <td>4</td>
      <td>37</td>
      <td>813.45</td>
    </tr>
    <tr>
      <th>3</th>
      <td>12820</td>
      <td>4</td>
      <td>17</td>
      <td>268.02</td>
    </tr>
    <tr>
      <th>4</th>
      <td>12822</td>
      <td>71</td>
      <td>9</td>
      <td>146.15</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3638</th>
      <td>18280</td>
      <td>278</td>
      <td>2</td>
      <td>38.70</td>
    </tr>
    <tr>
      <th>3639</th>
      <td>18281</td>
      <td>181</td>
      <td>2</td>
      <td>31.80</td>
    </tr>
    <tr>
      <th>3640</th>
      <td>18282</td>
      <td>8</td>
      <td>2</td>
      <td>30.70</td>
    </tr>
    <tr>
      <th>3641</th>
      <td>18283</td>
      <td>4</td>
      <td>152</td>
      <td>432.93</td>
    </tr>
    <tr>
      <th>3642</th>
      <td>18287</td>
      <td>43</td>
      <td>15</td>
      <td>395.76</td>
    </tr>
  </tbody>
</table>
<p>3643 rows × 4 columns</p>
</div>




```python
# Check columns for data types and missing values
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 3643 entries, 0 to 3642
    Data columns (total 4 columns):
     #   Column         Non-Null Count  Dtype  
    ---  ------         --------------  -----  
     0   CustomerID     3643 non-null   int64  
     1   Recency        3643 non-null   int64  
     2   Frequency      3643 non-null   int64  
     3   MonetaryValue  3643 non-null   float64
    dtypes: float64(1), int64(3)
    memory usage: 114.0 KB


## 2. Exploring the Data
Based on the evaluation above, you can select columns you wish to inspect further. In this template, three columns are selected from the four columns. CustomerID is omitted because it is an identifier and not useful for clustering.

The code below reduces the DataFrame to the columns you wish to cluster on and then prints descriptive statistics using the [`describe()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.describe.html) method from `pandas`.

Printing descriptive statistics is helpful because K-means clustering has several key assumptions that can be revealed via this exploration:
1. There is no skewness to the data.
2. The variables have the same average values.
3. The variables have the same variance.

If you'd like to learn more about pre-processing data for K-means clustering, you can refer to this [video](https://campus.datacamp.com/courses/customer-segmentation-in-python/data-pre-processing-for-clustering?ex=1) from the course Customer Segmentation in Python.


```python
# Select columns for clustering
columns_for_clustering = ["Recency", "Frequency", "MonetaryValue"]

# Create new DataFrame with clustering variables
df_features = df[columns_for_clustering]

# Print a summary of descriptive statistics
df_features.describe()
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Recency</th>
      <th>Frequency</th>
      <th>MonetaryValue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>3643.00000</td>
      <td>3643.000000</td>
      <td>3643.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>90.43563</td>
      <td>18.714247</td>
      <td>370.694387</td>
    </tr>
    <tr>
      <th>std</th>
      <td>94.44651</td>
      <td>43.754468</td>
      <td>1347.443451</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.00000</td>
      <td>1.000000</td>
      <td>0.650000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>19.00000</td>
      <td>4.000000</td>
      <td>58.705000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>51.00000</td>
      <td>9.000000</td>
      <td>136.370000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>139.00000</td>
      <td>21.000000</td>
      <td>334.350000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>365.00000</td>
      <td>1497.000000</td>
      <td>48060.350000</td>
    </tr>
  </tbody>
</table>
</div>



The [`facetgrid()`](https://seaborn.pydata.org/generated/seaborn.FacetGrid.html) function from `seaborn` creates a grid of histograms of the data to be clustered.  It serves as a further exploration of the data to determine its skew and whether it needs transformation.


```python
# Plot the distributions of the selected variables
g = sns.FacetGrid(
    df_features.melt(),  # Reformat the DataFrame for plotting purposes
    col="variable",  # Split on the 'variable' column created by reformating
    sharey=False,  # Turn off shared y-axis
    sharex=False,  # Turn off shared x-axis
)
# Apply a histogram to the facet grid
g.map(sns.histplot, "value")
# Adjust the top of the plots to make room for the title
g.fig.subplots_adjust(top=0.8)
# Create a title
g.fig.suptitle("Unprocessed Variable Distributions", fontsize=16)
plt.show()
```
    
![png](https://drive.google.com/file/d/1zciKFKwN3pi_Z_1SRDQlk6saDitI3PIk/view?usp=share_link)

Before proceeding, it is crucial to ensure that all columns selected for clustering are numeric. The following code iterates through the reduced DataFrame and checks whether each column is numeric. If it returns `True`, then you can proceed with the pre-processing.


```python
all([pd.api.types.is_numeric_dtype(df_features[col]) for col in columns_for_clustering])
```
    True

## 3. Pre-processing the Data
Based on the grids above, if there is a skew, you will have to complete this step which removes the skew and center the variables. This is the case for the placeholder dataset used in this template and will likely be the case for your data.
- First, a log transformation is applied to the data using the `numpy` [`log()`](https://numpy.org/doc/stable/reference/generated/numpy.log.html) function. A log transformation unskews the data in preparation for clustering.
- Next, the `StandardScaler()` from [`sklearn.preprocessing`](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html) fits and transforms the log-transformed data. This centers and scales the data in further preparation for clustering.
- Finally, a new DataFrame is created and visualized again to confirm the results.


```python
# Perform a log transformation of the data to unskew the data
df_log = np.log(df_features)

# Initialize a standard scaler and fit it
scaler = StandardScaler()
scaler.fit(df_log)

# Scale and center the data
df_normalized = scaler.transform(df_log)

# Create a pandas DataFrame of the processed data
df_processed = pd.DataFrame(
    data=df_normalized, index=df_features.index, columns=df_features.columns
)

# Plot the distributions of the selected variables
g = sns.FacetGrid(df_processed.melt(), col="variable")
g.map(sns.histplot, "value")
g.fig.subplots_adjust(top=0.8)
g.fig.suptitle("Preprocessed Variable Distributions", fontsize=16)
plt.show()
```
![png](https://drive.google.com/file/d/1MS3tR72e8Djg6DmNA27lng6mLo7YfNWd/view?usp=share_link)
    


## 4. Choosing the Number of Clusters

The next step is to fit a variable number of clusters and plot each cluster's sum-of-squared errors (SSE). The SSE reflects the sum of squared distances from every data point to the cluster center. The aim is to reduce the SSE while still maintaining a reasonable number of clusters. 

By plotting the SSE for each number of clusters, you can identify at what point there are diminishing returns by adding new clusters. This type of plot is called an elbow plot.

In the code below, you can set the maximum number of clusters you want to plot, and then a loop is used to generate the SSE for each number of clusters. Finally, the `seaborn` function [`pointplot()`](https://seaborn.pydata.org/generated/seaborn.pointplot.html) plots a curve with each cluster number and SSE. This allows you to identify the 'elbow' or point where there are only marginal reductions for each additional cluster.


```python
# Set the maximum number of clusters to plot
max_clusters = 10

# Initialize empty dictionary to store sum of squared errors
sse = {}

# Fit KMeans and calculate SSE for each k
for k in range(1, max_clusters):
    # Initialize KMeans with k clusters
    kmeans = KMeans(n_clusters=k, random_state=1)
    # Fit KMeans on the normalized dataset
    kmeans.fit(df_processed)
    # Assign sum of squared distances to k element of dictionary
    sse[k] = kmeans.inertia_

# Initialize a figure of set size
plt.figure(figsize=(10, 4))

# Create an elbow plot of SSE values for each key in the dictionary
sns.pointplot(x=list(sse.keys()), y=list(sse.values()))

# Add labels to the plot
plt.title("Elbow Method Plot", fontsize=16)  # Add a title to the plot
plt.xlabel("Number of Clusters")  # Add x-axis label
plt.ylabel("SSE")  # Add y-axis label

# Show the plot
plt.show()
```
    
![png](https://drive.google.com/file/d/1MS3tR72e8Djg6DmNA27lng6mLo7YfNWd/view?usp=share_link)

## 5. Clustering the Data
You can now select an optimal number of clusters based on the elbow plot above by setting `k`. In this example, `k` is set to 3.

[`KMeans()`](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html) from `sklearn.cluster` with `k` clusters is then fit to the processed data, and the cluster labels are extracted and assigned back to the original data. This allows you to inspect raw data by cluster in later steps.


```python
# Choose number of clusters
k = 3

# Initialize KMeans
kmeans = KMeans(n_clusters=k, random_state=1) 

# Fit k-means clustering on the normalized data set
kmeans.fit(df_processed)

# Extract cluster labels
cluster_labels = kmeans.labels_

# Create a new DataFrame by adding a new cluster column to the original data
df_clustered = df.assign(Cluster=cluster_labels)

# Preview the clustered DataFrame
df_clustered
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CustomerID</th>
      <th>Recency</th>
      <th>Frequency</th>
      <th>MonetaryValue</th>
      <th>Cluster</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>12747</td>
      <td>3</td>
      <td>25</td>
      <td>948.70</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>12748</td>
      <td>1</td>
      <td>888</td>
      <td>7046.16</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>12749</td>
      <td>4</td>
      <td>37</td>
      <td>813.45</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>12820</td>
      <td>4</td>
      <td>17</td>
      <td>268.02</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>12822</td>
      <td>71</td>
      <td>9</td>
      <td>146.15</td>
      <td>2</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>3638</th>
      <td>18280</td>
      <td>278</td>
      <td>2</td>
      <td>38.70</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3639</th>
      <td>18281</td>
      <td>181</td>
      <td>2</td>
      <td>31.80</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3640</th>
      <td>18282</td>
      <td>8</td>
      <td>2</td>
      <td>30.70</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3641</th>
      <td>18283</td>
      <td>4</td>
      <td>152</td>
      <td>432.93</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3642</th>
      <td>18287</td>
      <td>43</td>
      <td>15</td>
      <td>395.76</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>3643 rows × 5 columns</p>
</div>

## 6. Inspecting the Clusters
### 6a. Visualizing the Raw Values by Cluster
The next step is to analyze the unprocessed data by cluster. The `pandas` method [`DataFrame.groupby()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.groupby.html), combined with the [`.size()`](https://pandas.pydata.org/docs/reference/api/pandas.core.groupby.GroupBy.size.html) method, returns the total number of rows per `Cluster`.


```python
# Group the data by cluster and calculate the total number of rows per group
df_sizes = df_clustered.groupby(["Cluster"], as_index=False).size()

# Inspect the row counts
df_sizes
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Cluster</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>901</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1156</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>1586</td>
    </tr>
  </tbody>
</table>
</div>



Next, the mean values per cluster are visualized. The data is grouped again, and this time, the `pandas` method [`.mean()`](https://pandas.pydata.org/docs/reference/api/pandas.core.groupby.GroupBy.mean.html) is used to aggregate the data by cluster and calculate the mean for each variable. Alternatively, the [`.agg()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.agg.html) method can also be used to specify specific aggregations for different columns if necessary. Consult the [documentation](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.agg.html) for further information on the types of aggregations possible.

The `seaborn` [`catplot()`](https://seaborn.pydata.org/generated/seaborn.catplot.html#seaborn.catplot) function visualizes the means per cluster.


```python
# Calculate the mean of feature columns by cluster
df_means = df_clustered.groupby(["Cluster"])[df_features.columns].mean().reset_index()

# Plot the distributions of the selected variables
sns.catplot(
    data=df_means.melt(id_vars="Cluster"),  # Transform the data to enable plotting
    col="variable",
    x="Cluster",
    y="value",
    kind="bar",
)

# Add a title
plt.suptitle("Average Values by Cluster", y=1.04, fontsize=16)

# Show the plot
plt.show()
```

![png](https://drive.google.com/file/d/1Z8qatvpcpdYjqoKGm9gTSmigXtBs0dG8/view?usp=share_link)

### 6b. Create a Snake Plot of the Clusters
The next step takes the processed data and visualizes the differences between the clusters using a snake plot. This can be helpful in spotting trends or key differences that would not be visible with the raw data.


```python
# Assign cluster labels to processed DataFrame
df_processed_clustered = df_processed.assign(Cluster=cluster_labels)

# Melt the normalized DataFrame and reset the index
df_processed_melt = pd.melt(
    df_processed_clustered.reset_index(),
    # Assign the cluster labels as the ID
    id_vars=['Cluster'],
    # Assign clustering variables as values
    value_vars=df_features.columns,
    # Name the variable and value
    var_name="Metric",
    value_name="Value",
)

# Change the figure size
plt.figure(figsize=(10, 6))

# Add label and titles to the plot
plt.title('Snake Plot of Normalized Variables', fontsize=16)
plt.xlabel('Metric')
plt.ylabel('Average Normalized Value')

# Plot a line for each value of the cluster variable
sns.lineplot(data=df_processed_melt, x='Metric', y='Value', hue='Cluster')
plt.show()
```

![png](https://drive.google.com/file/d/179i9HqhIxJhpj63yJg25ccJKIbjTd5U8/view?usp=share_link)
    

### 6c. Create a Heatmap of Relative Importance
Another technique to help visualize how each segment is distinct is to plot the relative importance. The code below achieves this by doing the following:
- First, it calculates the average values for each cluster.
- Next, it calculates the average values for the total population.
- It then divides the cluster averages by the population averages and subtracts one.

This provides a relative importance score for each of the different features used for clustering. The `seaborn` [`heatmap()`](https://seaborn.pydata.org/generated/seaborn.heatmap.html) function plots these relative importances on a red-to-blue colour scale to help visualize the relative importance of each attribute to the segments.


```python
# Calculate average RFM values for each cluster
cluster_avg = df_clustered.groupby(["Cluster"])[columns_for_clustering].mean()

# Calculate average RFM values for the total customer population
population_avg = df[columns_for_clustering].mean()

# Calculate relative importance of cluster's attribute value compared to the population
relative_imp = cluster_avg / population_avg - 1

# Change the figure size
plt.figure(figsize=(8, 4))

# Add the plot title
plt.title("Relative importance of attributes", fontsize=16)

# Plot the heatmap
sns.heatmap(data=relative_imp, annot=True, fmt=".2f", cmap=sns.diverging_palette(20, 220, as_cmap=True))
plt.show()
```
    
![png](https://drive.google.com/file/d/1IJVsSJ0cBQXv7sKHBdAcTjdiaTX09cqh/view?usp=share_link)


This concludes the report! Hope you find it enjoying & insightful.