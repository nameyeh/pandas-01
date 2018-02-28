
# Heroes Of Pymoli Data Analysis
## Observed Trends 
* As seen in the gender demographics table, the player population is overwhelming male. 
* Because there are more male players, the total purchase value for males is higher than females; however, the normalized totals indicate that males and females spent roughly the same amount. 
* The age distribution shows that the majority of players were between the ages of 15-29, with about half between the ages of 20-24.  


```python
# Import dependencies
import pandas as pd
import numpy as np 

# Reference to CSV and read CSV into Pandas DataFrame
csv_path = "purchase_data.json"
df = pd.read_json(csv_path)
#df.head(10)
```

# Player Count




```python
#Total Number of Players
n_players = len(df["SN"].unique())
n_players
players = pd.DataFrame({"Total Players (Unique)":[n_players]})
players
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
      <th>Total Players (Unique)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>



# Purchasing Analysis (Total)
* Number of Unique Items
* Average Purchase Price
* Total Number of Purchases
* Total Revenue


```python
# Number of Unique Items
df["Item ID"].unique()
unique_items = len(df["Item ID"].unique())
#print(unique_items)
#--------------------
# Average Purchase Price
avg_price = df["Price"].mean()
#print(avg_price)
#--------------------
# Total Number of Purchases
n_purchases = df["Price"].count()
#print(n_purchases) 
#--------------------
# Total Revenue
total_rev = df["Price"].sum()
#print(total_rev)
#--------------------
#Print Purchasing Analysis (Total)table 
purch_table = pd.DataFrame({
    "Number of Unique Items":[unique_items],\
    "Average Price": [avg_price],\
    "Number of Purchases": [n_purchases],\
    "Total Revenue": [total_rev]
})

# Reorganizing the columns using double brackets
org_purch_table = purch_table[["Number of Unique Items","Average Price","Number of Purchases","Total Revenue"]]
org_purch_table
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>2.931192</td>
      <td>780</td>
      <td>2286.33</td>
    </tr>
  </tbody>
</table>
</div>



# Gender Demographics
* Percentage and Count of Male Players
* Percentage and Count of Female Players
* Percentage and Count of Other / Non-Disclosed


```python
# Extract unique players 
df2 = df.drop_duplicates('SN')
#df2.count()
#------------------------------------------
# print(df2["Gender"].value_counts())
# print("--------------------------")
# print(df2["Gender"].count())
#------------------------------------------
male = df2["Gender"].value_counts()['Male']
# print(("Male count: " )+ str(male))

male_pct = round((male/(df2["Gender"].count()))*100,1)
# male_pct
# print(("Male %: " )+ str(male_pct))
# print("--------------------------")

female = df2["Gender"].value_counts()['Female']
# print(("Female count: " )+ str(female))

female_pct = round((female/(df2["Gender"].count()))*100,1)
# female_pct
# print(("Female %: " )+ str(female_pct))
# print("--------------------------")

other_g = df2["Gender"].value_counts()['Other / Non-Disclosed']
# print(("Other gender count: " )+ str(other_g))

other_g_pct = round((other_g/(df2["Gender"].count()))*100,1)
# other_g_pct
# print(("Other gender %: " )+ str(other_g_pct))
#--------------------------------------------------------------------------------
# Print Gender Demographics Table 
gender_demo = pd.DataFrame({
    "Gender":["Male","Female","Other / Non-Disclosed"],
    "Percentage of Players": [male_pct, female_pct, other_g_pct],
    "Total Count": [male, female, other_g], 
})
gender_demo
#set index
gender_demo2 = gender_demo.set_index("Gender")
gender_demo2
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
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.2</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.5</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.4</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



# Purchasing Analysis (broken down by gender)

* Purchase Count
* Average Purchase Price
* Total Purchase Value
* Normalized Totals


```python
## use df2 for unique players 
# df2.count()
# df2.head()
# Purchase Count by gender 

