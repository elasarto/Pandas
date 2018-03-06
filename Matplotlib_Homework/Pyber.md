
Pyber Observable Trends:

- The majority of Pyber revenue from fares is driven by Urban areas
- Likely due to population or demand, urban areas also have a larger amount of drivers and ride frequencies
- People from the suburbs and rural communities are less likely to be ride share drivers than people from urban areas, however, about a third of all fares and rides come from those in suburban and rural areas.



```python
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import seaborn as sns

file = "city_data.csv"
file = pd.read_csv(file)

file2 = "ride_data.csv"
file2 = pd.read_csv(file2)
```


```python
combined_df = pd.merge(file2, file, how='outer', on=['city'])
combined_df.head()
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Sarabury</td>
      <td>2016-07-23 07:42:44</td>
      <td>21.76</td>
      <td>7546681945283</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Sarabury</td>
      <td>2016-04-02 04:32:25</td>
      <td>38.03</td>
      <td>4932495851866</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Sarabury</td>
      <td>2016-06-23 05:03:41</td>
      <td>26.82</td>
      <td>6711035373406</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Sarabury</td>
      <td>2016-09-30 12:48:34</td>
      <td>30.30</td>
      <td>6388737278232</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_grouped_df = combined_df.groupby(['city', 'type'])
```


```python
city_grouped_df.count().head()
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
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
    </tr>
    <tr>
      <th>city</th>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <th>Urban</th>
      <td>31</td>
      <td>31</td>
      <td>31</td>
      <td>31</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <th>Urban</th>
      <td>26</td>
      <td>26</td>
      <td>26</td>
      <td>26</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <th>Suburban</th>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <th>Urban</th>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <th>Urban</th>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




```python
avg_fare = round(city_grouped_df["fare"].mean(), 2)
total_rides = city_grouped_df["ride_id"].count()
total_drivers = city_grouped_df["driver_count"].sum()//city_grouped_df["driver_count"].count()
area_type = city_grouped_df["type"].apply(list).apply(set)

city_df = pd.DataFrame({"Average Fare":avg_fare,
                                   "Total Rides":total_rides,
                                "Total Drivers":total_drivers,
                            "Area Type":area_type
                       })

city_df["Average Fare"] = avg_fare
city_df["Total Rides"] = total_rides
city_df["Total Drivers"] = total_drivers

city_df.head()

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
      <th>Area Type</th>
      <th>Average Fare</th>
      <th>Total Drivers</th>
      <th>Total Rides</th>
    </tr>
    <tr>
      <th>city</th>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <th>Urban</th>
      <td>{Urban}</td>
      <td>23.93</td>
      <td>21</td>
      <td>31</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <th>Urban</th>
      <td>{Urban}</td>
      <td>20.61</td>
      <td>67</td>
      <td>26</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <th>Suburban</th>
      <td>{Suburban}</td>
      <td>37.32</td>
      <td>16</td>
      <td>9</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <th>Urban</th>
      <td>{Urban}</td>
      <td>23.62</td>
      <td>21</td>
      <td>22</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <th>Urban</th>
      <td>{Urban}</td>
      <td>21.98</td>
      <td>49</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_df["Average Fare"] = pd.to_numeric(city_df["Average Fare"])

```


```python
city_df.columns
```




    Index(['Area Type', 'Average Fare', 'Total Drivers', 'Total Rides'], dtype='object')




```python
x = "Total Rides"
y = "Average Fare"
z = "Total Drivers"

colors = ['gold', 'lightblue', 'coral']

city_df.plot.scatter(x, y, s=city_df['Total Drivers']*10, alpha=0.4, c=colors, edgecolors='darkgrey',
                    grid=True)
#hue="Area Type"
plt.xlim(0,40)
plt.ylim(15,45)

plt.xlabel('Total Rides')
plt.ylabel('Average Fare')
plt.title("Pyber Ride Share Data")

plt.show()
```


![png](output_8_0.png)



```python
city_grouped_df.count().head()
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
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
    </tr>
    <tr>
      <th>city</th>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <th>Urban</th>
      <td>31</td>
      <td>31</td>
      <td>31</td>
      <td>31</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <th>Urban</th>
      <td>26</td>
      <td>26</td>
      <td>26</td>
      <td>26</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <th>Suburban</th>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <th>Urban</th>
      <td>22</td>
      <td>22</td>
      <td>22</td>
      <td>22</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <th>Urban</th>
      <td>19</td>
      <td>19</td>
      <td>19</td>
      <td>19</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_grouped_df2 = combined_df.groupby(['type'])
city_grouped_df2.count().head()
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>125</td>
      <td>125</td>
      <td>125</td>
      <td>125</td>
      <td>125</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>657</td>
      <td>657</td>
      <td>657</td>
      <td>657</td>
      <td>657</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>1625</td>
      <td>1625</td>
      <td>1625</td>
      <td>1625</td>
      <td>1625</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_grouped_df2.sum()
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
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>4255.09</td>
      <td>658729360193746</td>
      <td>727</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>20335.69</td>
      <td>3139583688401015</td>
      <td>9730</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>40078.34</td>
      <td>7890194186030600</td>
      <td>64501</td>
    </tr>
  </tbody>
</table>
</div>




```python
fares = city_grouped_df2['fare'].sum()
drivers = city_grouped_df2['driver_count'].sum()
ride_id = city_grouped_df2['ride_id'].count()
cities_df = pd.DataFrame ({"Fares":fares,"Drivers":drivers,"Total Rides":ride_id})

#cities_df.loc['Total']= cities_df.sum()

cities_df

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
      <th>Drivers</th>
      <th>Fares</th>
      <th>Total Rides</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>727</td>
      <td>4255.09</td>
      <td>125</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>9730</td>
      <td>20335.69</td>
      <td>657</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>64501</td>
      <td>40078.34</td>
      <td>1625</td>
    </tr>
  </tbody>
</table>
</div>




```python
driver_percent = round(cities_df.iloc[:,0]/cities_df.iloc[:,0].sum()*100, 2)
cities_df['Driver Percent'] = driver_percent

fare_percent = round(cities_df.iloc[:,1]/cities_df.iloc[:,1].sum()*100, 2)
cities_df['Fare Percent'] = fare_percent

ride_percent = round(cities_df.iloc[:,2]/cities_df.iloc[:,2].sum()*100, 2)
cities_df['Ride Percent'] = ride_percent

final_df = pd.DataFrame ({"Fare Percentage":fare_percent,"Driver Percentage":driver_percent,"Ride Percentage":ride_percent})

final_df

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
      <th>Driver Percentage</th>
      <th>Fare Percentage</th>
      <th>Ride Percentage</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>0.97</td>
      <td>6.58</td>
      <td>5.19</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>12.98</td>
      <td>31.45</td>
      <td>27.30</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>86.05</td>
      <td>61.97</td>
      <td>67.51</td>
    </tr>
  </tbody>
</table>
</div>




```python
colors = ["gold", "lightblue", "lightcoral"]

explode = (0.1, 0.1, 0)

pie_plot1 = pd.DataFrame(final_df, index=['Rural', 'Suburban', 'Urban'])

pie_plot1.plot.pie(subplots=True, figsize=(20, 6), colors=colors, explode=explode, shadow=True, startangle=140, autopct="%1.1f%%")

plt.axis("equal")

plt.show()
```


![png](output_14_0.png)

