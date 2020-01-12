---
layout: single2
title: "Titanic Kaggle: Basic Machine learning"
excerpt: "The infamous collision dataset"
classes: wide2
toc: true
#toc_sticky: true
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


Studying The Kaggle [Titanic dataset]( https://www.kaggle.com/c/titanic/) was a perfect introduction to Machine Learning: it allows to really apprehend practical considerations in a beginner friendly way.

The target is simple: from a Training dataset which contains passengers information and survival info (Yes or No), ones needs to predict the survival of the passengers in the Test dataset.

Below are some piece of code I used to do my own approach. Note that the purpose was not to score the highest (with some reflection, there are several ways the **Feature Engineering** could be boosted), but more to hone one skills on Python for Data Science and start to build my own library of functions that I could re-use and perfect for other studies.

The code listed below is **Spyder** friendly and not comprehensive.   
The full kernel can be accessed in the GitHub repository [actuarial-tools](https://github.com/pascal-winter/actuarial-tools/){: .btn .btn--success}   
Specifically:
- Titanic dedicated code: [Titanic.py](https://github.com/pascal-winter/actuarial-tools/blob/master/Titanic.py)
- Library "Data Analytics": [dalib.py](https://github.com/pascal-winter/actuarial-tools/blob/master/dalib.py)

## Initialisation
First we need to import the libraries. In this case, `pandas` and  `numpy` for data manipulation, `matplotlip` and `seaborn` for visualisation.

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
plt.figure(figsize=(12, 8))
sns.heatmap(dF_Train.corr(), annot=True, linewidths=0.2, #ax=ax,
            vmin=-0.5, vmax=0.5, center= 0, cmap='coolwarm')
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
In this part, we will re-arrange the variables for the algorithms to run properly.
We need to do it for both "Train" and "Test" `DataFrames`.
Note that for audit purpose, I copied both Dataset - however - on large data - this is not a recommended practice.
```python
# Copy orginal dataframes and create a global one
dF_Train2 = dF_Train.copy()
dF_Test2 = dF_Test.copy()
dF_Test2['Survived'] = np.nan
dF_Train2['Source'] = "Train"
dF_Test2['Source'] = "Test"
dF_All = dF_Train2.append(dF_Test2)
```

### Deal with `NaN`
To deal with `NaN`, we can use "Mean", "Median" or "Most likely" on the whole data or on subsets of it.   
* For *'Embarked'* missing data, single males showed that the most obvious port was Southampton: manually - force to S  
* For *'Age'*: we will take the "Mean" on a categorization by *'Sex'*, *'Sibsp(grouped)'*, *'Parch(grouped)'* and *'Class'*, as shown below:
```python
# Use a grouping  by Gender, SibSp(gpd), Parch(gpd) and Class
dF_dummy = dF_All.groupby(['Parch', 'SibSp', 'Pclass'])['Age'].agg(['count', 'mean'])
dF_All2 = pd.merge(dF_All, dF_dummy, how='left',
                              left_on=['Parch', 'SibSp', 'Pclass'], right_index=True)
# When Nan, assign the result of the mean age
dF_All2['Age_Fill'] = dF_All2['Age']
dF_All2.loc[dF_All2['Age'].isna() == True, 'Age_Fill'] = dF_All2.loc[dF_All2['Age'].isna() == True, 'mean']
# Clean
dF_All2 = dF_All2.drop('count', axis=1)
dF_All2 = dF_All2.drop('mean', axis=1)
dF_All2['Age_G'] = dF_All2['Age_Fill']
```

### Object variable
We will parse the *'Name'* to extract the *'Title'* and then standardise it.

```python
#------------------------ Title -----------------------------------------------#
dF_All["Title"] = dF_All['Name'].str.extract(' ([A-Za-z]+)\.', expand=False)
# get the dict for mapping
dict_map = dict(enumerate(dF_All['Title'].unique())) # enumerate
dict_map = {v: k for k, v in dict_map.items()} # reversreverse map
dict_map ={'Mr': 'Mr',
             'Mrs': 'Mrs',
             'Miss': 'Miss',
             'Master': 'Master',
             'Don': 'Mr',
             'Rev':  'Spe',
             'Dr': 'Spe',
             'Mme': 'Mrs',
             'Ms': 'Miss',
             'Major': 'Spe',
             'Lady': 'Spe',
             'Sir': 'Mr',
             'Mlle': 'Miss',
             'Col': 'Spe',
             'Capt': 'Spe',
             'Countess': 'Spe',
             'Jonkheer': 'Mr',
             'Dona': 'Mrs'}
# replace
dF_All['Title'] = dF_All['Title'].replace(dict_map)
```

### Categorical variable
For Categorical, we will use **One hot encoding** and **Numerical encoding** for cardinal variables.
* *'Pclass'*: keep as is (1,2,3 is well anti correlated with ouput) as shown below:
* *'SibSp'*: group: 0 / 1 / 2 / 3-4-5-8 - keep as is
* *'Sex'*: 0 / 1
* *'Parch'*: group: 0 / 1 / 2 -5-3-4-6 - keep as is
* *'Embarked'*: One hot encoding
* *'Title'*: One hot encoding

Here is an example of **One hot encoding**:
```python
# Hot encoding
dF_dummy = pd.get_dummies(dF_All['Title'], prefix = 'Title' )
dF_All = pd.concat([dF_All, dF_dummy], axis = 1)
```


### Continuous variables
For categorical, we will use **Binning** on *'Age'* and will **Normalise** *'Fare'*.


## Machine learning algorithm
The code below was mostly inspired from these 2 kernels, even though the structure is now quite different:   
[Kernel 1](https://www.kaggle.com/atuljhasg/titanic-top-3-models)   
[Kernel 2](https://www.kaggle.com/rishitdagli/titanic-prediction-using-9-algorithims)   
They are great reads and were very valuable to me.

The 3 classifiers (or models) we are going to use are:
* A standard **Logistic Regression**
* A **Random Forest**
* A **Gradient Booster** classifier

### Setting
For testing goodness of fit, we will split the Train data in a sub-Training and validation subsets:
```python
# -------------------- Split the data for testing -----------------------------#
X = dF_TrainFinal.drop(['Survived'], axis=1)
y = dF_TrainFinal["Survived"]
X_train, X_val, y_train, y_val = train_test_split(X, y, test_size = 0.2, random_state = 0)
```
We will then initialise the algorithms. I will come back later on the parametrisation of the classifiers.

```python
# Model Names (the size will define the # of models)
lModelName = ['Logistic Regression','Random Forest','GradientBoosting']
# -------------------- Define Model parameters
param = [{} for i in range(len(lModelName))]
# Parameters for logit
param[0] = {'max_iter': 1000,
            'C': 10.0,
            'penalty': 'l2'}
# Parameters for random forest
param[1] = {'max_features': 9,
            'min_samples_leaf': 10,
            'n_estimators': 500}
# Parameter for
param[2] = {'n_estimators': 10,
            'learning_rate': 0.1,
            'max_depth': 6,
            'max_features': 1.0,
            'min_samples_leaf': 20}
# ------------------ Define Models
models = [LogisticRegression(**param[0]),
          RandomForestClassifier(**param[1]),
          GradientBoostingClassifier(**param[2])]
# ------------------ Define Score Train results
dF_ModelScore = pd.DataFrame({
        'Model': np.arange(len(models)),
        'Train Score': np.zeros(len(models)),
        'CVal Score': np.zeros(len(models))})
# Define Model features results
dF_ModelFeat = pd.DataFrame(index = X_train.columns)
```

### Training
We can than then train each classifier and store their fit accuracy:

```python
for i in range(len(models)):
    # Train the model
    models[i].fit(X_train,y_train)
    # Log the training score and cross val score
    dF_ModelScore.loc[i,:] = [lModelName[i],
                      models[i].score(X_train,y_train),  
                      accuracy_score(y_val, models[i].predict(X_val))]
    #print(classification_report(y_val, models[1].predict(X_val)))
```

### Assessing

For assessing, we will a KFold cross validation. The algorithm is derived from above Kaggle links, and I repackaged it into my own functions for future re-use:
```python
def model_assessment(classifiers, models, X, y, nsplit):
    """
    Assess a series of model with several sim and Provide a Boxplot of dsitrib results
    -------------
    classifiers = ['Logistic Regression','Random Forest','GradientBoosting']
    models = [LogisticRegression(),RandomForestClassifier(n_estimators=100),GradientBoostingClassifier(n_estimators=7,learning_rate=1.1)]
    X: full data predictors
    y: full data target
    nsplit: number of split
    -----------------
    """
    from sklearn.model_selection import KFold #for K-fold cross validation
    from sklearn.model_selection import cross_val_score #score evaluation
    from sklearn.model_selection import cross_val_predict #prediction
    from sklearn import svm #support vector Machine
    kfold = KFold(n_splits=nsplit, random_state=22) # k=10, split the data into 10 equal parts
    lMean = []
    nAAccuracy = []
    lStdev = []
    for i in models:
        model = i
        cv_result = cross_val_score(model, X, y, cv=kfold, scoring="accuracy")
        lMean.append(cv_result.mean())
        lStdev.append(cv_result.std())
        nAAccuracy.append(cv_result)
    df_Res = pd.DataFrame({'CV Mean':lMean,'Std':lStdev}, index=classifiers)
    df_Acc = pd.DataFrame(nAAccuracy, index=classifiers)
    plt.figure(0)
    sns.catplot(kind='box', data=df_Acc.T)
    return df_Res, df_Acc
```
Final results are as follow:    
![Test]({{site.baseurl}}/assets/images/titanic/titmodelassess.png){: .align-center}

An important step to understand the models is to be able to track the **feature importance**.
This can allow further tuning on the **Feature Engineering** and enhance the overall fit of the classifier:
```python
# ----------- Assess the models by "Bootstrapping" them on various subsets
dF_ModelAssess , dF_ModelAccuracy = dalib.model_assessment(lModelName, models, X, y, 5)
# ----------- Get feature importance (cannot loop since logit is different)
dF_ModelFeat[lModelName[0]] = abs(models[0].coef_[0])
dF_ModelFeat[lModelName[0]]  = dF_ModelFeat[lModelName[0]] /dF_ModelFeat[lModelName[0]].sum()
dF_ModelFeat[lModelName[1]] = models[1].feature_importances_
dF_ModelFeat[lModelName[2]] = models[2].feature_importances_
# Graph
dF_ModelFeat2 = dF_ModelFeat.stack().reset_index().rename(columns = {"level_0": "Feat", "level_1": "Model", 0: "Value"})
plt.figure(5)
sns.catplot(data=dF_ModelFeat2, x="Feat", y = "Value", hue="Model", kind ='bar').set_xticklabels(rotation=45)
```
Which provides a comparison of the feature importance for each model:
![Test]({{site.baseurl}}/assets/images/titanic/titfeatimp.png){: .align-center}


### Optimizing the parameters

To optimize the classifier parameters, I used a **GridSearch**, which is basically a brute force selection of the most effective model through a Grid simulation. Below is the code for the 3 classifiers:

```python
from sklearn.model_selection import GridSearchCV

###############  RANDOM GRid Search for Random Forest
parameters = {'max_features': np.arange(5, 10), 'n_estimators':[500], 'min_samples_leaf':[10, 50, 100, 200, 500]}
random_grid = GridSearchCV(models[1], parameters, cv = 5, verbose=10)
random_grid.fit(X_train,y_train)
a = random_grid.cv_results_
a = random_grid.best_estimator_
a = random_grid.best_params_

################  RANDOM GRid Search for Logit
parameters = {"C":np.logspace(-3,3,7), "penalty":["l1","l2"]} # l1 lasso l2 ridge
random_grid = GridSearchCV(models[0], parameters, cv=10, verbose=10)
random_grid.fit(X_train,y_train)
a = random_grid.best_params_

###############  RANDOM GRid Search for GradientBoosting
parameters = {
    "loss":["deviance"],
    "learning_rate": [0.01, 0.025, 0.05, 0.075, 0.1, 0.15, 0.2],
    "min_samples_split": np.linspace(0.1, 0.5, 12),
    "min_samples_leaf": np.linspace(0.1, 0.5, 12),
    "max_depth":[3,5,8],
    "max_features":["log2","sqrt"],
    "criterion": ["friedman_mse",  "mae"],
    "subsample":[0.5, 0.618, 0.8, 0.85, 0.9, 0.95, 1.0],
    "n_estimators":[10]
    }
parameters = {'learning_rate': [0.1, 0.05, 0.02, 0.01],
              'max_depth': [4, 6, 8],
              'min_samples_leaf': [20, 50, 100,150],
              'max_features': [1.0, 0.3, 0.1]
              }
random_grid = GridSearchCV(models[2], parameters, cv=10, verbose=3)
random_grid.fit(X_train,y_train)
a = random_grid.best_params_
```

## Conclusion
* *Humbling*:
     - when the first submission of a complicated algorithm scores less than just using Sex as survival...
     - there is still much to be understood on the classifiers, proper model validation and cross checking
* Final Kaggle score: [0.78468](https://www.kaggle.com/wiloo82), which is when I write these lines at 35% best score. Not good neither bad - but the thought process to do this study was the most valuable.
