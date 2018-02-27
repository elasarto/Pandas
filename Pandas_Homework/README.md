
Observable Trends

- In both data sets: 
    - Males make up over 80% of users of the gaming app, and they make the most purchases of in-game items 
    - Almost 50% of users (male or female), are ages 20-24
    - The 20-24 age group makes the most purchases of in-game add-ons out of any age group



```python
import pandas as pd

file = "purchase_data.json"

pymoli_df = pd.read_json(file)
pymoli_df.head()
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



Player Count:


```python
#calculate total players
total_players = pymoli_df['Gender'].count()

total_players_df = pd.DataFrame ({"Total Players":[total_players]})

total_players_df
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>780</td>
    </tr>
  </tbody>
</table>
</div>



Purchasing Analysis:


```python
#unique items
unique_items = len(pymoli_df['Item Name'].unique())

#average price
average = round(pymoli_df['Price'].mean(), 2)

#purchase total
purchase_total = pymoli_df['Item Name'].count()

#sum of Prices (Revenue)
price_sum = round(pymoli_df['Price'].sum(), 2)

# total data frame using item price
price_summary_table = pd.DataFrame({"Unique Item Total": [unique_items], 
                                     "Average Item Price": [average],
                                     "Number of Purchases": [purchase_total],
                                     "Total Revenue": [price_sum]})

price_summary_table["Average Item Price"] = price_summary_table["Average Item Price"].map("${0:,.2f}".format)
price_summary_table["Total Revenue"] = price_summary_table["Total Revenue"].map("${0:,.2f}".format)

price_summary_table = price_summary_table[["Unique Item Total", "Average Item Price", 
                                           "Number of Purchases", "Total Revenue"]]

price_summary_table
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
      <th>Unique Item Total</th>
      <th>Average Item Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>179</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>



Gender Demographics:


```python
#calculate gender count and percentages
gender_count = pymoli_df['Gender'].value_counts()
gender_percent = round(pymoli_df['Gender'].value_counts()/pymoli_df['Gender'].count()*100, 2)

#dataframe
demo_df = pd.DataFrame ({"Gender %":gender_percent,"Gender Count":gender_count})

demo_df["Gender %"] = demo_df["Gender %"].map("{0:,.2f}%".format)

demo_df
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
      <th>Gender %</th>
      <th>Gender Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>633</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.44%</td>
      <td>136</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.41%</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>



Purchasing Analysis (Gender):


```python
grouped_demo_df = pymoli_df.groupby(['Gender'])

purchases_total = grouped_demo_df["Price"].sum()
purchases_count = grouped_demo_df["Price"].count()
average = round(grouped_demo_df["Price"].mean(), 2)

demog_summary_table = pd.DataFrame({"Purchase Count":purchases_count,
                                    "Average Purchase Price":average,
                                   "Total Purchase Value":purchases_total})

demog_summary_table["Average Purchase Price"] = demog_summary_table["Average Purchase Price"].astype(float)
norm_values = (demog_summary_table["Average Purchase Price"] - demog_summary_table["Average Purchase Price"].mean())/(demog_summary_table["Average Purchase Price"].std())

demog_summary_table["Average Purchase Price"] = demog_summary_table["Average Purchase Price"].map("${0:,.2f}".format)
demog_summary_table["Total Purchase Value"] = demog_summary_table["Total Purchase Value"].map("${0:,.2f}".format)

demog_summary_table = demog_summary_table[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]] 

demog_summary_table["Normalized Totals"] = norm_values.map("${0:,.2f}".format)

demog_summary_table
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
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$-0.85</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1,867.68</td>
      <td>$-0.26</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$1.10</td>
    </tr>
  </tbody>
</table>
</div>



Age Demographics:


```python
bins = [0, 10, 14, 19, 24, 29, 34, 39, 100]

