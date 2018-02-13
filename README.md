# Heroes_of_Pymoli



```python
# Import Dependencies
import pandas as pd
import os
```


```python
# Create a reference the JSON file desired
json_path = os.path.join('Resources', 'purchase_data.json')

# Read the CSV into a Pandas DataFrame
purchase_data_df = pd.read_json(json_path)
```


```python
# Print the first five rows of the first data frame to the screen
purchase_data_df.head()
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
#Counting the total number of unique players
purchase_data_df["SN"].nunique()
```




    573




```python
#Creating the values for the purchasing analysis summary table
unique_items = purchase_data_df["Item Name"].nunique()
avg_pur_price = purchase_data_df["Price"].mean()
total_purcahse = purchase_data_df["Price"].count()
total_rev = purchase_data_df["Price"].sum()

#Create summary table for purchasing analysis
summary_df = pd.DataFrame ({"Unique Items": [unique_items], 
                           "Average Purchase Price": [avg_pur_price], 
                           "Number of Purchases": [total_purcahse], 
                           "Total Revenue": [total_rev]})

#Arrange columns in desired order and print summary table
summary_df_2 = summary_df[["Unique Items", "Average Purchase Price", "Number of Purchases", "Total Revenue"]]
summary_df_2

summary_df_2.style.format({"Average Purchase Price": "${:.2f}", "Total Revenue": "${:.2f}"})
```




<style  type="text/css" >
</style>  
<table id="T_85f03574_10c2_11e8_b6bf_060088ba5501" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Unique Items</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Number of Purchases</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_85f03574_10c2_11e8_b6bf_060088ba5501level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_85f03574_10c2_11e8_b6bf_060088ba5501row0_col0" class="data row0 col0" >179</td> 
        <td id="T_85f03574_10c2_11e8_b6bf_060088ba5501row0_col1" class="data row0 col1" >$2.93</td> 
        <td id="T_85f03574_10c2_11e8_b6bf_060088ba5501row0_col2" class="data row0 col2" >780</td> 
        <td id="T_85f03574_10c2_11e8_b6bf_060088ba5501row0_col3" class="data row0 col3" >$2286.33</td> 
    </tr></tbody> 
</table> 




```python
#Gender Analytics
total_gender = purchase_data_df["Gender"].count()
male = purchase_data_df["Gender"].value_counts()['Male']
female = purchase_data_df["Gender"].value_counts()['Female']
other = purchase_data_df["Gender"].value_counts()['Other / Non-Disclosed']
male_percent = (male/total_gender) * 100
female_percent = (female/total_gender) * 100
other_percent = (other/total_gender) * 100

#Create gender analytics summary table
summary_gender_df = pd.DataFrame ({"Percentage of Players": [male_percent, female_percent, other_percent], 
                                  "Total Count": [male, female, other]}, index = ["Male", "Female", "Non-Specific"])

summary_gender_df.style.format({"Percentage of Players": "%{:.2f}"})

summary_gender_df
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.153846</td>
      <td>633</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.435897</td>
      <td>136</td>
    </tr>
    <tr>
      <th>Non-Specific</th>
      <td>1.410256</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis by Gender

pur_gender = purchase_data_df.groupby(["Gender"])

pur_count_gender = pur_gender["SN"].count()

avg_pur_price = pur_gender["Price"].mean()

tot_pur_value = pur_gender["Price"].sum()

duplicate_data = purchase_data_df.drop_duplicates(subset='SN', keep="first")
grouped_duplicate_data = duplicate_data.groupby(["Gender"])

norm_total = (pur_gender["Price"].sum() / grouped_duplicate_data["SN"].count())

summary_analysis_gender_df = pd.DataFrame({ "Purchase Count":pur_count_gender, 
                                            "Average Purchase Price": avg_pur_price, 
                                            "Total Purchase Value": tot_pur_value, 
                                            "Normalized Total": norm_total })

summary_analysis_gender_df

summary_analysis_gender_df.style.format({"Total Purchase Value": '${:.2f}', "Average Purchase Price": '${:.2f}', "Normalized Total": '${:.2f}'})    

```




