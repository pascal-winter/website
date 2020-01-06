---
layout: single2
title: "Titanic Kaggle: Basic Machine learning"
excerpt: "The infamous collision dataset"
classes: wide2
toc: true
toc_sticky: true
toc_label: "Content"
header:
  image: /assets/images/titanic/titanic.jpg
  teaser: /assets/images/titanic/titanic.jpg
#sidebar:
#  - title: "Titanic Dataset"
#    text: "Kaggle Competition"
#    image: /assets/images/kaggle.jpg
#gallery:
#  - image_path: /assets/images/titanic/titv (1).png
#    url: /assets/images/titanic/titv (1).png
#  - image_path: /assets/images/titanic/titv (2).png
#  - image_path: /assets/images/titanic/titv (3).png

# Code to put in MD to show gallery:
# {% include gallery caption="This is a sample gallery to go along with this case study." %}
---


Kaggle [Titanic dataset]( https://www.kaggle.com/c/titanic/) is well suited to begin machine learning.
The goal is simple: from a Training dataset which contains passengers information and survival info (Yes or No), ones needs to predict the survival of the passengers in the Test dataset.

## Initialisation
First we need to import the libraries. In this case, `pandas` and  `numpy` for data manipulation, `matplotlip` and `seaborn` for visualisation. Note that `dalib` is a home made library.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```
The `pathlib` library will be useful to navigate in working directory.
```python
from pathlib import Path
CWD = Path.cwd()
```

We can then import the data. The method `CWD.rglob` will find the path of the listed file name automatically in the CWD. More information here: [`pathlib`](https://docs.python.org/3/library/pathlib.html). To load, a standard `pd.read_csv` will do.

```python
# ----------------------------- Import Dataset --------------------------------#
csv_test = list(CWD.rglob('test.csv'))[0]
dF_Test = pd.read_csv(csv_test)
csv_train = list(CWD.rglob('train.csv'))[0]
dF_Train = pd.read_csv(csv_train)
```



## Data Exploration
Let's start with the exploration phase. A small extension of the `Pandas.describe()` gives us a quick overview:
```python
def data_describe(df_data):
    """
    Add further description to describe
    Plot a heatmap with Null values
    """
    # ------------- Calculate a Basic Description  --------------------------------#
    df_des = pd.DataFrame(index=df_data.columns)
    df_des['dtypes'] = df_data.dtypes
    df_des['nunique'] = df_data.nunique()
    df_des['isnull'] = df_data.isnull().sum()
    df_des['null_pc'] = df_des['isnull'] / df_data.shape[0]
    # link with describe file
    dF_dummy = df_data.describe().transpose()
    df_des = pd.merge(df_des, dF_dummy, how ='left', left_index=True, right_index=True)
    # Produce a Is Na heatmap
    # Video on youtube
    plt.figure(3)
    sns.heatmap(df_data.isnull(), cbar=False, cmap='viridis')
    return df_des
```



<div><style scoped>    .dataframe tbody tr th:only-of-type {        vertical-align: middle;    }    .dataframe tbody tr th {        vertical-align: top;    }    .dataframe thead th {        text-align: right;    }</style><table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>dtypes</th>      <th>nunique</th>      <th>isnull</th>      <th>null_pc</th>      <th>count</th>      <th>mean</th>      <th>std</th>      <th>min</th>      <th>25%</th>      <th>50%</th>      <th>75%</th>      <th>max</th>      <th>Type</th>    </tr>  </thead>  <tbody>    <tr>      <th>Survived</th>      <td>bool</td>      <td>2</td>      <td>0</td>      <td>0.000000</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>Cat</td>    </tr>    <tr>      <th>Pclass</th>      <td>int64</td>      <td>3</td>      <td>0</td>      <td>0.000000</td>      <td>891.0</td>      <td>2.308642</td>      <td>0.836071</td>      <td>1.00</td>      <td>2.0000</td>      <td>3.0000</td>      <td>3.0</td>      <td>3.0000</td>      <td>Cat</td>    </tr>    <tr>      <th>Name</th>      <td>object</td>      <td>891</td>      <td>0</td>      <td>0.000000</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>Object</td>    </tr>    <tr>      <th>Sex</th>      <td>category</td>      <td>2</td>      <td>0</td>      <td>0.000000</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>Cat</td>    </tr>    <tr>      <th>Age</th>      <td>float64</td>      <td>88</td>      <td>177</td>      <td>0.198653</td>      <td>714.0</td>      <td>29.699118</td>      <td>14.526497</td>      <td>0.42</td>      <td>20.1250</td>      <td>28.0000</td>      <td>38.0</td>      <td>80.0000</td>      <td>Num</td>    </tr>    <tr>      <th>SibSp</th>      <td>int64</td>      <td>7</td>      <td>0</td>      <td>0.000000</td>      <td>891.0</td>      <td>0.523008</td>      <td>1.102743</td>      <td>0.00</td>      <td>0.0000</td>      <td>0.0000</td>      <td>1.0</td>      <td>8.0000</td>      <td>Cat</td>    </tr>    <tr>      <th>Parch</th>      <td>int64</td>      <td>7</td>      <td>0</td>      <td>0.000000</td>      <td>891.0</td>      <td>0.381594</td>      <td>0.806057</td>      <td>0.00</td>      <td>0.0000</td>      <td>0.0000</td>      <td>0.0</td>      <td>6.0000</td>      <td>Cat</td>    </tr>    <tr>      <th>Ticket</th>      <td>object</td>      <td>681</td>      <td>0</td>      <td>0.000000</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>Object</td>    </tr>    <tr>      <th>Fare</th>      <td>float64</td>      <td>248</td>      <td>0</td>      <td>0.000000</td>      <td>891.0</td>      <td>32.204208</td>      <td>49.693429</td>      <td>0.00</td>      <td>7.9104</td>      <td>14.4542</td>      <td>31.0</td>      <td>512.3292</td>      <td>Num</td>    </tr>    <tr>      <th>Cabin</th>      <td>object</td>      <td>147</td>      <td>687</td>      <td>0.771044</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>Object</td>    </tr>    <tr>      <th>Embarked</th>      <td>category</td>      <td>3</td>      <td>2</td>      <td>0.002245</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>NaN</td>      <td>Cat</td>    </tr>  </tbody></table></div>

* *'Sex'*,*'Survived'*, *'Pclass'*, *'SibSp'*, *'Parch'* and *'Embarked'* are clearly categorical variables.
* *'Name'*, *'Ticket'* and *'Cabin'* will require some work if we are to gather useable information from them
* *'Fare'* and *'Age'* look like continuous variable, though for *'Age'* the fact is that they are grouped by year except for small babies.

The above table also provide valuable information on the % of missing value for each row `isnull` and `null_pc`. 20% *'Age'* value are missing, while for *'Cabin'*, it goes up to 77%.
A better way to visualize the missing values - found initially on [this youtube channel]( https://www.youtube.com/user/krishnaik06) is a `seaborn` heatmap as follow:
```python
plt.figure(3)
sns.heatmap(df_data.isnull(), cbar=False, cmap='viridis')
```
Which ouptuts the following:   
![Test]({{site.baseurl}}/assets/images/titanic/titanic_isnull.png)

Note that similar analysis needs to be done on the **Test** dataset, and you might find some other values are missing...

## Data Visualisation

Since humans struggle to go through large table of raw data - visualization can be a convenient way to get a grasp of some features.  
Below is a simple representation for each variable - classified in "numerical" and "categorical", using the `seaborn` library:

```python
# ------------------------ Univariate Graphing --------------------------------#
def graph_univar(df_data, list_numvar, list_catvar):
    """
    Graph very simple univariate for each variable
    NumVar: Histogram
    Catvar: BarPlot based on Count
    """
    # Histograms for Numerical variable
    for i, item in enumerate(list_numvar):
        cond_a = df_data[item].isna() == False
        dF_dummy = df_data.loc[cond_a]
        plt.figure(i)
        sns.distplot(dF_dummy[item])
    # Catplots for Cat variable
    for i, item in enumerate(list_catvar):
        cond_a = df_data[item].isna() == False
        dF_dummy = df_data.loc[cond_a]
        plt.figure(i)
        sns.catplot(x=item, kind="count", data=dF_dummy)
    return
```

Applied to the training Dataset, this gives:

![Test]({{site.baseurl}}/assets/images/titanic/titv (1).png)
![Test]({{site.baseurl}}/assets/images/titanic/titv (2).png)
![Test]({{site.baseurl}}/assets/images/titanic/titv (3).png)
![Test]({{site.baseurl}}/assets/images/titanic/titv (4).png)
![Test]({{site.baseurl}}/assets/images/titanic/titv (5).png)
![Test]({{site.baseurl}}/assets/images/titanic/titv (6).png)
![Test]({{site.baseurl}}/assets/images/titanic/titv (7).png)
![Test]({{site.baseurl}}/assets/images/titanic/titv (8).png)
![Test]({{site.baseurl}}/assets/images/titanic/titv (9).png)

A similar routine, this time including the comparison with the target variable *'Survived'* gives:

```python
# --------------- Univariate Graphing with prediction -------------------------#
def graph_univar_pred(df_data, col_predict , list_numvar, list_catvar):
    """
    Graph very simple univariate for each variable compared with prediction (label in dF)
    NumVar: Violin Chart
    CatVar: Barplot count
    """
    # Histograms for Numerical variable
    for i, item in enumerate(list_numvar):
        cond_a = df_data[item].isna() == False
        cond_b = df_data[col_predict] != np.nan
        dF_dummy = df_data.loc[cond_a & cond_b]    
        plt.figure(i)
        sns.catplot(x=col_predict, y=item, kind='violin', data=dF_dummy)
    # Catplots for Cat with prediction
    for i, item in enumerate(list_catvar):
        cond_a = df_data[item].isna() == False
        cond_b = df_data[col_predict].isna() == False
        dF_dummy = df_data.loc[cond_a & cond_b]    
        plt.figure(i)
        sns.catplot(x=item, hue=col_predict, kind='count', data=dF_dummy)
    return
```

![Test]({{site.baseurl}}/assets/images/titanic/titvp (1).png)
![Test]({{site.baseurl}}/assets/images/titanic/titvp (2).png)
![Test]({{site.baseurl}}/assets/images/titanic/titvp (3).png)
![Test]({{site.baseurl}}/assets/images/titanic/titvp (4).png)
![Test]({{site.baseurl}}/assets/images/titanic/titvp (5).png)
![Test]({{site.baseurl}}/assets/images/titanic/titvp (6).png)
![Test]({{site.baseurl}}/assets/images/titanic/titvp (7).png)
![Test]({{site.baseurl}}/assets/images/titanic/titvp (8).png)
![Test]({{site.baseurl}}/assets/images/titanic/titvp (9).png)




## Feature engineering

## Machine learning algorithm

## Conclusion