purch_ct_df = pd.DataFrame({
    "Gender":["Male","Female","Other / Non-Disclosed"],
    "Purchase Count": [male,female,other_g]
})
purch_ct_df
#---------------------------------------------------------------------------------
# groupby gender
grp_gender = df2.groupby(['Gender'])

#Total Purchase Value by gender 
grp_purch = grp_gender['Price'].sum()
# print(grp_purch)

purch_f = grp_gender['Price'].sum()["Female"]
# print(purch_f)

purch_m = grp_gender['Price'].sum()["Male"]
# print(purch_m)

purch_o = grp_gender['Price'].sum()["Other / Non-Disclosed"]
# print(purch_o)
#---------------------------------------------------------------------------------
# Average Purchase Price by gender 
grp_avgp = grp_gender['Price'].mean()
# print(grp_avgp)

avgp_f = (grp_gender['Price'].mean()["Female"])
# print(avgp_f)

avgp_m = grp_gender['Price'].mean()["Male"]
# print(avgp_m)

avgp_o = grp_gender['Price'].mean()["Other / Non-Disclosed"]
# print(avgp_o)
#---------------------------------------------------------------------------------
# Normalized Totals 
norm_total_m = round((purch_m/male), 2)
norm_total_f = round((purch_f/female), 2)
norm_total_o = round((purch_o/other_g), 2)
#---------------------------------------------------------------------------------
# Print Purchasing Analysis (Gender) Table 
gender_purch = pd.DataFrame({
    "Gender":["Male","Female","Other / Non-Disclosed"],
    "Purchase Count": [male,female,other_g],
    "Average Purchase Price": [avgp_m,avgp_f,avgp_o], 
    "Total Purchase Value": [purch_m, purch_f, purch_o],
    "Normalized Totals": [norm_total_m,norm_total_f,norm_total_o]
})
gender_purch

# Reorganizing the columns using double brackets
gender_purch = gender_purch[["Gender","Purchase Count","Average Purchase Price",\
                             "Total Purchase Value","Normalized Totals"]]
gender_purch
# set index 
gender_purch2 = gender_purch.set_index("Gender")
gender_purch2
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>465</td>
      <td>2.988645</td>
      <td>1389.72</td>
      <td>2.99</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>100</td>
      <td>2.887900</td>
      <td>288.79</td>
      <td>2.89</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>8</td>
      <td>3.447500</td>
      <td>27.58</td>
      <td>3.45</td>
    </tr>
  </tbody>
</table>
</div>



# Age Demographics (broken into bins of 4 years)
* Purchase Count
* Average Purchase Price
* Total Purchase Value
* Normalized Totals


