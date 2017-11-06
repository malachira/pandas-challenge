

```python
import os
import pandas as pd
```


```python
#create filepath
filepath1 = os.path.join("purchase_data.json")
filepath2 = os.path.join("purchase_data2.json")
```


```python
#read the files into data-frames
data1_df = pd.read_json(filepath1)
data2_df = pd.read_json(filepath2)
```


```python
#merge data1 & data2
lst = [data1_df, data2_df]
data_df = pd.concat(lst)
data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#total_players = len(data_df["SN"])
total_players = data_df["SN"].nunique()
total_players
```




    612




```python
uniq_items = data_df["Item Name"].nunique()
```


```python
avg_purchase_price = data_df["Price"].mean()
```


```python
tot_purchases = len(data_df["Item Name"])
```


```python
tot_rev = data_df["Price"].sum()
```


```python
#create a data frame from the above calculations
purchasing_analysis = pd.DataFrame({"Number of Unique Items" : [uniq_items],
                                 "Average Purchase Price" : [avg_purchase_price],
                                 "Total Number of Purchases": [tot_purchases],
                                 "Total Revenue" : [tot_rev]
                                })
purchasing_analysis
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Number of Unique Items</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2.930571</td>
      <td>180</td>
      <td>858</td>
      <td>2514.43</td>
    </tr>
  </tbody>
</table>
</div>




```python
#create a dataframe with no duplicate entries for buyers
players_df = data_df.drop_duplicates("SN",keep='first', inplace=False)
```


```python
#groupby gender to extract gender related data
pd.DataFrame(players_df.groupby("Gender")["Gender"].count())
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>108</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>495</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>




```python
prcnt_female = (len(players_df.groupby("Gender").groups["Female"])/total_players)*100
prcnt_male = (len(players_df.groupby("Gender").groups["Male"])/total_players)*100
prcnt_other = (len(players_df.groupby("Gender").groups["Other / Non-Disclosed"])/total_players)*100
```


```python
#create a data frame to summarize gender %
gender_analysis = pd.DataFrame({"% Male " : [prcnt_male],
                                "% Female": [prcnt_female],
                                "% other": [prcnt_other]
                                })
gender_analysis
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>% Female</th>
      <th>% Male</th>
      <th>% other</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>17.647059</td>
      <td>80.882353</td>
      <td>1.470588</td>
    </tr>
  </tbody>
</table>
</div>




```python
data_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Use groupby & agg to perform purchase analysis on gender
avg_price = data_df.groupby("Gender",as_index=False).agg({"Price":"mean"})
avg_price = avg_price.rename(columns = {"Price":"average price"})
tot_price = data_df.groupby("Gender",as_index=False).agg({"Price":"sum"})
tot_price = tot_price.rename(columns = {"Price":"total purchase"})
tot_count = data_df.groupby("Gender",as_index=False).agg({"Price":"count"})
tot_count = tot_count.rename(columns = {"Price":"purchase count"})

```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Gender</th>
      <th>average price</th>
      <th>total purchase</th>
      <th>purchase count</th>
      <th>Normalized totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Female</td>
      <td>2.847584</td>
      <td>424.29</td>
      <td>149</td>
      <td>2.847584</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Male</td>
      <td>2.944448</td>
      <td>2052.28</td>
      <td>697</td>
      <td>2.944448</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>3.155000</td>
      <td>37.86</td>
      <td>12</td>
      <td>3.155000</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Merge the above data frames to create a summary df
lst = [tot_count, avg_price, tot_price]
gender_df = pd.merge(avg_price, tot_price)
gender_df = pd.merge(gender_df,tot_count )
gender_df["Normalized totals"] = gender_df["total purchase"] / gender_df["purchase count"]
gender_df
```
