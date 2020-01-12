---
title: "Allocation Engine Tryout"
excerpt: "A simple routine for allocating anything"
classes: wide2
toc: true
#toc_sticky: true
toc_label: "Content"
header:
  image: /assets/images/opex/asteroid.jpg
  teaser: /assets/images/opex/asteroid.jpg

---

Allocation: Expenses, Revenues or anything really - is a common thematic in Actuarial and Finance field.

In the context of **IFRS17**, this problematic becomes even more crucial since actual revenues and expense items will have to be allocated at a **Unit of Account** level on Monthly or Quarterly basis.
This new level of complexity requires adequate tools and automation.   

We will see here the potential of `Pandas` for allocation - on a simple example.

## Introduction
In a galaxy far, far away, an asteroid miner, "Jill" has a good idea on the revenues she generates per **asteroid** mined. But she still needs to clearly identify the fully loaded resources she spent on each case. Now that her **Droid mining fleet** is stable and her ship - the infamous **"Millennials Fallout"** is operational, she has time to analyse her resource allocation.

## Data
By querying the **"Millennials Fallout"** registry, Jill's is able to get a summary of the expenses incurred on the last revolution of her home planet around its star (*commonly known as "year" for earth inhabitants")*.

She gets the following output (truncated for the 15 first entries), called **`dF_BaseCost`**:
<div><style scoped>    .dataframe tbody tr th:only-of-type {        vertical-align: middle;    }    .dataframe tbody tr th {        vertical-align: top;    }    .dataframe thead th {        text-align: right;    }</style><table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>Center</th>      <th>Nature</th>      <th>Amount</th>    </tr>  </thead>  <tbody>    <tr>      <th>0</th>      <td>MilleniumFallout</td>      <td>Defense</td>      <td>135,281</td>    </tr>    <tr>      <th>1</th>      <td>MilleniumFallout</td>      <td>Prospection</td>      <td>270,008</td>    </tr>    <tr>      <th>2</th>      <td>MilleniumFallout</td>      <td>Logistic</td>      <td>179,362</td>    </tr>    <tr>      <th>3</th>      <td>MilleniumFallout</td>      <td>Extraction</td>      <td>724,089</td>    </tr>    <tr>      <th>4</th>      <td>MilleniumFallout</td>      <td>Maintenance</td>      <td>343,378</td>    </tr>    <tr>      <th>5</th>      <td>MilleniumFallout</td>      <td>Relation</td>      <td>120,682</td>    </tr>    <tr>      <th>6</th>      <td>DroidFleet</td>      <td>Defense</td>      <td>238,004</td>    </tr>    <tr>      <th>7</th>      <td>DroidFleet</td>      <td>Prospection</td>      <td>484,864</td>    </tr>    <tr>      <th>8</th>      <td>DroidFleet</td>      <td>Logistic</td>      <td>293,807</td>    </tr>    <tr>      <th>9</th>      <td>DroidFleet</td>      <td>Extraction</td>      <td>1,082,120</td>    </tr>    <tr>      <th>10</th>      <td>DroidFleet</td>      <td>Maintenance</td>      <td>514,404</td>    </tr>    <tr>      <th>11</th>      <td>DroidFleet</td>      <td>Relation</td>      <td>387,256</td>    </tr>    <tr>      <th>12</th>      <td>Contractors</td>      <td>Defense</td>      <td>46,088</td>    </tr>    <tr>      <th>13</th>      <td>Contractors</td>      <td>Prospection</td>      <td>102,434</td>    </tr>    <tr>      <th>14</th>      <td>Contractors</td>      <td>Logistic</td>      <td>65,326</td>    </tr>  </tbody></table></div>
*<Dataframe (36,3)>*

She notes that her total expense amounts to **6,194,655** Credits, which is slightly more than she initially thought.