```python
#df2["Age"].max()
bins = [0, 9, 14, 19, 24, 29, 34, 39, 45]
bin_names = ["<10", "10-14", "15-19","20-24","25-29","30-34","35-39","40-45"]
df4 = df2

# Cut Age and place into bins
df4["Age Summary"] = pd.cut(df4["Age"], bins, labels=bin_names)
# df4.head()
# df4.count()

#groupby age 
age_grp = df4.groupby(['Age Summary'])
#see age distribution across unique players 
age_grp.count()["SN"]
#----------------------------------------------------------
age_grp.count()["Item Name"]

ct_agegrp1 = age_grp['Item Name'].count()["<10"]
# print(ct_agegrp1)
ct_agegrp1_pct = round((ct_agegrp1/573)*100,1)
# print(ct_agegrp1_pct)
# print("-----------------")

ct_agegrp2 = age_grp['Item Name'].count()["10-14"]
# print(ct_agegrp2)
ct_agegrp2_pct = round((ct_agegrp2/573)*100,1)
# print(ct_agegrp2_pct)
# print("-----------------")

ct_agegrp3 = age_grp['Item Name'].count()["15-19"]
# print(ct_agegrp3)
ct_agegrp3_pct = round((ct_agegrp3/573)*100,1)
# print(ct_agegrp3_pct)
# print("-----------------")

ct_agegrp4 = age_grp['Item Name'].count()["20-24"]
# print(ct_agegrp4)
ct_agegrp4_pct = round((ct_agegrp4/573)*100,1)
# print(ct_agegrp4_pct)
# print("-----------------")

ct_agegrp5 = age_grp['Item Name'].count()["25-29"]
# print(ct_agegrp5)
ct_agegrp5_pct = round((ct_agegrp5/573)*100,1)
# print(ct_agegrp5_pct)
# print("-----------------")

ct_agegrp6 = age_grp['Item Name'].count()["30-34"]
# print(ct_agegrp6)
ct_agegrp6_pct = round((ct_agegrp6/573)*100,1)
# print(ct_agegrp6_pct)
# print("-----------------")

ct_agegrp7 = age_grp['Item Name'].count()["35-39"]
# print(ct_agegrp7)
ct_agegrp7_pct = round((ct_agegrp7/573)*100,1)
# print(ct_agegrp7_pct)
# print("-----------------")

ct_agegrp8 = age_grp['Item Name'].count()["40-45"]
# print(ct_agegrp8)
ct_agegrp8_pct = round((ct_agegrp8/573)*100,1)
# print(ct_agegrp8_pct)
# print("-----------------")
# -------------------------------------------------------------------------------------------
#Print Age Table 
age_tbl = pd.DataFrame({
    "Age Summary":["<10", "10-14", "15-19","20-24","25-29","30-34","35-39","40-45"],
    "Percentage of Players": [ct_agegrp1_pct,ct_agegrp2_pct,ct_agegrp3_pct,ct_agegrp4_pct,ct_agegrp5_pct,ct_agegrp6_pct,ct_agegrp7_pct,ct_agegrp8_pct],
    "Total Count": [ct_agegrp1,ct_agegrp2,ct_agegrp3,ct_agegrp4,ct_agegrp5,ct_agegrp6,ct_agegrp7,ct_agegrp8],
})
age_tbl
#-----------------------------------------
age_tbl2 = age_tbl.set_index("Age Summary")
age_tbl2
```

    C:\Users\nab226\AppData\Local\Continuum\anaconda3\envs\PythonData\lib\site-packages\ipykernel_launcher.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      import sys
    




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
    <tr>
      <th>Age Summary</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>3.3</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>4.0</td>
      <td>23</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.5</td>
      <td>100</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>45.2</td>
      <td>259</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>15.2</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.2</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>4.7</td>
      <td>27</td>
    </tr>
    <tr>
      <th>40-45</th>
      <td>1.9</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Total Purchase Value by age 
age_purch = age_grp['Price'].sum()
# print(age_purch)

purch_0_9 = age_grp['Price'].sum()["<10"]
# print(purch_0_9)

purch_10_14 = age_grp['Price'].sum()["10-14"]
# print(purch_10_14)

purch_15_19 = age_grp['Price'].sum()["15-19"]
# print(purch_15_19)

purch_20_24 = age_grp['Price'].sum()["20-24"]
# print(purch_20_24)

purch_25_29 = age_grp['Price'].sum()["25-29"]
# print(purch_25_29)

purch_30_34 = age_grp['Price'].sum()["30-34"]
# print(purch_30_34)

purch_35_39 = age_grp['Price'].sum()["35-39"]
# print(purch_35_39)

purch_40_45 = age_grp['Price'].sum()["40-45"]
# print(purch_40_45)
#----------------------------------------------
#Average Purchase Price by age 
agegrp_avg = age_grp['Price'].mean()
# print(agegrp_avg)

agegrp1_avg = age_grp['Price'].mean()["<10"]
# print(agegrp1_avg)

agegrp2_avg = age_grp['Price'].mean()["10-14"]
# print(agegrp2_avg)

agegrp3_avg = age_grp['Price'].mean()["15-19"]
# print(agegrp3_avg)

agegrp4_avg = age_grp['Price'].mean()["20-24"]
# print(agegrp4_avg)

agegrp5_avg = age_grp['Price'].mean()["25-29"]
# print(agegrp5_avg)

