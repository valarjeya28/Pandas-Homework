
### Heroes Of Pymoli Data Analysis
* Of the 1163 active players, the vast majority are male (84%). There also exists, a smaller, but notable proportion of female players (14%).

* Our peak age demographic falls between 20-24 (44.8%) with secondary groups falling between 15-19 (18.60%) and 25-29 (13.4%).  
-----

### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# File to Load (Remember to Change These)
file_to_load = "Resources/purchase_data.csv"

# Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file_to_load)


purchase_data.columns
purchase_data.head()
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
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Lisim78</td>
      <td>20</td>
      <td>Male</td>
      <td>108</td>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>3.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Lisovynya38</td>
      <td>40</td>
      <td>Male</td>
      <td>143</td>
      <td>Frenzied Scimitar</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Ithergue48</td>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Chamassasya86</td>
      <td>24</td>
      <td>Male</td>
      <td>100</td>
      <td>Blindscythe</td>
      <td>3.27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Iskosia90</td>
      <td>23</td>
      <td>Male</td>
      <td>131</td>
      <td>Fury</td>
      <td>1.44</td>
    </tr>
  </tbody>
</table>
</div>




```python
renamed_df=purchase_data.rename(columns={"Purchase ID":"Purchase_ID","SN":"Player_Name","Age":"Player_Age","Gender":"Player_Gender","Item ID":"Item_ID","Item Name":"Item_Name","Price":"Item_Price"})
renamed_df.columns

```




    Index(['Purchase_ID', 'Player_Name', 'Player_Age', 'Player_Gender', 'Item_ID',
           'Item_Name', 'Item_Price'],
          dtype='object')



## Player Count

* Display the total number of players



```python
#Display the total number of players
player_unique=(renamed_df["Player_Name"]).unique()
player_unique
Total=len(player_unique)
summary_totaldata=pd.DataFrame({"Total Players":[Total]})
summary_totaldata

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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
Items_unique=(renamed_df["Item_ID"]).unique()
print(len(Items_unique))
Avg_Price=renamed_df["Item_Price"].mean()
Avg_Price
Purchase_count=renamed_df["Purchase_ID"].count()
Purchase_count
revenue=renamed_df["Item_Price"].sum()
revenue

summary_data=pd.DataFrame({"Number of Unique Items":[len(Items_unique)],"Average Price":[Avg_Price],"Number of Purchases":[Purchase_count],"Total Revenue":[revenue]})
summary_data
summary_data.style.format({'Average Price': '${:.2f}','Total Revenue': '${:.2f}'})

```

    183





<style  type="text/css" >
</style>  
<table id="T_cfc15908_45c3_11e9_83f4_3af9d34bcbc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Number of Unique Items</th> 
        <th class="col_heading level0 col1" >Average Price</th> 
        <th class="col_heading level0 col2" >Number of Purchases</th> 
        <th class="col_heading level0 col3" >Total Revenue</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_cfc15908_45c3_11e9_83f4_3af9d34bcbc3level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_cfc15908_45c3_11e9_83f4_3af9d34bcbc3row0_col0" class="data row0 col0" >183</td> 
        <td id="T_cfc15908_45c3_11e9_83f4_3af9d34bcbc3row0_col1" class="data row0 col1" >$3.05</td> 
        <td id="T_cfc15908_45c3_11e9_83f4_3af9d34bcbc3row0_col2" class="data row0 col2" >780</td> 
        <td id="T_cfc15908_45c3_11e9_83f4_3af9d34bcbc3row0_col3" class="data row0 col3" >$2379.77</td> 
    </tr></tbody> 
</table> 



## Gender Demographics

* Percentage and Count of Male Players


* Percentage and Count of Female Players


* Percentage and Count of Other / Non-Disclosed