She also look for the current 5 asteroids she is mining and the number of "month - Droids" deployed and the revenues generated - called **`dF_Asteroids`**:
<div><style scoped>    .dataframe tbody tr th:only-of-type {        vertical-align: middle;    }    .dataframe tbody tr th {        vertical-align: top;    }    .dataframe thead th {        text-align: right;    }</style><table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>Asteroid</th>      <th>noDroids</th>      <th>Extracted</th>    </tr>  </thead>  <tbody>    <tr>      <th>0</th>      <td>Enigma</td>      <td>549</td>      <td>64,589</td>    </tr>    <tr>      <th>1</th>      <td>Europa</td>      <td>715</td>      <td>43,759</td>    </tr>    <tr>      <th>2</th>      <td>Genosis</td>      <td>603</td>      <td>89,177</td>    </tr>    <tr>      <th>3</th>      <td>Tarsis</td>      <td>545</td>      <td>96,366</td>    </tr>    <tr>      <th>4</th>      <td>Altoria</td>      <td>424</td>      <td>38,344</td>    </tr>  </tbody></table></div>
*<Dataframe (5,3)>*

Since she realises she might be late for an Asteroid guild conference, she decides that it is enough for a first "rough" assessment - and also choses to use the **Number of Droids** deployed as an allocation key.

## Allocation

She then types the following instructions.

```python
#--------------------------- Develop Target Dataframe -------------------------#
dF_BaseCost2 = dF_BaseCost.assign(key=1).merge(dF_Asteroid.assign(key=1), on='key').drop('key', 1)
```
Here, she develops the Dataframe **`dF_BaseCost`** as a Cartesian product of the "Center - Nature" with "Asteroid".
This allows her to get a line for each Asteroid and couple `(Center, Nature)`.
The **`dF_BaseCost`** containing 36 combinaisons and **`dF_Asteroids`** 5 different Asteroid, this gives her a 5 * 36 = 180 lines:

Which is confirmed when she prints **`dF_BaseCost2`**:
<div><style scoped>    .dataframe tbody tr th:only-of-type {        vertical-align: middle;    }    .dataframe tbody tr th {        vertical-align: top;    }    .dataframe thead th {        text-align: right;    }</style><table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>Center</th>      <th>Nature</th>      <th>Amount</th>      <th>Asteroid</th>      <th>noDroids</th>      <th>Extracted</th>      <th>key1</th>    </tr>  </thead>  <tbody>    <tr>      <th>0</th>      <td>MilleniumFallout</td>      <td>Defense</td>      <td>135,281</td>      <td>Enigma</td>      <td>549</td>      <td>64,589</td>      <td>549</td>    </tr>    <tr>      <th>1</th>      <td>MilleniumFallout</td>      <td>Defense</td>      <td>135,281</td>      <td>Europa</td>      <td>715</td>      <td>43,759</td>      <td>715</td>    </tr>    <tr>      <th>2</th>      <td>MilleniumFallout</td>      <td>Defense</td>      <td>135,281</td>      <td>Genosis</td>      <td>603</td>      <td>89,177</td>      <td>603</td>    </tr>    <tr>      <th>3</th>      <td>MilleniumFallout</td>      <td>Defense</td>      <td>135,281</td>      <td>Tarsis</td>      <td>545</td>      <td>96,366</td>      <td>545</td>    </tr>    <tr>      <th>4</th>      <td>MilleniumFallout</td>      <td>Defense</td>      <td>135,281</td>      <td>Altoria</td>      <td>424</td>      <td>38,344</td>      <td>424</td>    </tr>    <tr>      <th>5</th>      <td>MilleniumFallout</td>      <td>Prospection</td>      <td>270,008</td>      <td>Enigma</td>      <td>549</td>      <td>64,589</td>      <td>549</td>    </tr>    <tr>      <th>6</th>      <td>MilleniumFallout</td>      <td>Prospection</td>      <td>270,008</td>      <td>Europa</td>      <td>715</td>      <td>43,759</td>      <td>715</td>    </tr>    <tr>      <th>7</th>      <td>MilleniumFallout</td>      <td>Prospection</td>      <td>270,008</td>      <td>Genosis</td>      <td>603</td>      <td>89,177</td>      <td>603</td>    </tr>    <tr>      <th>8</th>      <td>MilleniumFallout</td>      <td>Prospection</td>      <td>270,008</td>      <td>Tarsis</td>      <td>545</td>      <td>96,366</td>      <td>545</td>    </tr>    <tr>      <th>9</th>      <td>MilleniumFallout</td>      <td>Prospection</td>      <td>270,008</td>      <td>Altoria</td>      <td>424</td>      <td>38,344</td>      <td>424</td>    </tr>    <tr>      <th>10</th>      <td>MilleniumFallout</td>      <td>Logistic</td>      <td>179,362</td>      <td>Enigma</td>      <td>549</td>      <td>64,589</td>      <td>549</td>    </tr>    <tr>      <th>11</th>      <td>MilleniumFallout</td>      <td>Logistic</td>      <td>179,362</td>      <td>Europa</td>      <td>715</td>      <td>43,759</td>      <td>715</td>    </tr>    <tr>      <th>12</th>      <td>MilleniumFallout</td>      <td>Logistic</td>      <td>179,362</td>      <td>Genosis</td>      <td>603</td>      <td>89,177</td>      <td>603</td>    </tr>    <tr>      <th>13</th>      <td>MilleniumFallout</td>      <td>Logistic</td>      <td>179,362</td>      <td>Tarsis</td>      <td>545</td>      <td>96,366</td>      <td>545</td>    </tr>    <tr>      <th>14</th>      <td>MilleniumFallout</td>      <td>Logistic</td>      <td>179,362</td>      <td>Altoria</td>      <td>424</td>      <td>38,344</td>      <td>424</td>    </tr>  </tbody></table></div>
*<Dataframe (180,7)>*