bin_names = ["< 10","10-14","15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

pd.cut(pymoli_df["Age"], bins, labels=bin_names)

pymoli_df["Age Group"] = pd.cut(pymoli_df["Age"], bins, labels=bin_names)
pymoli_df

grouped_age_groups = pymoli_df.groupby("Age Group")
grouped_age_groups

age_groups = grouped_age_groups["Age"].count()
age_groups_avg = round(grouped_age_groups["Age"].count()/pymoli_df["Age"].count()*100, 2)

ages_df = pd.DataFrame({"Percentage of Players":age_groups_avg,"Total Count":age_groups})

ages_df["Percentage of Players"] = ages_df["Percentage of Players"].map("{0:,.2f}%".format)

ages_df
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
      <th>Age Group</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt; 10</th>
      <td>4.10%</td>
      <td>32</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3.97%</td>
      <td>31</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>17.05%</td>
      <td>133</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>43.08%</td>
      <td>336</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>16.03%</td>
      <td>125</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>8.21%</td>
      <td>64</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>5.38%</td>
      <td>42</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>2.18%</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
</div>



Purchasing Analysis (Age):


```python
age_purchase_total = grouped_age_groups["Price"].sum()
age_purchase_count = grouped_age_groups["Item Name"].count()
age_groups_avg = round(grouped_age_groups["Price"].mean(), 2)

age_sales_df = pd.DataFrame({"Purchase Count":age_purchase_count,
                                    "Average Purchase Price":age_groups_avg,
                                   "Total Purchase Value":age_purchase_total})

age_sales_df["Average Purchase Price"] = age_sales_df["Average Purchase Price"].astype(float)
norm_values = (age_sales_df["Average Purchase Price"] - age_sales_df["Average Purchase Price"].mean())/(age_sales_df["Average Purchase Price"].std())

age_sales_df["Average Purchase Price"] = age_sales_df["Average Purchase Price"].map("${0:,.2f}".format)
age_sales_df["Total Purchase Value"] = age_sales_df["Total Purchase Value"].map("${0:,.2f}".format)

age_sales_df = age_sales_df[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]

age_sales_df["Normalized Totals"] = norm_values.map("${0:,.2f}".format)

age_sales_df

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
      <th>Age Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt; 10</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$0.51</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>31</td>
      <td>$2.70</td>
      <td>$83.79</td>
      <td>$-1.73</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$-0.26</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$-0.26</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$0.09</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$0.92</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$-0.75</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$1.48</td>
    </tr>
  </tbody>
</table>
</div>



Top Spenders


```python
grouped_sn = pymoli_df.groupby(['SN'])

sn_totals = grouped_sn["Price"].sum()
sn_purchase_count = grouped_sn["Item Name"].count()
sn_groups_avg = round(grouped_sn["Price"].mean(),2)

sn_sales_df = pd.DataFrame({"Purchase Count":sn_purchase_count,
                                   "Average Purchase Price":sn_groups_avg,
                                   "Total Purchase Value":sn_totals})

sn_sales_df = sn_sales_df[["Purchase Count", "Average Purchase Price", "Total Purchase Value"]]

sn_sales_df = sn_sales_df.sort_values("Total Purchase Value", ascending=False)

sn_sales_df["Average Purchase Price"] = sn_sales_df["Average Purchase Price"].map("${0:,.2f}".format)
sn_sales_df["Total Purchase Value"] = sn_sales_df["Total Purchase Value"].map("${0:,.2f}".format)

sn_sales_df.head()
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
      <th>Undirrala66</th>
      <td>5</td>
      <td>$3.41</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>$3.39</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>$3.18</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>$4.24</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>$3.86</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>



Most Popular Items


```python
grouped_items = pymoli_df.groupby(['Item ID', 'Item Name'])

counts = grouped_items['Price'].count()
total = grouped_items['Price'].sum()
price = round(grouped_items['Price'].sum()/grouped_items['Price'].count(), 2)

item_df = pd.DataFrame({"Purchase Count":counts,
                                  "Item Price":price,
                                  "Total Purchase Value":total})

item_df = item_df[["Purchase Count", "Item Price", "Total Purchase Value"]]

item_df = item_df.sort_values("Purchase Count", ascending=False)

item_df["Item Price"] = item_df["Item Price"].map("${0:,.2f}".format)
item_df["Total Purchase Value"] = item_df["Total Purchase Value"].map("${0:,.2f}".format)

item_df.head()
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>



Most Profitable Items:


```python
grouped_items = pymoli_df.groupby(['Item ID', 'Item Name'])

counts = grouped_items['Price'].count()
total = grouped_items['Price'].sum()
price = round(grouped_items['Price'].sum()/grouped_items['Price'].count(), 2)

item_df = pd.DataFrame({"Purchase Count":counts,
                                  "Item Price":price,
                                  "Total Purchase Value":total})

item_df = item_df[["Purchase Count", "Item Price", "Total Purchase Value"]]

item_df = item_df.sort_values("Total Purchase Value", ascending=False)

item_df["Item Price"] = item_df["Item Price"].map("${0:,.2f}".format)
item_df["Total Purchase Value"] = item_df["Total Purchase Value"].map("${0:,.2f}".format)

item_df.head()
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