<style  type="text/css" >
</style>  
<table id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Purchase Price</th> 
        <th class="col_heading level0 col1" >Normalized Total</th> 
        <th class="col_heading level0 col2" >Purchase Count</th> 
        <th class="col_heading level0 col3" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501level0_row0" class="row_heading level0 row0" >Female</th> 
        <td id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501row0_col0" class="data row0 col0" >$2.82</td> 
        <td id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501row0_col1" class="data row0 col1" >$3.83</td> 
        <td id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501row0_col2" class="data row0 col2" >136</td> 
        <td id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501row0_col3" class="data row0 col3" >$382.91</td> 
    </tr>    <tr> 
        <th id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501level0_row1" class="row_heading level0 row1" >Male</th> 
        <td id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501row1_col0" class="data row1 col0" >$2.95</td> 
        <td id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501row1_col1" class="data row1 col1" >$4.02</td> 
        <td id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501row1_col2" class="data row1 col2" >633</td> 
        <td id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501row1_col3" class="data row1 col3" >$1867.68</td> 
    </tr>    <tr> 
        <th id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501row2_col0" class="data row2 col0" >$3.25</td> 
        <td id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501row2_col1" class="data row2 col1" >$4.47</td> 
        <td id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501row2_col2" class="data row2 col2" >11</td> 
        <td id="T_85fa3aba_10c2_11e8_bc8d_060088ba5501row2_col3" class="data row2 col3" >$35.74</td> 
    </tr></tbody> 
</table> 




```python
#Age Demographics by Binning

bins = [0,10,15,20,25,30,35,40,200]
bins_list = ["< 10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

#Bin dataframe
bin_df = purchase_data_df.copy()
bin_df["Age Groups"] = pd.cut(bin_df["Age"], bins, labels =bins_list)
bin_cut = pd.cut(bin_df["Age"], bins, labels = bins_list)
grouped_bin_df = bin_df.groupby(["Age Groups"])

pur_count_age = grouped_bin_df["Age"].count()
avg_price_age = grouped_bin_df["Price"].mean()
tot_pur_age = grouped_bin_df["Price"].sum() 

duplicate = purchase_data_df.drop_duplicates(subset = 'SN', keep = "first")
duplicate["Age Groups"] = pd.cut(duplicate["Age"], bins, labels = bins_list)
duplicate = duplicate.groupby(["Age Groups"])

norm_total_bin_df = (grouped_bin_df["Price"].sum()/duplicate["SN"].count())
norm_total_bin_df

age_bin_df = pd.DataFrame({ "Purchase Count": pur_count_age,
                            "Average Purchase Price": avg_price_age,
                            "Total Purchase Value": tot_pur_age,
                            "Normalized Total": norm_total_bin_df})

age_bin_df

age_bin_df.style.format({"Total Purchase Value": '${:.2f}', "Average Purchase Price": '${:.2f}', "Normalized Total": '${:.2f}'})    

```

    /anaconda3/envs/PythonData/lib/python3.6/site-packages/ipykernel_launcher.py:17: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy





<style  type="text/css" >
</style>  
<table id="T_8608a9d8_10c2_11e8_8321_060088ba5501" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Purchase Price</th> 
        <th class="col_heading level0 col1" >Normalized Total</th> 
        <th class="col_heading level0 col2" >Purchase Count</th> 
        <th class="col_heading level0 col3" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Age Groups</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_8608a9d8_10c2_11e8_8321_060088ba5501level0_row0" class="row_heading level0 row0" >< 10</th> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row0_col0" class="data row0 col0" >$3.02</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row0_col1" class="data row0 col1" >$4.39</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row0_col2" class="data row0 col2" >32</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row0_col3" class="data row0 col3" >$96.62</td> 
    </tr>    <tr> 
        <th id="T_8608a9d8_10c2_11e8_8321_060088ba5501level0_row1" class="row_heading level0 row1" >10-14</th> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row1_col0" class="data row1 col0" >$2.87</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row1_col1" class="data row1 col1" >$4.15</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row1_col2" class="data row1 col2" >78</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row1_col3" class="data row1 col3" >$224.15</td> 
    </tr>    <tr> 
        <th id="T_8608a9d8_10c2_11e8_8321_060088ba5501level0_row2" class="row_heading level0 row2" >15-19</th> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row2_col0" class="data row2 col0" >$2.87</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row2_col1" class="data row2 col1" >$3.80</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row2_col2" class="data row2 col2" >184</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row2_col3" class="data row2 col3" >$528.74</td> 
    </tr>    <tr> 
        <th id="T_8608a9d8_10c2_11e8_8321_060088ba5501level0_row3" class="row_heading level0 row3" >20-24</th> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row3_col0" class="data row3 col0" >$2.96</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row3_col1" class="data row3 col1" >$3.86</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row3_col2" class="data row3 col2" >305</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row3_col3" class="data row3 col3" >$902.61</td> 
    </tr>    <tr> 
        <th id="T_8608a9d8_10c2_11e8_8321_060088ba5501level0_row4" class="row_heading level0 row4" >25-29</th> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row4_col0" class="data row4 col0" >$2.89</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row4_col1" class="data row4 col1" >$4.23</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row4_col2" class="data row4 col2" >76</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row4_col3" class="data row4 col3" >$219.82</td> 
    </tr>    <tr> 
        <th id="T_8608a9d8_10c2_11e8_8321_060088ba5501level0_row5" class="row_heading level0 row5" >30-34</th> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row5_col0" class="data row5 col0" >$3.07</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row5_col1" class="data row5 col1" >$4.05</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row5_col2" class="data row5 col2" >58</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row5_col3" class="data row5 col3" >$178.26</td> 
    </tr>    <tr> 
        <th id="T_8608a9d8_10c2_11e8_8321_060088ba5501level0_row6" class="row_heading level0 row6" >35-39</th> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row6_col0" class="data row6 col0" >$2.90</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row6_col1" class="data row6 col1" >$5.10</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row6_col2" class="data row6 col2" >44</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row6_col3" class="data row6 col3" >$127.49</td> 
    </tr>    <tr> 
        <th id="T_8608a9d8_10c2_11e8_8321_060088ba5501level0_row7" class="row_heading level0 row7" >40+</th> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row7_col0" class="data row7 col0" >$2.88</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row7_col1" class="data row7 col1" >$2.88</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row7_col2" class="data row7 col2" >3</td> 
        <td id="T_8608a9d8_10c2_11e8_8321_060088ba5501row7_col3" class="data row7 col3" >$8.64</td> 
    </tr></tbody> 