agegrp6_avg = age_grp['Price'].mean()["30-34"]
# print(agegrp6_avg)

agegrp7_avg = age_grp['Price'].mean()["35-39"]
# print(agegrp7_avg)

agegrp8_avg = age_grp['Price'].mean()["40-45"]
# print(agegrp8_avg)

# Normalized Totals 
norm_total_agegrp1 = round((purch_0_9/ct_agegrp1), 2)
norm_total_agegrp2 = round((purch_10_14/ct_agegrp2), 2)
norm_total_agegrp3 = round((purch_15_19/ct_agegrp3), 2)

norm_total_agegrp4 = round((purch_20_24/ct_agegrp4), 2)
norm_total_agegrp5 = round((purch_25_29/ct_agegrp5), 2)
norm_total_agegrp6 = round((purch_30_34/ct_agegrp6), 2)
norm_total_agegrp7 = round((purch_35_39/ct_agegrp7), 2)
norm_total_agegrp8 = round((purch_40_45/ct_agegrp8), 2)

#---------------------------------------------------------------------------------
#Print Purchase by Age Table 
age_ptable = pd.DataFrame({
    "Age Summary":["<10", "10-14", "15-19","20-24","25-29","30-34","35-39","40-45"],
    "Purchase Count": [ct_agegrp1,ct_agegrp2,ct_agegrp3,ct_agegrp4,ct_agegrp5,ct_agegrp6,ct_agegrp7,ct_agegrp8],
    "Average Purchase Price": [agegrp1_avg,agegrp2_avg,agegrp3_avg,agegrp4_avg,agegrp5_avg,agegrp6_avg,agegrp7_avg,agegrp8_avg], 
    "Total Purchase Value": [purch_0_9,purch_10_14,purch_15_19,purch_20_24,purch_25_29,purch_30_34,purch_35_39,purch_40_45],
    "Normalized Totals": [norm_total_agegrp1,norm_total_agegrp2,norm_total_agegrp3,\
                         norm_total_agegrp4,norm_total_agegrp5,norm_total_agegrp6,\
                         norm_total_agegrp7,norm_total_agegrp8]
})
age_ptable

# Reorganizing the columns using double brackets
age_ptable = age_ptable[["Age Summary","Purchase Count","Average Purchase Price",\
                         "Total Purchase Value","Normalized Totals"]]
age_ptable

age_ptable2 = age_ptable.set_index("Age Summary")
age_ptable2
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Age Summary</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>19</td>
      <td>3.128947</td>
      <td>59.45</td>
      <td>3.13</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>23</td>
      <td>2.697391</td>
      <td>62.04</td>
      <td>2.70</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>100</td>
      <td>2.898800</td>
      <td>289.88</td>
      <td>2.90</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>259</td>
      <td>2.956332</td>
      <td>765.69</td>
      <td>2.96</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>87</td>
      <td>3.029080</td>
      <td>263.53</td>
      <td>3.03</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>47</td>
      <td>3.246809</td>
      <td>152.60</td>
      <td>3.25</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>27</td>
      <td>2.912963</td>
      <td>78.65</td>
      <td>2.91</td>
    </tr>
    <tr>
      <th>40-45</th>
      <td>11</td>
      <td>3.113636</td>
      <td>34.25</td>
      <td>3.11</td>
    </tr>
  </tbody>
</table>
</div>



# Top Spenders 
* Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
    + SN
    + Purchase Count
    + Average Purchase Price
    + Total Purchase Value
    


