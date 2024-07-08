---
title: Handling Missing Data
author: "Juma Shafara"
date: "2024-04-24"
categories: [Data Analysis]
---

## Handling Missing Data

![Photo by DATAIDEA](thumbnail.png)

## Introduction:

Missing data is a common hurdle in data analysis, impacting the reliability of insights drawn from datasets. Python offers a range of solutions to address this issue, some of which we discussed in the earlier weeks. In this notebook, we look into the top three missing data imputation methods in Python—SimpleImputer, KNNImputer, and IterativeImputer from scikit-learn—providing insights into their functionalities and practical considerations. We'll explore these essential techniques, using the weather dataset.

```
# install the libraries for this demonstration
! pip install dataidea==0.2.5
```

```python
from dataidea.packages import *
from dataidea.datasets import loadDataset
```

`from dataidea.packages import *` imports for us np, pd, plt, etc. `loadDataset` allows us to load datasets inbuilt in the dataidea library

```python
weather = loadDataset('weather')
```

```python
weather
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
      <th>day</th>
      <th>temperature</th>
      <th>windspead</th>
      <th>event</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>01/01/2017</td>
      <td>32.0</td>
      <td>6.0</td>
      <td>Rain</td>
    </tr>
    <tr>
      <th>1</th>
      <td>04/01/2017</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>Sunny</td>
    </tr>
    <tr>
      <th>2</th>
      <td>05/01/2017</td>
      <td>28.0</td>
      <td>NaN</td>
      <td>Snow</td>
    </tr>
    <tr>
      <th>3</th>
      <td>06/01/2017</td>
      <td>NaN</td>
      <td>7.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>07/01/2017</td>
      <td>32.0</td>
      <td>NaN</td>
      <td>Rain</td>
    </tr>
    <tr>
      <th>5</th>
      <td>08/01/2017</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Sunny</td>
    </tr>
    <tr>
      <th>6</th>
      <td>09/01/2017</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7</th>
      <td>10/01/2017</td>
      <td>34.0</td>
      <td>8.0</td>
      <td>Cloudy</td>
    </tr>
    <tr>
      <th>8</th>
      <td>11/01/2017</td>
      <td>40.0</td>
      <td>12.0</td>
      <td>Sunny</td>
    </tr>
  </tbody>
</table>
</div>

```python
weather.isna().sum()
```

    day            0
    temperature    4
    windspead      4
    event          2
    dtype: int64

Let's demonstrate how to use the top three missing data imputation methods—SimpleImputer, KNNImputer, and IterativeImputer—using the simple weather dataset.

```python
# select age from the data
temp_wind = weather[['temperature', 'windspead']].copy()
```

```python
temp_wind_imputed = temp_wind.copy()
```

## SimpleImputer from scikit-learn:

- **Usage**: SimpleImputer is a straightforward method for imputing missing values by replacing them with a constant, mean, median, or most frequent value along each column.
- **Pros**:
  - Easy to use and understand.
  - Can handle both numerical and categorical data.
  - Offers flexibility with different imputation strategies.
- **Cons**:
  - It doesn't consider relationships between features.
  - May not be the best choice for datasets with complex patterns of missingness.
- **Example**:

```python
from sklearn.impute import SimpleImputer

simple_imputer = SimpleImputer(strategy='mean')
temp_wind_simple_imputed = simple_imputer.fit_transform(temp_wind)

temp_wind_simple_imputed_df = pd.DataFrame(temp_wind_simple_imputed, columns=temp_wind.columns)
```

Let's have a look at the outcome

```python
temp_wind_simple_imputed_df
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
      <th>temperature</th>
      <th>windspead</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>32.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>33.2</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>28.0</td>
      <td>8.4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>33.2</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>32.0</td>
      <td>8.4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>33.2</td>
      <td>8.4</td>
    </tr>
    <tr>
      <th>6</th>
      <td>33.2</td>
      <td>8.4</td>
    </tr>
    <tr>
      <th>7</th>
      <td>34.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>40.0</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>

## KNNImputer from scikit-learn:

- **Usage**: KNNImputer imputes missing values using k-nearest neighbors, replacing them with the mean value of the nearest neighbors.
- **Pros**:
  - Considers relationships between features, making it suitable for datasets with complex patterns of missingness.
  - Can handle both numerical and categorical data.
- **Cons**:
  - Computationally expensive for large datasets.
  - Requires careful selection of the number of neighbors (k).