</table> 




```python
#Top Spenders

SN_group = purchase_data_df.groupby(["SN"])

SN_group_count = SN_group["Item ID"].count()
SN_group_total = SN_group["Price"].sum()
SN_group_Average = (SN_group_total/SN_group_count)

spender_df = pd.DataFrame({"Purchase Count": SN_group_count, 
                      "Average Purchase Price": SN_group_Average,
                      "Total Purchase Value":SN_group_total})

spender_df = spender_df.sort_values("Total Purchase Value", ascending = False)

spender_df.style.format({"Total Purchase Value": '${:.2f}', "Average Purchase Price": '${:.2f}'})

spender_df.head(5)
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
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>3.412000</td>
      <td>5</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>3.390000</td>
      <td>4</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>3.185000</td>
      <td>4</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>4.243333</td>
      <td>3</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3.860000</td>
      <td>3</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular Items

top5_items_ID = pd.DataFrame(purchase_data_df.groupby("Item ID")["Item ID"].count())

top5_items_ID.sort_values("Item ID", ascending = False, inplace = True)

top5_items_ID = top5_items_ID.iloc[0:5][:]

top5_items_total = pd.DataFrame(purchase_data_df.groupby("Item ID")["Price"].sum())

top5_items = pd.merge(top5_items_ID, top5_items_total, left_index = True, right_index = True)

no_dup_items = purchase_data_df.drop_duplicates(["Item ID"], keep = 'last')

top5_merge_ID = pd.merge(top5_items, no_dup_items, left_index = True, right_on = "Item ID")

top5_merge_ID = top5_merge_ID[["Item ID", "Item Name", "Item ID_x", "Price_y", "Price_x"]]

top5_merge_ID.set_index(["Item ID"], inplace = True)

top5_merge_ID.rename(columns =  {"Item ID_x": "Purchase Count", 
                                 "Price_y": "Item Price", 
                                 "Price_x": "Total Purchase Value"}, inplace=True)

top5_merge_ID.style.format({"Item Price": '${:.2f}', "Total Purchase Value": '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_86193f14_10c2_11e8_a313_060088ba5501" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Item Name</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Item Price</th> 
        <th class="col_heading level0 col3" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_86193f14_10c2_11e8_a313_060088ba5501level0_row0" class="row_heading level0 row0" >39</th> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row0_col0" class="data row0 col0" >Betrayal, Whisper of Grieving Widows</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row0_col1" class="data row0 col1" >11</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row0_col2" class="data row0 col2" >$2.35</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row0_col3" class="data row0 col3" >$25.85</td> 
    </tr>    <tr> 
        <th id="T_86193f14_10c2_11e8_a313_060088ba5501level0_row1" class="row_heading level0 row1" >84</th> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row1_col0" class="data row1 col0" >Arcane Gem</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row1_col1" class="data row1 col1" >11</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row1_col2" class="data row1 col2" >$2.23</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row1_col3" class="data row1 col3" >$24.53</td> 
    </tr>    <tr> 
        <th id="T_86193f14_10c2_11e8_a313_060088ba5501level0_row2" class="row_heading level0 row2" >31</th> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row2_col0" class="data row2 col0" >Trickster</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row2_col1" class="data row2 col1" >9</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row2_col2" class="data row2 col2" >$2.07</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row2_col3" class="data row2 col3" >$18.63</td> 
    </tr>    <tr> 
        <th id="T_86193f14_10c2_11e8_a313_060088ba5501level0_row3" class="row_heading level0 row3" >175</th> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row3_col0" class="data row3 col0" >Woeful Adamantite Claymore</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row3_col1" class="data row3 col1" >9</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row3_col2" class="data row3 col2" >$1.24</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row3_col3" class="data row3 col3" >$11.16</td> 
    </tr>    <tr> 
        <th id="T_86193f14_10c2_11e8_a313_060088ba5501level0_row4" class="row_heading level0 row4" >13</th> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row4_col0" class="data row4 col0" >Serenity</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row4_col1" class="data row4 col1" >9</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row4_col2" class="data row4 col2" >$1.49</td> 
        <td id="T_86193f14_10c2_11e8_a313_060088ba5501row4_col3" class="data row4 col3" >$13.41</td> 
    </tr></tbody> 
</table> 




```python
#Most Profitable