She then attach the allocation key - by summing "'noDroids'" on each original segment and re-attaching to the developed dataframe:
```python
#--------------------------- Calculate the allocation key ---------------------#
dF_BaseCost2['key1'] = dF_BaseCost2['noDroids']
dF_Dummy = dF_BaseCost2.groupby(['Center','Nature'])['key1'].sum().rename('key1sum')
dF_BaseCost3 = dF_BaseCost2.merge(dF_Dummy, how='left', left_on=['Center','Nature'],
                                 right_index =True)#.reset_index()
```

She can after easily obtain the allocated expenses by applying the key to the nominal expense amount:

```python
#--------------------------- Calculate the amount -----------------------------#
dF_BaseCost3['Amount'] = dF_BaseCost3['Amount'] * dF_BaseCost3['key1']
dF_BaseCost3['Amount'] = dF_BaseCost3['Amount'] / dF_BaseCost3['key1sum']
```
Which gets her:

<div><style scoped>    .dataframe tbody tr th:only-of-type {        vertical-align: middle;    }    .dataframe tbody tr th {        vertical-align: top;    }    .dataframe thead th {        text-align: right;    }</style><table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>Center</th>      <th>Nature</th>      <th>Amount</th>      <th>Asteroid</th>      <th>noDroids</th>      <th>Extracted</th>      <th>key1</th>      <th>key1sum</th>    </tr>  </thead>  <tbody>    <tr>      <th>0</th>      <td>MilleniumFallout</td>      <td>Defense</td>      <td>26,186</td>      <td>Enigma</td>      <td>549</td>      <td>64,589</td>      <td>549</td>      <td>2,835</td>    </tr>    <tr>      <th>1</th>      <td>MilleniumFallout</td>      <td>Defense</td>      <td>34,124</td>      <td>Europa</td>      <td>715</td>      <td>43,759</td>      <td>715</td>      <td>2,835</td>    </tr>    <tr>      <th>2</th>      <td>MilleniumFallout</td>      <td>Defense</td>      <td>28,760</td>      <td>Genosis</td>      <td>603</td>      <td>89,177</td>      <td>603</td>      <td>2,835</td>    </tr>    <tr>      <th>3</th>      <td>MilleniumFallout</td>      <td>Defense</td>      <td>25,998</td>      <td>Tarsis</td>      <td>545</td>      <td>96,366</td>      <td>545</td>      <td>2,835</td>    </tr>    <tr>      <th>4</th>      <td>MilleniumFallout</td>      <td>Defense</td>      <td>20,214</td>      <td>Altoria</td>      <td>424</td>      <td>38,344</td>      <td>424</td>      <td>2,835</td>    </tr>    <tr>      <th>5</th>      <td>MilleniumFallout</td>      <td>Prospection</td>      <td>52,264</td>      <td>Enigma</td>      <td>549</td>      <td>64,589</td>      <td>549</td>      <td>2,835</td>    </tr>    <tr>      <th>6</th>      <td>MilleniumFallout</td>      <td>Prospection</td>      <td>68,108</td>      <td>Europa</td>      <td>715</td>      <td>43,759</td>      <td>715</td>      <td>2,835</td>    </tr>    <tr>      <th>7</th>      <td>MilleniumFallout</td>      <td>Prospection</td>      <td>57,402</td>      <td>Genosis</td>      <td>603</td>      <td>89,177</td>      <td>603</td>      <td>2,835</td>    </tr>    <tr>      <th>8</th>      <td>MilleniumFallout</td>      <td>Prospection</td>      <td>51,890</td>      <td>Tarsis</td>      <td>545</td>      <td>96,366</td>      <td>545</td>      <td>2,835</td>    </tr>    <tr>      <th>9</th>      <td>MilleniumFallout</td>      <td>Prospection</td>      <td>40,345</td>      <td>Altoria</td>      <td>424</td>      <td>38,344</td>      <td>424</td>      <td>2,835</td>    </tr>    <tr>      <th>10</th>      <td>MilleniumFallout</td>      <td>Logistic</td>      <td>34,718</td>      <td>Enigma</td>      <td>549</td>      <td>64,589</td>      <td>549</td>      <td>2,835</td>    </tr>    <tr>      <th>11</th>      <td>MilleniumFallout</td>      <td>Logistic</td>      <td>45,243</td>      <td>Europa</td>      <td>715</td>      <td>43,759</td>      <td>715</td>      <td>2,835</td>    </tr>    <tr>      <th>12</th>      <td>MilleniumFallout</td>      <td>Logistic</td>      <td>38,131</td>      <td>Genosis</td>      <td>603</td>      <td>89,177</td>      <td>603</td>      <td>2,835</td>    </tr>    <tr>      <th>13</th>      <td>MilleniumFallout</td>      <td>Logistic</td>      <td>34,469</td>      <td>Tarsis</td>      <td>545</td>      <td>96,366</td>      <td>545</td>      <td>2,835</td>    </tr>    <tr>      <th>14</th>      <td>MilleniumFallout</td>      <td>Logistic</td>      <td>26,801</td>      <td>Altoria</td>      <td>424</td>      <td>38,344</td>      <td>424</td>      <td>2,835</td>    </tr>  </tbody></table></div>
*<Dataframe (180,8)>*