```python
#removing duplicates to get unique dataframe
remove_dup_players = renamed_df.drop_duplicates(['Player_Name'], keep ='last')
remove_dup_players

#Percentage and Count of  Players by gender
gender_counts = remove_dup_players['Player_Gender'].value_counts()
gender_counts


gender_percent = gender_counts/Total * 100
gender_percent

gender_data=pd.DataFrame({"Number of Players":gender_counts,"Percentage of Players":gender_percent })
gender_data


gender_data.style.format({"Percentage of Players": "{:.1f}%"})

```




<style  type="text/css" >
</style>  
<table id="T_cfc4a9f0_45c3_11e9_83f4_3af9d34bcbc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Number of Players</th> 
        <th class="col_heading level0 col1" >Percentage of Players</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_cfc4a9f0_45c3_11e9_83f4_3af9d34bcbc3level0_row0" class="row_heading level0 row0" >Male</th> 
        <td id="T_cfc4a9f0_45c3_11e9_83f4_3af9d34bcbc3row0_col0" class="data row0 col0" >484</td> 
        <td id="T_cfc4a9f0_45c3_11e9_83f4_3af9d34bcbc3row0_col1" class="data row0 col1" >84.0%</td> 
    </tr>    <tr> 
        <th id="T_cfc4a9f0_45c3_11e9_83f4_3af9d34bcbc3level0_row1" class="row_heading level0 row1" >Female</th> 
        <td id="T_cfc4a9f0_45c3_11e9_83f4_3af9d34bcbc3row1_col0" class="data row1 col0" >81</td> 
        <td id="T_cfc4a9f0_45c3_11e9_83f4_3af9d34bcbc3row1_col1" class="data row1 col1" >14.1%</td> 
    </tr>    <tr> 
        <th id="T_cfc4a9f0_45c3_11e9_83f4_3af9d34bcbc3level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_cfc4a9f0_45c3_11e9_83f4_3af9d34bcbc3row2_col0" class="data row2 col0" >11</td> 
        <td id="T_cfc4a9f0_45c3_11e9_83f4_3af9d34bcbc3row2_col1" class="data row2 col1" >1.9%</td> 
    </tr></tbody> 
</table> 




## Purchasing Analysis (Gender)

* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender




* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
#Run basic calculations to obtain
#purchase count avg. by gender
Purchase_count=renamed_df.groupby('Player_Gender')["Purchase_ID"].count()
Purchase_count

#, avg. purchase price  by gender
Purchase_Avg=renamed_df.groupby('Player_Gender')["Item_Price"].mean()
Purchase_Avg
# purchase total per person  by gender
Purchase_Total=renamed_df.groupby('Player_Gender')["Item_Price"].sum()
Purchase_Total

Purchase_Total_PP=Purchase_Total/gender_counts
Purchase_Total_PP
#Create a summary data frame to hold the results
Purchase_Data_Total=pd.DataFrame({"Purchase Count":Purchase_count,"Average Purchase Price":Purchase_Avg,
                            "Total Purchase Value":Purchase_Total,"Avg Total Purchase per Person":Purchase_Total_PP})
Purchase_Data_Total

#data cleaner formatting
Purchase_Data_Totalformat=Purchase_Data_Total.style.format({"Average Purchase Price": "${:.2f}","Total Purchase Value": "${:.2f}",
                            "Avg Total Purchase per Person": "${:.2f}"})
#Display the summary data frame
Purchase_Data_Totalformat