top5_profit = pd.DataFrame(purchase_data_df.groupby("Item ID")["Price"].sum())
top5_profit.sort_values("Price", ascending = False, inplace = True)

top5_profit = top5_profit.iloc[0:5][:]

pur_count_profit = pd.DataFrame(purchase_data_df.groupby("Item ID")["Item ID"].count())

top5_profit = pd.merge(top5_profit, pur_count_profit, left_index = True, right_index = True, how = 'left')
top5_merge_profit = pd.merge(top5_profit, no_dup_items, left_index = True, right_on = "Item ID", how = 'left')
top5_merge_profit = top5_merge_profit[["Item ID", "Item Name", "Item ID_x", "Price_y","Price_x"]]
top5_merge_profit.set_index(["Item ID"], inplace=True)
top5_merge_profit.rename(columns = {"Item ID_x": "Purchase Count", 
                                    "Price_y": "Item Price", 
                                    "Price_x": "Total Purchase Value"}, inplace = True)
top5_merge_profit.style.format({"Item Price": '${:.2f}', "Total Purchase Value": '${:.2f}'})
```




<style  type="text/css" >
</style>  
<table id="T_8624005c_10c2_11e8_9ff0_060088ba5501" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Item Name</th> 
        <th class="col_heading level0 col1" >Purchase Count</th> 
        <th class="col_heading level0 col2" >Item Price</th> 
        <th class="col_heading level0 col3" >Total Purchase Value</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Item ID</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_8624005c_10c2_11e8_9ff0_060088ba5501level0_row0" class="row_heading level0 row0" >34</th> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row0_col0" class="data row0 col0" >Retribution Axe</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row0_col1" class="data row0 col1" >9</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row0_col2" class="data row0 col2" >$4.14</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row0_col3" class="data row0 col3" >$37.26</td> 
    </tr>    <tr> 
        <th id="T_8624005c_10c2_11e8_9ff0_060088ba5501level0_row1" class="row_heading level0 row1" >115</th> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row1_col0" class="data row1 col0" >Spectral Diamond Doomblade</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row1_col1" class="data row1 col1" >7</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row1_col2" class="data row1 col2" >$4.25</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row1_col3" class="data row1 col3" >$29.75</td> 
    </tr>    <tr> 
        <th id="T_8624005c_10c2_11e8_9ff0_060088ba5501level0_row2" class="row_heading level0 row2" >32</th> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row2_col0" class="data row2 col0" >Orenmir</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row2_col1" class="data row2 col1" >6</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row2_col2" class="data row2 col2" >$4.95</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row2_col3" class="data row2 col3" >$29.70</td> 
    </tr>    <tr> 
        <th id="T_8624005c_10c2_11e8_9ff0_060088ba5501level0_row3" class="row_heading level0 row3" >103</th> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row3_col0" class="data row3 col0" >Singed Scalpel</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row3_col1" class="data row3 col1" >6</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row3_col2" class="data row3 col2" >$4.87</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row3_col3" class="data row3 col3" >$29.22</td> 
    </tr>    <tr> 
        <th id="T_8624005c_10c2_11e8_9ff0_060088ba5501level0_row4" class="row_heading level0 row4" >107</th> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row4_col0" class="data row4 col0" >Splitter, Foe Of Subtlety</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row4_col1" class="data row4 col1" >8</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row4_col2" class="data row4 col2" >$3.61</td> 
        <td id="T_8624005c_10c2_11e8_9ff0_060088ba5501row4_col3" class="data row4 col3" >$28.88</td> 
    </tr></tbody> 
</table> 