And just to be sure - a sum of **6,194,655** Credits.


## Summary

The following routine can be adapted to work with multiple keys, different aggregation levels and various sub-selection.
With additional steps and further tuning, it has the potential to fully develop, aggregate and allocate any kind of expenses on large datasets.

**Note:**
In order to use this approach, we need to ensure that the initial Dataframe is aggregated at a `['Center','Nature']` level (ie all the combinations of these 2 columns are unique. This can be performed by a `pd.grouby`
{: .notice--warning}


```python
################################################################################
################################### ALLOCATION #################################
################################################################################

#--------------------------- Develop Target Dataframe -------------------------#
dF_BaseCost2 = dF_BaseCost.assign(key=1).merge(dF_Asteroid.assign(key=1), on='key').drop('key', 1)

#--------------------------- Calculate the allocation key ---------------------#
dF_BaseCost2['key1'] = dF_BaseCost2['noDroids']
dF_Dummy = dF_BaseCost2.groupby(['Center','Nature'])['key1'].sum().rename('key1sum')
dF_BaseCost3 = dF_BaseCost2.merge(dF_Dummy, how='left', left_on=['Center','Nature'],
                                 right_index =True)#.reset_index()
#--------------------------- Calculate the amount -----------------------------#
dF_BaseCost3['Amount'] = dF_BaseCost3['Amount'] * dF_BaseCost3['key1']
dF_BaseCost3['Amount'] = dF_BaseCost3['Amount'] / dF_BaseCost3['key1sum']
```