```




<style  type="text/css" >
</style>  
<table id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Purchase Count</th> 
        <th class="col_heading level0 col1" >Average Purchase Price</th> 
        <th class="col_heading level0 col2" >Total Purchase Value</th> 
        <th class="col_heading level0 col3" >Avg Total Purchase per Person</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Player_Gender</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3level0_row0" class="row_heading level0 row0" >Female</th> 
        <td id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3row0_col0" class="data row0 col0" >113</td> 
        <td id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3row0_col1" class="data row0 col1" >$3.20</td> 
        <td id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3row0_col2" class="data row0 col2" >$361.94</td> 
        <td id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3row0_col3" class="data row0 col3" >$4.47</td> 
    </tr>    <tr> 
        <th id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3level0_row1" class="row_heading level0 row1" >Male</th> 
        <td id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3row1_col0" class="data row1 col0" >652</td> 
        <td id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3row1_col1" class="data row1 col1" >$3.02</td> 
        <td id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3row1_col2" class="data row1 col2" >$1967.64</td> 
        <td id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3row1_col3" class="data row1 col3" >$4.07</td> 
    </tr>    <tr> 
        <th id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3level0_row2" class="row_heading level0 row2" >Other / Non-Disclosed</th> 
        <td id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3row2_col0" class="data row2 col0" >15</td> 
        <td id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3row2_col1" class="data row2 col1" >$3.35</td> 
        <td id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3row2_col2" class="data row2 col2" >$50.19</td> 
        <td id="T_cfc8550a_45c3_11e9_83f4_3af9d34bcbc3row2_col3" class="data row2 col3" >$4.56</td> 
    </tr></tbody> 
</table> 



## Age Demographics

* Establish bins for ages


* Categorize the existing players using the age bins. Hint: use pd.cut()


* Calculate the numbers and percentages by age group


* Create a summary data frame to hold the results


* Optional: round the percentage column to two decimal points


* Display Age Demographics Table



```python
#Establish bins for ages
bins = [0, 9,14, 19, 24,29,34,39,45]
age_group = ["<10", "10-14", "15-19", "20-24", "25-29","30-34","35-39","40+"]

renamed_df["Age_group"]= pd.cut(renamed_df["Player_Age"], bins, labels=age_group)
renamed_df["Age_group"]

#group by name and age

df_name_Age_Range = renamed_df.groupby(["Player_Name","Age_group"])
df_name_Age_Range

unique_players_df = pd.DataFrame(df_name_Age_Range.size())
unique_players_df
#Calculate the numbers and percentages by age group and round the percentage column to two decimal points
pymoli_agegrp_df_total= unique_players_df.groupby(["Age_group"]).count()
pymoli_agegrp_df_total.columns= ["Total Count"]
pymoli_agegrp_df_total["Percentage of Players"] =round(100 *pymoli_agegrp_df_total["Total Count"]/pymoli_agegrp_df_total["Total Count"].sum(),2)

pymoli_agegrp_df_total




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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Age_group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>17</td>
      <td>2.95</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>22</td>
      <td>3.82</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>107</td>
      <td>18.58</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>258</td>
      <td>44.79</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>77</td>
      <td>13.37</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>52</td>
      <td>9.03</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>31</td>
      <td>5.38</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>12</td>
      <td>2.08</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
#Bin the purchase_data data frame by age
renamed_df["Age_group"]
pymoli_agegrp_count_df = pd.DataFrame(renamed_df.groupby(["Age_group"]).count())
pymoli_agegrp_sum_df = pd.DataFrame(renamed_df.groupby(["Age_group"]).sum())
pymoli_agegrp_sum_df
#Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below
pymoli_agegrp_df_total["Purchase Count"] =  pymoli_agegrp_count_df["Item_ID"]


pymoli_agegrp_df_total["Average Purchase Price"] = round(pymoli_agegrp_sum_df["Item_Price"]/pymoli_agegrp_df_total["Purchase Count"],2).map("${:,.2f}".format)
pymoli_agegrp_df_total["Total Purchase Value"] = pymoli_agegrp_sum_df["Item_Price"]

pymoli_agegrp_df_total["Total per Person"] =  round(pymoli_agegrp_df_total["Total Purchase Value"]/pymoli_agegrp_df_total["Total Count"],2).map("${:,.2f}".format)

pymoli_agegrp_df_total["Total Purchase Value"] = pymoli_agegrp_df_total["Total Purchase Value"].map("${:,.2f}".format)
#Display the summary data frame
pymoli_agegrp_df_total_Summary=pymoli_agegrp_df_total[["Purchase Count","Average Purchase Price","Total Purchase Value","Total per Person"]]
pymoli_agegrp_df_total_Summary

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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Total per Person</th>
    </tr>
    <tr>
      <th>Age_group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$4.54</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$3.76</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1,114.06</td>
      <td>$4.32</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$3.81</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$4.76</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$3.19</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders

