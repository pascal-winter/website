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


Studying The Kaggle [Titanic dataset]( https://www.kaggle.com/c/titanic/) is a perfect introduction to Machine Learning.

The target is simple: from a Training dataset which contains passengers information and survival info (Yes or No), ones needs to predict the survival of the passengers in the Test dataset.

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

* **Take-away:**
   - *'Sex'*,*'Survived'*, *'Pclass'*, *'SibSp'*, *'Parch'* and *'Embarked'* are clearly categorical variables
   - *'Name'*, *'Ticket'* and *'Cabin'* will require some work if we are to gather useable information from them
   - *'Fare'* and *'Age'* look like continuous variable, however, their `nunique` count is quite low compared to the dataset size: Age is grouped by year and there were probably some "standard" ticket Fare
{: .notice--warning}


The above table also provide valuable information on the % of missing value for each row `isnull` and `null_pc`. 20% *'Age'* value are missing, while for *'Cabin'*, it goes up to 77%.
A better way to visualize the missing values - found initially on [this youtube channel]( https://www.youtube.com/user/krishnaik06) is a `seaborn` heatmap as follow:
```python
plt.figure(3)
sns.heatmap(df_data.isnull(), cbar=False, cmap='viridis')
```
Which ouptuts the following:   
![Test]({{site.baseurl}}/assets/images/titanic/titanic_isnull.png){: .align-center}

* **Take-away:**
   - *'Sex'*, *'Embarked'* and *'Cabin'* `NaN` will have to be dealt with
{: .notice--warning}

Note that similar analysis needs to be done on the **Test** dataset, and you might find some other values are missing...

## Data Visualisation

Visualization is a convenient way to get a grasp of some features interaction.

### Univariate  
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


* **Facts:**    
  - The survival rate is roughly 1/3
  - Female ratio is 1/30
  - Half of passengers were in 3rd class, other half in 2nd and 1st
  - Majority of passengers were traveling alone
  - Majority of passengers embarked in Southampton
  - *'Age'* distribution shows a bump is small values (0 to 3) then the majority is between 20-40. Note that there are some wide fluctuations from year to year
  - *'Fare shows'* a more classic distribution with a mean at 30, median at 15 and max at 500
{: .notice--success}

* **Take-aways:**    
  - We will probably have to bin the *'Age'* as the fluctuations could bring some noise if considered as continuous variable
  - Fare will have to be normalized: a log approach seems consistent
{: .notice--warning}



### Univariate with Target variable

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

* **Facts:**    
  - *'Sex'* is clearly a key variable
  - Young *'Age'* have a better survival under 10 then worst survival from 15 to 40
  - Higher *'Fare'* seems correlated with better survival
  - *'Class'* 3 shows worst survival than 1 and 2
  - *'Age'* distribution shows a bump is small values (0 to 3) then the majority is between 20-40. Note that there are some wide fluctuations from year to year
  - *'Fare shows'* a more classic distribution with a mean at 30, median at 15 and max at 500
  - Traveling with one sibling/spouse or parent/child seems correlated with better survival, though it is likely due to gender minmist_exemple
  - *'Embarked'* shows worst survival for Southampton than other ports
{: .notice--success}


* **Take-aways:**    
  - *'Sibsp'* and *'Parch'* will be grouped
  - Need to look at cross distribution for *'Embarked'* with *'Class'* or *'Sex'* and  *'Sibsp'* / *'Parch'* with *'Class'* or *'Sex'*
{: .notice--warning}


### Correlation

```python
# ------------------------------ Basic Heatmap --------------------------------#
fig, ax = plt.subplots(figsize=(12, 8))
ax.set_ylim(-0.5,len(dF_Train.corr()) + 0.5)
sns.heatmap(dF_Train.corr(), annot=True, linewidths=0.2, ax=ax)
```
![Test]({{site.baseurl}}/assets/images/titanic/titcorr.png)

### Multivariate

In order to visualize one to one, here are a few home made functions to graph Numerical with Categorical, Numerical with Numerical and Categorical with Categorical.

For Categorical with Categorical, we can use *"Balloon Charts"*, with the attached code below.

```python
# ----------------------------------- CAT with Num ----------------------------#
def graph_multvar_numcat(df_data, list_numvar, list_catvar):
    """
    Graph  simple bivariate Num with cat
    NumVar: Violin Chart
    CatVar: Barplot count
    """
    for i_num, item_num in enumerate(list_numvar):
        for i_cat, item_cat in enumerate(list_catvar):
            cond_a = df_data[item_num].isna() == False
            cond_b = df_data[item_cat].isna() == False
            dF_dummy = df_data.loc[cond_a & cond_b]
            plt.figure(i_cat)
            #sns.catplot(x=item_cat, y=item_num, data=dF_dummy)
            #sns.catplot(x=item_cat, y=item_num, data=dF_dummy, kind='violin')
            sns.catplot(x=item_cat, y=item_num, data=dF_dummy, kind='boxen')
    return
```

![Test]({{site.baseurl}}/assets/images/titanic/titmultcatnum(1).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatnum(2).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatnum(3).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatnum(4).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatnum(5).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatnum(6).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatnum(7).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatnum(8).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatnum(9).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatnum(10).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatnum(11).png)


```python
# ----------------------------------- Num with Num ----------------------------#
def graph_multvar_numnum(df_data, list_numvar):
    """
    Graph  simple bivariate Num with Num
    Scatter Plot
    """
    for i_num, item_num in enumerate(list_numvar):
        for i_num2, item_num2 in enumerate(list_numvar):
            if i_num != i_num2:
                cond_a = df_data[item_num].isna() == False
                cond_b = df_data[item_num2].isna() == False
                dF_dummy = df_data.loc[cond_a & cond_b]
                plt.figure(i_num)
                sns.relplot(x=item_num, y=item_num2, data=dF_dummy)
    return
```

![Test]({{site.baseurl}}/assets/images/titanic/titmultnumnum(1).png)

```python
# ----------------------------------- CAT with CAT ----------------------------#
def graph_multvar_catcat(df_data, list_catvar):
    """
    Graph  simple bivariate Cat with Cat
    Using Balloon Chart
    """
    for i_cat, item_cat in enumerate(list_catvar):
        for i_cat2, item_cat2 in enumerate(list_catvar):
            if i_cat != i_cat2:
                cond_a = df_data[item_cat].isna() == False
                cond_b = df_data[item_cat2].isna() == False
                dF_dummy = df_data.loc[cond_a & cond_b]
                plt.figure(i_cat)
                #sns.catplot(x=item_cat2, hue=item_cat, kind='count', data=dF_dummy)
                graph_balloon(dF_dummy,item_cat,item_cat2)
    return

# ------------------------ Baloon graph for multi cat -------------------------#
def graph_balloon(dF, x, y):
    """
    Prepare balloon chart with dF[x, y]

    """
    dF_data = dF.groupby([x,y])[y].count()
    dF_data = dF_data.rename("value")
    dF_data = dF_data.reset_index()
    # -------- Create the mapping dictionary for the x and y axis -------------#
    if pd.api.types.is_categorical_dtype(dF[x]) == True:
       xax_dict = dict(enumerate(dF[x].cat.categories))
    else:
       xax_dict = dict(enumerate(dF[x].unique()))
    if pd.api.types.is_categorical_dtype(dF[y]) == True:
       yax_dict = dict(enumerate(dF[y].cat.categories))
    else:
       yax_dict = dict(enumerate(dF[y].unique()))
    # -------------------------- Invert dict ----------------------------------#
    xax_dict = {v: k for k, v in xax_dict.items()}
    yax_dict = {v: k for k, v in yax_dict.items()}
    # --------------------------- Map the data --------------------------------#
    dF_data[x + '_'] = dF_data[x].map(xax_dict)
    dF_data[y +'_'] = dF_data[y].map(yax_dict)

    # -------------------------------- Graph ----------------------------------#
    fig, ax = plt.subplots()
    sns.scatterplot(x=x + '_', y=y +'_', size='value', data=dF_data
                , sizes=(20, 1000), hue='value')
                #, legend="full")
    ax.set_xlim(-1, len(xax_dict))
    ax.set_ylim(-1, len(yax_dict))
    ax.set_xticks(np.arange(len(xax_dict)))
    ax.set_yticks(np.arange(len(yax_dict)))
    ax.set_xticklabels(xax_dict.keys())
    ax.set_yticklabels(yax_dict.keys())
    return fig
```



![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (1).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (2).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (3).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (4).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (5).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (6).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (7).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (8).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (9).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (10).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (11).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (12).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (13).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (14).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (15).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (16).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (17).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (18).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (19).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (20).png)
![Test]({{site.baseurl}}/assets/images/titanic/titmultcatcat (21).png)

## Feature engineering
Take both Train and
### Deal with NaN
Mean / Most likely
### Categorical variable
One hot encoding
Numerical encoding
### Continuous variables
Binning
Normalising and Scaling


## Machine learning algorithm
### Setting
### Optimizing the parameters
### Assessing
### Output

## Conclusion