```python
# df.head(10)

# Grouping the DataFrame by "SN"
sn_grp = df.groupby("SN")
#sn_grp.count()

# Sum the prices for grouped SN and assign to dataframe  
sn_grp_prices = pd.DataFrame(sn_grp["Price"].sum())
sn_grp_prices.head(10)
# sort by price 
sn_grp_prices_sorted = sn_grp_prices.sort_values(["Price"], ascending=False)
sn_grp_prices_sorted
# extract top 5 
top5 = sn_grp_prices_sorted.head(5)
top5
#rename "price" to "total spent" 
top5_renamed = top5.rename(columns={"Price":"Total Spent"})
top5_renamed
# reset index 
top5_renamed2 = top5_renamed.reset_index()
top5_renamed2
#then merge top5 with full dataset 
merge_table = pd.merge(top5_renamed2, df, how="inner")
merge_table
#---------------------------------------------------------------
SN_purch_ct = pd.DataFrame(merge_table.groupby("SN").count()["Item Name"])
SN_purch_ct
#rename 
SN_purch_ct = SN_purch_ct.rename(columns={"Item Name":"Purchase Count"})
SN_purch_ct
# reset index for merge 
SN_purch_ct = SN_purch_ct.reset_index()
SN_purch_ct
#merge 
merge2 = pd.merge(SN_purch_ct, df, how="inner")
merge2
#Total Purchase Value by SN 
m2a = merge2.groupby("SN")

SN_purch = m2a['Price'].sum()
# print(SN_purch)

SN_purch_df = pd.DataFrame(SN_purch)
SN_purch_df

SN_purch_df2 = SN_purch_df.rename(columns={"Price":"Total Purchase Value"})
SN_purch_df2 = SN_purch_df2.reset_index()
SN_purch_df2

merge3 = pd.merge(SN_purch_df2, merge2, how="inner")
merge3

# Average Purchase Price by SN 
avg_purch_SN = merge3.groupby("SN").mean()["Price"]
# print(avg_purch_SN)
# assign to df 
avg_purch_SN_df = pd.DataFrame(avg_purch_SN)
avg_purch_SN_df
#rename column and reset index 
avg_purch_SN_df2 = avg_purch_SN_df.rename(columns={"Price":"Average Purchase Price"})
avg_purch_SN_df2 = avg_purch_SN_df2.reset_index()
avg_purch_SN_df2
# merge 
merge4 = pd.merge(avg_purch_SN_df2, merge3, how="inner")
merge4
# merge
merge5 = merge4.drop_duplicates(["SN"])
merge5

# Reorganizing the columns using double brackets
merge5 = merge5[["SN","Purchase Count","Average Purchase Price","Total Purchase Value"]]
merge5

merge6 = merge5.set_index("SN")
merge6 
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
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
      <th>Eoda93</th>
      <td>3</td>
      <td>3.860000</td>
      <td>11.58</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>4.243333</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>3.185000</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>3.390000</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Undirrala66</th>
      <td>5</td>
      <td>3.412000</td>
      <td>17.06</td>
    </tr>
  </tbody>
</table>
</div>



# Most Popular Items
* Identify the 5 most popular items by purchase count, then list (in a table):
    + Item ID
    + Item Name
    + Purchase Count
    + Item Price
    + Total Purchase Value


```python
# Grouping the DataFrame by "Item ID"
item_grp = df.groupby("Item ID")
item_grp.count()
# Sum the prices for grouped Item ID and assign to dataframe  

item_grp_ct = pd.DataFrame(item_grp["Item ID"].count())
item_grp_ct

item_grp_ct_sorted = item_grp_ct.sort_values(["Item ID"], ascending=False)
item_grp_ct_sorted

# # #rename 
item_grp_ct_sorted = item_grp_ct_sorted.rename(columns={"Item ID":"Purchase Count"})
item_grp_ct_sorted

item_grp_ct_sorted2 = item_grp_ct_sorted.reset_index()
#item_grp_ct_sorted2 

#merge purchase counts with original df 
merge_ct = pd.merge(item_grp_ct_sorted2 , df, how="inner")
#merge_ct

merge_ct2 = merge_ct.drop_duplicates("Item ID")
merge_ct3 = merge_ct2.head(5)
#merge_ct3 

#remove extra columns 
merge_ct4 = merge_ct3.drop(["Age","Gender","SN"], 1)
#merge_ct4

merge_ct5 = merge_ct4
merge_ct5["Total Purchase Value"] = merge_ct4["Purchase Count"]*merge_ct4["Price"]
#merge_ct5