- **Example**:

```python
from sklearn.impute import KNNImputer

knn_imputer = KNNImputer(n_neighbors=2)
temp_wind_knn_imputed = knn_imputer.fit_transform(temp_wind)

temp_wind_knn_imputed_df = pd.DataFrame(temp_wind_knn_imputed, columns=temp_wind.columns)
```

If we take a look at the outcome

```python
temp_wind_knn_imputed_df
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
      <th>temperature</th>
      <th>windspead</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>32.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>33.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>28.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>33.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>32.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>33.2</td>
      <td>8.4</td>
    </tr>
    <tr>
      <th>6</th>
      <td>33.2</td>
      <td>8.4</td>
    </tr>
    <tr>
      <th>7</th>
      <td>34.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>40.0</td>
      <td>12.0</td>
    </tr>
  </tbody>
</table>
</div>

## IterativeImputer from scikit-learn:

- **Usage**: IterativeImputer models each feature with missing values as a function of other features and uses that estimate for imputation. It iteratively estimates the missing values.
- **Pros**:
  - Takes into account relationships between features, making it suitable for datasets with complex missing patterns.
  - More robust than SimpleImputer for handling missing data.
- **Cons**:
  - Can be computationally intensive and slower than SimpleImputer.
  - Requires careful tuning of model parameters.
- **Example**:

```python
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer

iterative_imputer = IterativeImputer()
temp_wind_iterative_imputed = iterative_imputer.fit_transform(temp_wind)

temp_wind_iterative_imputed_df = pd.DataFrame(temp_wind_iterative_imputed, columns=temp_wind.columns)
```

Let's take a look at the outcome

```python
temp_wind_iterative_imputed_df
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
      <th>temperature</th>
      <th>windspead</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>32.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>35.773287</td>
      <td>9.000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>28.000000</td>
      <td>3.321648</td>
    </tr>
    <tr>
      <th>3</th>
      <td>33.042537</td>
      <td>7.000000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>32.000000</td>
      <td>6.238915</td>
    </tr>
    <tr>
      <th>5</th>
      <td>33.545118</td>
      <td>7.365795</td>
    </tr>
    <tr>
      <th>6</th>
      <td>33.545118</td>
      <td>7.365795</td>
    </tr>
    <tr>
      <th>7</th>
      <td>34.000000</td>
      <td>8.000000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>40.000000</td>
      <td>12.000000</td>
    </tr>
  </tbody>
</table>
</div>

## Datawig:

Datawig is a library specifically designed for imputing missing values in tabular data using deep learning models.

```python
# import datawig

# # Impute missing values
# df_imputed = datawig.SimpleImputer.complete(weather)
```

These top imputation methods offer different trade-offs in terms of computational complexity, handling of missing data patterns, and ease of use. The choice between them depends on the specific characteristics of the dataset and the requirements of the analysis.

## Homework

- Try out these techniques for categorical data

## Credit

#### Do you seriously want to learn Programming and Data Analysis with Python?

If you’re serious about learning Programming, Data Analysis with Python and getting prepared for Data Science roles, I highly encourage you to enroll in my Programming for Data Science Course, which I've taught to hundreds of students. Don’t waste your time following disconnected, outdated tutorials

My Complete Programming for Data Science Course has everything you need in one place.

The course offers:

- Duration: Usually 3-4 months
- Sessions: Four times a week (one on one)
- Location: Online or/and at UMF House, Sir Apollo Kagwa Road

What you'l learn:

- Fundamentals of programming
- Data manipulation and analysis
- Visualization techniques
- Introduction to machine learning
- Database Management with SQL (optional)
- Web Development with Django (optional)

Best

Juma Shafara

Data Scientist, Instructor

jumashafara0@gmail.com / dataideaorg@gmail.com

+256701520768 / +256771754118

<!-- Insert AdSense script dynamically -->
<script>
    (function() {
        var adScript = document.createElement('script');
        adScript.src = 'https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-8076040302380238';
        adScript.async = true;
        adScript.crossorigin="anonymous"
        document.head.appendChild(adScript);
    })();
</script>

<div>
<h2>You may also like:</h2>
<a href="/posts/bffill_and_ffill/">
<h4>Handling Missing Data in Pandas, When to Use `bfill` and `ffill` Methods</h4>
![Handling Missing Data in Pandas, When to Use `bfill` and `ffill` Methods](../bffill_and_ffill/thumbnail.png)
</a>
</div>