* Run basic calculations to obtain the results in the table below


* Create a summary data frame to hold the results


* Sort the total purchase value column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
pymoli_counts_df = pd.DataFrame(renamed_df.groupby("Player_Name").count())

pymoli_sum_df = pd.DataFrame(renamed_df.groupby("Player_Name").sum())
pymoli_spenders_df = pymoli_sum_df
pymoli_spenders_df = pymoli_spenders_df.sort_values(by=["Item_Price"], ascending=False)

pymoli_spenders_df["Purchase Count"] = pymoli_counts_df["Item_ID"]
pymoli_spenders_df["Average Purchase Price"] = round(pymoli_spenders_df["Item_Price"]/pymoli_spenders_df["Purchase Count"],2).map("${:,.2f}".format)
pymoli_spenders_df["Total Purchase Value"] = pymoli_spenders_df["Item_Price"].map("${:,.2f}".format)


pymoli_spenders_df_Summary=pymoli_spenders_df[["Purchase Count","Average Purchase Price","Total Purchase Value"]]
pymoli_spenders_df_Summary.head(5)
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Player_Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items

* Retrieve the Item ID, Item Name, and Item Price columns


* Group by Item ID and Item Name. Perform calculations to obtain purchase count, item price, and total purchase value


* Create a summary data frame to hold the results


* Sort the purchase count column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
pymoli_items_count_df = pd.DataFrame(renamed_df.groupby(["Item_ID","Item_Name"]).count())

pymoli_items_sum_df = pd.DataFrame(renamed_df.groupby(["Item_ID","Item_Name"]).sum())

pymoli_items_count_df["Purchase Count"] = pymoli_items_count_df["Purchase_ID"]
pymoli_items_count_df["Item_Price"] = round(pymoli_items_sum_df["Item_Price"]/pymoli_items_count_df["Purchase_ID"],2)
pymoli_items_count_df["Total Purchase Value"] = pymoli_items_sum_df["Item_Price"]


pymoli_items_count_df = pymoli_items_count_df.sort_values(by = ["Purchase Count","Total Purchase Value"],ascending = False)



pymoli_items_count_df["Item_Price"] = pymoli_items_count_df["Item_Price"].map("${:,.2f}".format)
pymoli_items_count_df["Total Purchase Value"] = pymoli_items_count_df["Total Purchase Value"].map("${:,.2f}".format)
pymoli_popular_items_Summary=pymoli_items_count_df[["Purchase Count","Item_Price","Total Purchase Value"]]
pymoli_popular_items_Summary

most_popular_items_df = pymoli_popular_items_Summary.sort_values("Purchase Count", ascending=False)
most_popular_items_df.head()

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
      <th></th>
      <th>Purchase Count</th>
      <th>Item_Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item_ID</th>
      <th>Item_Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>60</th>
      <th>Wolf</th>
      <td>8</td>
      <td>$3.54</td>
      <td>$28.32</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items

* Sort the above table by total purchase value in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the data frame




```python

#Sort the above table by total purchase value in descending order
pymoli_items_profit = pymoli_items_count_df.sort_values(by = ["Purchase Count","Total Purchase Value"],ascending = False)

#Display a preview of the data frame
pymoli_profit_items_Summary=pymoli_items_profit[["Purchase Count","Item_Price","Total Purchase Value"]]
pymoli_profit_items_Summary
pymoli_profit_items_Summary.head()

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
      <th></th>
      <th>Purchase Count</th>
      <th>Item_Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item_ID</th>
      <th>Item_Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
    <tr>
      <th>19</th>
      <th>Pursuit, Cudgel of Necromancy</th>
      <td>8</td>
      <td>$1.02</td>
      <td>$8.16</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python

```