#set index
merge_ct6 = merge_ct5.set_index(["Item ID"])
#merge_ct6 
#organize columns 
merge_ct6 = merge_ct6[["Item Name","Purchase Count","Price","Total Purchase Value"]]
merge_ct6
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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <td>Betrayal, Whisper of Grieving Widows</td>
      <td>11</td>
      <td>2.35</td>
      <td>25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Arcane Gem</td>
      <td>11</td>
      <td>2.23</td>
      <td>24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Trickster</td>
      <td>9</td>
      <td>2.07</td>
      <td>18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <td>Woeful Adamantite Claymore</td>
      <td>9</td>
      <td>1.24</td>
      <td>11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Serenity</td>
      <td>9</td>
      <td>1.49</td>
      <td>13.41</td>
    </tr>
  </tbody>
</table>
</div>



# Most Profitable Items
* Identify the 5 most profitable items by total purchase value, then list (in a table):
    + Item ID
    + Item Name
    + Purchase Count
    + Item Price
    + Total Purchase Value


```python
#df.head(10)
# Grouping the DataFrame by "Item ID"
item_grp = df.groupby("Item ID")

# Sum the prices for grouped SN and assign to dataframe  

item_grp_prices = pd.DataFrame(item_grp["Item ID"].sum())
item_grp_prices.head(10)

item_grp_prices_sorted = item_grp_prices.sort_values(["Item ID"], ascending=False)
item_grp_prices_sorted


# #rename "price" to "total spent" 
item_grp_prices_sorted2 = item_grp_prices_sorted.rename(columns={"Item ID":"Purchase Count"})
item_grp_prices_sorted2

item_grp_prices_sorted3 = item_grp_prices_sorted2.reset_index()


# #then merge top5 with the full dataset 
item_grp_prices_sorted3 = pd.merge(item_grp_prices_sorted3, df, how="inner")
item_grp_prices_sorted3 
#drop duplicates in ITEM ID 
item_grp_prices_sorted3 = item_grp_prices_sorted3.drop_duplicates("Item ID")
item_grp_prices_sorted3

item_grp_prices_sorted4 = item_grp_prices_sorted3.head(5)
item_grp_prices_sorted4
#-------------------------------------------------------------------------------
#remove extra columns 
item_grp_prices_sorted5 = item_grp_prices_sorted4.drop(["Age","Gender","SN"], 1)
item_grp_prices_sorted5

item_grp_prices_sorted6 = item_grp_prices_sorted5
item_grp_prices_sorted6["Total Purchase Value"] = item_grp_prices_sorted6["Purchase Count"]*item_grp_prices_sorted6["Price"]
item_grp_prices_sorted6

#set index
item_grp_prices_sorted7 = item_grp_prices_sorted6.set_index(["Item ID"])
item_grp_prices_sorted7
#organize columns 
item_grp_prices_sorted7 = item_grp_prices_sorted7[["Item Name","Purchase Count","Price","Total Purchase Value"]]
item_grp_prices_sorted7
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
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>175</th>
      <td>Woeful Adamantite Claymore</td>
      <td>1575</td>
      <td>1.24</td>
      <td>1953.00</td>
    </tr>
    <tr>
      <th>179</th>
      <td>Wolf, Promise of the Moonwalker</td>
      <td>1253</td>
      <td>1.88</td>
      <td>2355.64</td>
    </tr>
    <tr>
      <th>152</th>
      <td>Darkheart</td>
      <td>1216</td>
      <td>3.15</td>
      <td>3830.40</td>
    </tr>
    <tr>
      <th>172</th>
      <td>Blade of the Grave</td>
      <td>1204</td>
      <td>1.69</td>
      <td>2034.76</td>
    </tr>
    <tr>
      <th>158</th>
      <td>Darkheart, Butcher of the Champion</td>
      <td>1106</td>
      <td>3.56</td>
      <td>3937.36</td>
    </tr>
  </tbody>
</table>
</div>


