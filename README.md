Case Study: How Does a Bike-Share Navigate Speedy Success?
============================================================
## introduction 
This is a case study about the **Cyclistic bike-share analysis**, in this case, I will be analyzed by following the steps of **the data analysis process: ask, prepare, process, analyze, share, and act**.

### About the Company
In 2016, **Cyclistic** launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system at any time.

### Business task:
Analyze the Cyclistic data set for one year to understand how annual members and casual riders use Cyclistic bikes differently.


### Stakeholders:

**Lily Moreno**: The director of marketing. Moreno is responsible for the development of campaigns and initiatives to promote the bike-share program.
**Cyclistic marketing analytics team: ** A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy.
**Cyclistic executive team**: The executive team will decide whether to approve the recommended marketing program.

### Deliverables:

> - A description of all data sources used
> - Documentation of any cleaning or manipulation of data
> - A summary of the analysis
> - Supporting visualizations and key findings
> - Top three to four recommendations based on the analysis

## STEP ONE: **ASK**
**Lily Moreno**(The director of marketing and my manager) has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. To do that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends by asking three questions that will guide the future marketing program:

**1.** How do annual members and casual riders use Cyclistic bikes differently?

**2.** Why would casual riders buy Cyclistic annual memberships?

**3.** How can Cyclistic use digital media to influence casual riders to become members?

**Moreno** has assigned you the first question to answer: **How do annual members and casual riders use Cyclistic bikes differently?**, to answer this question I should prepare data by collecting it from a trusted source.

## STEP TWO: PREPARE
the raw data is located in [here](https://divvy-tripdata.s3.amazonaws.com/index.html), and it is publicly available under this [license](https://ride.divvybikes.com/data-license-agreement) with some privacy restriction.


**ROCCC data**

> - **Reliable**: this dataset is complete, accurate and it is from a trusted source

> - **Original:** the data collected from the first part means it is Original 

> - **Comprehensive:** the data contain information that we need to do the analysis

> -  **Current:** I choose the period from September 2021 to August 2022 

> - **Cited:** the data is [here](https://divvy-tripdata.s3.amazonaws.com/index.html)

before downloading the raw data we need to install some dependencies

***install and import the libraries***


```python

import matplotlib.pyplot as plt 
import seaborn as sns
import pandas as pd
import numpy as np
import gc
```

1. Pandas is python data analysis library, you can check the [documentation](https://pandas.pydata.org/docs/)
2. Numpy is python library for scientific computing, again you can see more in the [documenatation](https://numpy.org/doc/stable/)
3. Matplotlib is pythonn data visualization library, see [more](https://matplotlib.org/)

***then, we downloaded 12 the dataset that are from September 2021 to August 2022***


```python
df_01 = pd.read_csv("/content/drive/MyDrive/dataset/divvy-tripdata_202109.csv")
df_02 = pd.read_csv("/content/drive/MyDrive/dataset/divvy-tripdata_202110.csv")
df_03 = pd.read_csv("/content/drive/MyDrive/dataset/divvy-tripdata_202111.csv")
df_04 = pd.read_csv("/content/drive/MyDrive/dataset/divvy-tripdata_202112.csv")
df_05 = pd.read_csv("/content/drive/MyDrive/dataset/divvy-tripdata_202201.csv")
df_06 = pd.read_csv("/content/drive/MyDrive/dataset/divvy-tripdata_202202.csv")
df_07 = pd.read_csv("/content/drive/MyDrive/dataset/divvy-tripdata_202203.csv")
df_08 = pd.read_csv("/content/drive/MyDrive/dataset/divvy-tripdata_202204.csv")
df_09 = pd.read_csv("/content/drive/MyDrive/dataset/divvy-tripdata_202205.csv")
df_10 = pd.read_csv("/content/drive/MyDrive/dataset/divvy-tripdata_202206.csv")
df_11 = pd.read_csv("/content/drive/MyDrive/dataset/divvy-tripdata_202207.csv")
df_12 = pd.read_csv("/content/drive/MyDrive/dataset/divvy-tripdata_202208.csv")

```

#### 1.Before wrangling dataset we will concatenate all the datatset into one then remove the 12 dataset 

we doing that because we need more RAM


```python
#concate the all dataset into one 
df = pd.concat([df_01, df_02, df_03, df_04, df_05, df_06, df_07, df_08, df_09, df_10,
               df_11, df_12])
```

Save df to  pickle, for computation reasons.


```python
#save df to pickle 
df.to_pickle("./bike.pkl")
```

read bike.pkl 


```python
bike = pd.read_pickle('bike.pkl')
bike.head()
```





  <div id="df-1b6da382-f415-4dff-8ba2-547d3508a018">
    <div class="colab-df-container">
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
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>start_station_name</th>
      <th>start_station_id</th>
      <th>end_station_name</th>
      <th>end_station_id</th>
      <th>start_lat</th>
      <th>start_lng</th>
      <th>end_lat</th>
      <th>end_lng</th>
      <th>member_casual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9DC7B962304CBFD8</td>
      <td>electric_bike</td>
      <td>2021-09-28 16:07:10</td>
      <td>2021-09-28 16:09:54</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>41.89</td>
      <td>-87.68</td>
      <td>41.89</td>
      <td>-87.67</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>1</th>
      <td>F930E2C6872D6B32</td>
      <td>electric_bike</td>
      <td>2021-09-28 14:24:51</td>
      <td>2021-09-28 14:40:05</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>41.94</td>
      <td>-87.64</td>
      <td>41.98</td>
      <td>-87.67</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6EF72137900BB910</td>
      <td>electric_bike</td>
      <td>2021-09-28 00:20:16</td>
      <td>2021-09-28 00:23:57</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>41.81</td>
      <td>-87.72</td>
      <td>41.80</td>
      <td>-87.72</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>3</th>
      <td>78D1DE133B3DBF55</td>
      <td>electric_bike</td>
      <td>2021-09-28 14:51:17</td>
      <td>2021-09-28 15:00:06</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>41.80</td>
      <td>-87.72</td>
      <td>41.81</td>
      <td>-87.72</td>
      <td>casual</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E03D4ACDCAEF6E00</td>
      <td>electric_bike</td>
      <td>2021-09-28 09:53:12</td>
      <td>2021-09-28 10:03:44</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>41.88</td>
      <td>-87.74</td>
      <td>41.88</td>
      <td>-87.71</td>
      <td>casual</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-1b6da382-f415-4dff-8ba2-547d3508a018')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-1b6da382-f415-4dff-8ba2-547d3508a018 button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-1b6da382-f415-4dff-8ba2-547d3508a018');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>





```python
#delete all 
del df_01, df_02, df_03, df_04, df_05, df_06, df_07, df_08, df_09, df_10, df_11, df_12
gc.collect()

```




    0



#### 2. see if we have the same columns in all dataset


```python
#bike columns
bike.columns 
```




    Index(['ride_id', 'rideable_type', 'started_at', 'ended_at',
           'start_station_name', 'start_station_id', 'end_station_name',
           'end_station_id', 'start_lat', 'start_lng', 'end_lat', 'end_lng',
           'member_casual'],
          dtype='object')



#### 3. A Structure  dataset, we want to see each column and it's a datatype


```python
#dtypes
bike.dtypes
```




    ride_id                object
    rideable_type          object
    started_at             object
    ended_at               object
    start_station_name     object
    start_station_id       object
    end_station_name       object
    end_station_id         object
    start_lat             float64
    start_lng             float64
    end_lat               float64
    end_lng               float64
    member_casual          object
    dtype: object




dataset have the same columns ***(ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_id, start_lat, start_lng, end_lat, end_lng, member_casual)***,  I didn't to rename some dataset's becaue they have same columns name

## STEP THREE: PROCESSING

After combining all datasets into one, first clean the df dataset by the filter's out the duplicated value and looking for missing numeric values, after that, I will add more columns that beneficial to my analysis.

#### 1- remove the duplicated value


```python
#df, ride_id duplicated
print(sum(bike['ride_id'].duplicated()))
```

    0


-as you can see there's no duplication in ride_id, so now let's keep forward and deal with the missing value

#### 2- check if there is a missing value.


```python
#df missing value for ride_id
print(sum(bike.ride_id.isnull()))

#df missing value for rideable_type
print(sum(bike.rideable_type.isnull()))

#df missing value for started_at 
print(sum(bike.started_at.isnull()))

#df missing value for ended_at
print(sum(bike.ended_at.isnull()))

#df missing value for start_station_name
print(sum(bike.start_station_name.isnull()))

#df missing value for start_station_id
print(sum(bike.start_station_id.isnull()))

#df missing value for end_station_name
print(sum(bike.end_station_name.isnull()))

#df missing value for end_station_id
print(sum(bike.end_station_id.isnull()))

#df missing value for start_lat
print(sum(bike.start_lat.isnull()))

#dfmissing value for start_lng
print(sum(bike.start_lng.isnull()))

#dfmissing value for end_lat
print(sum(bike.end_lat.isnull()))

#df missing value for end_lng
print(sum(bike.end_lng.isnull()))

#df missing value for member_casual
print(sum(bike.member_casual.isnull()))

```

    0
    0
    0
    0
    884365
    884363
    946303
    946303
    0
    0
    5727
    5727
    0


The df columns missing value is **start_station_name, start_station_id, end_station_name, end_station_id, end_lat, end_lng**

 #### 3- filling the missing 
 let's filling each dataset missing value by first looking for the datatype and filling the null in terms of thier datatype


```python
#fillna
bike["start_station_name"].ffill(axis=0, inplace=True)
bike["start_station_name"].bfill(axis=0, inplace=True)
bike["start_station_id"].ffill(axis=0, inplace=True)
bike["start_station_id"].bfill(axis=0, inplace=True)
bike["end_station_name"].ffill(axis=0, inplace=True)
bike["end_station_name"].bfill(axis=0, inplace=True)
bike["end_station_id"].ffill(axis=0, inplace=True)
bike["end_station_id"].bfill(axis=0, inplace=True)
bike["end_lat"].ffill(axis=0, inplace=True)
bike["end_lat"].bfill(axis=0, inplace=True)
bike["end_lng"].ffill(axis=0, inplace=True)
bike["end_lng"].bfill(axis=0, inplace=True)
```



##### 4- Data varification
check if there is duplication and missing value 



```python
#duplucation
print(sum(bike['ride_id'].duplicated()))

```

    0



```python
#df missing value for ride_id
print(sum(bike.ride_id.isnull()))

#df missing value for rideable_type
print(sum(bike.rideable_type.isnull()))

#df missing value for started_at 
print(sum(bike.started_at.isnull()))

#df missing value for ended_at
print(sum(bike.ended_at.isnull()))

#df missing value for start_station_name
print(sum(bike.start_station_name.isnull()))

#df missing value for start_station_id
print(sum(bike.start_station_id.isnull()))

#df missing value for end_station_name
print(sum(bike.end_station_name.isnull()))

#df missing value for end_station_id
print(sum(bike.end_station_id.isnull()))

#df missing value for start_lat
print(sum(bike.start_lat.isnull()))

#dfmissing value for start_lng
print(sum(bike.start_lng.isnull()))

#dfmissing value for end_lat
print(sum(bike.end_lat.isnull()))

#df missing value for end_lng
print(sum(bike.end_lng.isnull()))

#df missing value for member_casual
print(sum(bike.member_casual.isnull()))
```

    0
    0
    0
    0
    0
    0
    0
    0
    0
    0
    0
    0
    0


##### 5. create more columns
5.1. converting the started_at and ended_at DateTime datatype and splitting into the day, month, year, and day of the week.


```python
#convert the dataframe
#started_at, ended_at to datetime 
bike['started_at'] = pd.to_datetime(bike['started_at'])
bike['ended_at'] = pd.to_datetime(bike['ended_at'])
bike['day'] = bike['started_at'].dt.day
bike['month'] = bike['started_at'].dt.month_name()
bike['year'] = bike['started_at'].dt.year
bike['day_of_week'] = bike['started_at'].dt.day_name()
bike['hour'] = bike['started_at'].dt.hour
bike['minute'] = bike['started_at'].dt.minute
bike['second'] = bike['started_at'].dt.second
bike.head()
```





  <div id="df-9f7500d4-e90c-496e-95b3-dcf52aeb923f">
    <div class="colab-df-container">
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
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>start_station_name</th>
      <th>start_station_id</th>
      <th>end_station_name</th>
      <th>end_station_id</th>
      <th>start_lat</th>
      <th>start_lng</th>
      <th>end_lat</th>
      <th>end_lng</th>
      <th>member_casual</th>
      <th>day</th>
      <th>month</th>
      <th>year</th>
      <th>day_of_week</th>
      <th>hour</th>
      <th>minute</th>
      <th>second</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9DC7B962304CBFD8</td>
      <td>electric_bike</td>
      <td>2021-09-28 16:07:10</td>
      <td>2021-09-28 16:09:54</td>
      <td>Clark St &amp; Grace St</td>
      <td>TA1307000127</td>
      <td>Desplaines St &amp; Kinzie St</td>
      <td>TA1306000003</td>
      <td>41.89</td>
      <td>-87.68</td>
      <td>41.89</td>
      <td>-87.67</td>
      <td>casual</td>
      <td>28</td>
      <td>September</td>
      <td>2021</td>
      <td>Tuesday</td>
      <td>16</td>
      <td>7</td>
      <td>10</td>
    </tr>
    <tr>
      <th>1</th>
      <td>F930E2C6872D6B32</td>
      <td>electric_bike</td>
      <td>2021-09-28 14:24:51</td>
      <td>2021-09-28 14:40:05</td>
      <td>Clark St &amp; Grace St</td>
      <td>TA1307000127</td>
      <td>Desplaines St &amp; Kinzie St</td>
      <td>TA1306000003</td>
      <td>41.94</td>
      <td>-87.64</td>
      <td>41.98</td>
      <td>-87.67</td>
      <td>casual</td>
      <td>28</td>
      <td>September</td>
      <td>2021</td>
      <td>Tuesday</td>
      <td>14</td>
      <td>24</td>
      <td>51</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6EF72137900BB910</td>
      <td>electric_bike</td>
      <td>2021-09-28 00:20:16</td>
      <td>2021-09-28 00:23:57</td>
      <td>Clark St &amp; Grace St</td>
      <td>TA1307000127</td>
      <td>Desplaines St &amp; Kinzie St</td>
      <td>TA1306000003</td>
      <td>41.81</td>
      <td>-87.72</td>
      <td>41.80</td>
      <td>-87.72</td>
      <td>casual</td>
      <td>28</td>
      <td>September</td>
      <td>2021</td>
      <td>Tuesday</td>
      <td>0</td>
      <td>20</td>
      <td>16</td>
    </tr>
    <tr>
      <th>3</th>
      <td>78D1DE133B3DBF55</td>
      <td>electric_bike</td>
      <td>2021-09-28 14:51:17</td>
      <td>2021-09-28 15:00:06</td>
      <td>Clark St &amp; Grace St</td>
      <td>TA1307000127</td>
      <td>Desplaines St &amp; Kinzie St</td>
      <td>TA1306000003</td>
      <td>41.80</td>
      <td>-87.72</td>
      <td>41.81</td>
      <td>-87.72</td>
      <td>casual</td>
      <td>28</td>
      <td>September</td>
      <td>2021</td>
      <td>Tuesday</td>
      <td>14</td>
      <td>51</td>
      <td>17</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E03D4ACDCAEF6E00</td>
      <td>electric_bike</td>
      <td>2021-09-28 09:53:12</td>
      <td>2021-09-28 10:03:44</td>
      <td>Clark St &amp; Grace St</td>
      <td>TA1307000127</td>
      <td>Desplaines St &amp; Kinzie St</td>
      <td>TA1306000003</td>
      <td>41.88</td>
      <td>-87.74</td>
      <td>41.88</td>
      <td>-87.71</td>
      <td>casual</td>
      <td>28</td>
      <td>September</td>
      <td>2021</td>
      <td>Tuesday</td>
      <td>9</td>
      <td>53</td>
      <td>12</td>
    </tr>
  </tbody>
</table>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-9f7500d4-e90c-496e-95b3-dcf52aeb923f')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-9f7500d4-e90c-496e-95b3-dcf52aeb923f button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-9f7500d4-e90c-496e-95b3-dcf52aeb923f');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>




5.2. Add another column to ride_length by calculating the difference between ended_at and started_at, convert the datatype of ride_length, and than check if ride_length is numeric.



```python
#difference betwee the ended_at and started_at
bike['ride_length'] = bike['ended_at'] - bike['started_at']
bike['ride_length'] = bike['ride_length'].dt.total_seconds()
bike['ride_lengthMean'] = bike['ride_length'].rolling(20).mean() #simple moving average 
bike['ride_lengthMean'].fillna(bike['ride_lengthMean'].mean(), inplace=True)
bike['ride_lengthMean'].head(40)
```




    0     1185.211725
    1     1185.211725
    2     1185.211725
    3     1185.211725
    4     1185.211725
    5     1185.211725
    6     1185.211725
    7     1185.211725
    8     1185.211725
    9     1185.211725
    10    1185.211725
    11    1185.211725
    12    1185.211725
    13    1185.211725
    14    1185.211725
    15    1185.211725
    16    1185.211725
    17    1185.211725
    18    1185.211725
    19     811.650000
    20     863.700000
    21     853.900000
    22     916.850000
    23     905.300000
    24     907.900000
    25     931.950000
    26     948.150000
    27     900.300000
    28     956.050000
    29    1125.600000
    30    1184.600000
    31    1198.850000
    32    1212.800000
    33    1237.500000
    34    1250.450000
    35    1239.750000
    36    1254.850000
    37    1261.350000
    38    1161.600000
    39    1189.100000
    Name: ride_lengthMean, dtype: float64




```python
bike['ride_lengthMean'].dropna(inplace=True)
```

- convert to float64




```python
#to_numeric, head, dtypes
bike['ride_length'] = bike['ride_length'].astype(np.float64)
print(bike.head())
print(bike.dtypes)
```

                ride_id  rideable_type          started_at            ended_at  \
    0  9DC7B962304CBFD8  electric_bike 2021-09-28 16:07:10 2021-09-28 16:09:54   
    1  F930E2C6872D6B32  electric_bike 2021-09-28 14:24:51 2021-09-28 14:40:05   
    2  6EF72137900BB910  electric_bike 2021-09-28 00:20:16 2021-09-28 00:23:57   
    3  78D1DE133B3DBF55  electric_bike 2021-09-28 14:51:17 2021-09-28 15:00:06   
    4  E03D4ACDCAEF6E00  electric_bike 2021-09-28 09:53:12 2021-09-28 10:03:44   
    
        start_station_name start_station_id           end_station_name  \
    0  Clark St & Grace St     TA1307000127  Desplaines St & Kinzie St   
    1  Clark St & Grace St     TA1307000127  Desplaines St & Kinzie St   
    2  Clark St & Grace St     TA1307000127  Desplaines St & Kinzie St   
    3  Clark St & Grace St     TA1307000127  Desplaines St & Kinzie St   
    4  Clark St & Grace St     TA1307000127  Desplaines St & Kinzie St   
    
      end_station_id  start_lat  start_lng  ...  member_casual  day      month  \
    0   TA1306000003      41.89     -87.68  ...         casual   28  September   
    1   TA1306000003      41.94     -87.64  ...         casual   28  September   
    2   TA1306000003      41.81     -87.72  ...         casual   28  September   
    3   TA1306000003      41.80     -87.72  ...         casual   28  September   
    4   TA1306000003      41.88     -87.74  ...         casual   28  September   
    
       year day_of_week  hour minute  second  ride_length  ride_lengthMean  
    0  2021     Tuesday    16      7      10        164.0      1185.211725  
    1  2021     Tuesday    14     24      51        914.0      1185.211725  
    2  2021     Tuesday     0     20      16        221.0      1185.211725  
    3  2021     Tuesday    14     51      17        529.0      1185.211725  
    4  2021     Tuesday     9     53      12        632.0      1185.211725  
    
    [5 rows x 22 columns]
    ride_id                       object
    rideable_type                 object
    started_at            datetime64[ns]
    ended_at              datetime64[ns]
    start_station_name            object
    start_station_id              object
    end_station_name              object
    end_station_id                object
    start_lat                    float64
    start_lng                    float64
    end_lat                      float64
    end_lng                      float64
    member_casual                 object
    day                            int64
    month                         object
    year                           int64
    day_of_week                   object
    hour                           int64
    minute                         int64
    second                         int64
    ride_length                  float64
    ride_lengthMean              float64
    dtype: object



```python
#error's in ride_length 
sum(bike['ride_length'] < 0)

#convert the negative to positive 
bike['ride_length'] = bike['ride_length'].abs()
```

check if ride_length has no negative number


```python
sum(bike['ride_length'] < 0)

```




    0



7 -  let's see if more then casual and member in member_casual, and I doing some calculation.


```python
# unique the member_casual
set(bike['member_casual'])
```




    {'casual', 'member'}



In member_casual there are two category object, the casual and member.

After this long step of processing data, now I can do the analysis step.

 ## STEP FOUR: ANALYSIS

-Before jumping to the analysis, let's take the summary, structure, number of columns 


```python
#head, describe(), dtypes, columns
print(bike.head())
print(bike.describe())
print(bike.dtypes)
print(bike.columns)
```

                ride_id  rideable_type          started_at            ended_at  \
    0  9DC7B962304CBFD8  electric_bike 2021-09-28 16:07:10 2021-09-28 16:09:54   
    1  F930E2C6872D6B32  electric_bike 2021-09-28 14:24:51 2021-09-28 14:40:05   
    2  6EF72137900BB910  electric_bike 2021-09-28 00:20:16 2021-09-28 00:23:57   
    3  78D1DE133B3DBF55  electric_bike 2021-09-28 14:51:17 2021-09-28 15:00:06   
    4  E03D4ACDCAEF6E00  electric_bike 2021-09-28 09:53:12 2021-09-28 10:03:44   
    
        start_station_name start_station_id           end_station_name  \
    0  Clark St & Grace St     TA1307000127  Desplaines St & Kinzie St   
    1  Clark St & Grace St     TA1307000127  Desplaines St & Kinzie St   
    2  Clark St & Grace St     TA1307000127  Desplaines St & Kinzie St   
    3  Clark St & Grace St     TA1307000127  Desplaines St & Kinzie St   
    4  Clark St & Grace St     TA1307000127  Desplaines St & Kinzie St   
    
      end_station_id  start_lat  start_lng  ...  member_casual  day      month  \
    0   TA1306000003      41.89     -87.68  ...         casual   28  September   
    1   TA1306000003      41.94     -87.64  ...         casual   28  September   
    2   TA1306000003      41.81     -87.72  ...         casual   28  September   
    3   TA1306000003      41.80     -87.72  ...         casual   28  September   
    4   TA1306000003      41.88     -87.74  ...         casual   28  September   
    
       year day_of_week  hour minute  second  ride_length  ride_lengthMean  
    0  2021     Tuesday    16      7      10        164.0      1185.211725  
    1  2021     Tuesday    14     24      51        914.0      1185.211725  
    2  2021     Tuesday     0     20      16        221.0      1185.211725  
    3  2021     Tuesday    14     51      17        529.0      1185.211725  
    4  2021     Tuesday     9     53      12        632.0      1185.211725  
    
    [5 rows x 22 columns]
              start_lat     start_lng       end_lat       end_lng           day  \
    count  5.883043e+06  5.883043e+06  5.883043e+06  5.883043e+06  5.883043e+06   
    mean   4.190104e+01 -8.764766e+01  4.190129e+01 -8.764787e+01  1.572517e+01   
    std    4.719795e-02  3.097188e-02  4.730615e-02  3.060118e-02  8.719473e+00   
    min    4.164000e+01 -8.784000e+01  4.139000e+01 -8.897000e+01  1.000000e+00   
    25%    4.188103e+01 -8.766201e+01  4.188103e+01 -8.766356e+01  9.000000e+00   
    50%    4.189993e+01 -8.764377e+01  4.190000e+01 -8.764410e+01  1.600000e+01   
    75%    4.193000e+01 -8.762930e+01  4.193000e+01 -8.762932e+01  2.300000e+01   
    max    4.563503e+01 -7.379648e+01  4.237000e+01 -8.750000e+01  3.100000e+01   
    
                   year          hour        minute        second   ride_length  \
    count  5.883043e+06  5.883043e+06  5.883043e+06  5.883043e+06  5.883043e+06   
    mean   2.021661e+03  1.422414e+01  2.950921e+01  2.952116e+01  1.185279e+03   
    std    4.734011e-01  5.033026e+00  1.728415e+01  1.730999e+01  9.628988e+03   
    min    2.021000e+03  0.000000e+00  0.000000e+00  0.000000e+00  0.000000e+00   
    25%    2.021000e+03  1.100000e+01  1.500000e+01  1.500000e+01  3.630000e+02   
    50%    2.022000e+03  1.500000e+01  2.900000e+01  3.000000e+01  6.430000e+02   
    75%    2.022000e+03  1.800000e+01  4.500000e+01  4.500000e+01  1.160000e+03   
    max    2.022000e+03  2.300000e+01  5.900000e+01  5.900000e+01  2.442301e+06   
    
           ride_lengthMean  
    count     5.883043e+06  
    mean      1.185212e+03  
    std       2.263981e+03  
    min      -2.940000e+02  
    25%       7.105000e+02  
    50%       9.059500e+02  
    75%       1.203700e+03  
    max       1.243909e+05  
    ride_id                       object
    rideable_type                 object
    started_at            datetime64[ns]
    ended_at              datetime64[ns]
    start_station_name            object
    start_station_id              object
    end_station_name              object
    end_station_id                object
    start_lat                    float64
    start_lng                    float64
    end_lat                      float64
    end_lng                      float64
    member_casual                 object
    day                            int64
    month                         object
    year                           int64
    day_of_week                   object
    hour                           int64
    minute                         int64
    second                         int64
    ride_length                  float64
    ride_lengthMean              float64
    dtype: object
    Index(['ride_id', 'rideable_type', 'started_at', 'ended_at',
           'start_station_name', 'start_station_id', 'end_station_name',
           'end_station_id', 'start_lat', 'start_lng', 'end_lat', 'end_lng',
           'member_casual', 'day', 'month', 'year', 'day_of_week', 'hour',
           'minute', 'second', 'ride_length', 'ride_lengthMean'],
          dtype='object')


How many percentage of each category in member_casual 

convert member_casual to be string datatypes


```python
#convert to category 
bike['member_casual'] = bike['member_casual'].astype("category")
bike.dtypes
```




    ride_id                       object
    rideable_type                 object
    started_at            datetime64[ns]
    ended_at              datetime64[ns]
    start_station_name            object
    start_station_id              object
    end_station_name              object
    end_station_id                object
    start_lat                    float64
    start_lng                    float64
    end_lat                      float64
    end_lng                      float64
    member_casual               category
    day                            int64
    month                         object
    year                           int64
    day_of_week                   object
    hour                           int64
    minute                         int64
    second                         int64
    ride_length                  float64
    ride_lengthMean              float64
    dtype: object



the count and the percentage of each category in member_casual


```python
print(bike['member_casual'].value_counts())
print(len(bike['member_casual']))
count = bike['member_casual'].value_counts()
lenth = len(bike['member_casual'])
print((count/lenth)*100)
```

    member    3414564
    casual    2468479
    Name: member_casual, dtype: int64
    5883043
    member    58.040779
    casual    41.959221
    Name: member_casual, dtype: float64


-**FINDING (1)**


- In general, in the  above the casual have great (mean, max, sum) values, and they also varied (standard deviation) than members even if the count and percentage of member are more by near to 58% of the member greater than casual, maybe this is because there are a few users casuals (42%) using the bike's for a long ride but they are not a member.

Using **pivot table** to sumarise the mean, sum, std, max and min 


```python
table = pd.pivot_table(bike, values=['ride_length'], index=['member_casual'],
                     aggfunc=[np.mean, np.sum, np.std, max, min])
print(table)
```

                          mean           sum           std         max         min
                   ride_length   ride_length   ride_length ride_length ride_length
    member_casual                                                                 
    casual         1757.901519  4.339343e+09  14716.900850   2442301.0         0.0
    member          771.314240  2.633702e+09   1661.414401     93594.0         0.0


-**FINDING (2)**
- > here in the table above we can take an intuition about why the mean of casual is greater than member but the number of rides is way less than that of the member, that's because the ride length sum of casual(4339343000) is greater of the ride sum of member(2633702000). conversely, the length of rides of casual is less than the ride length of the member, this explains why the mean of the casual ride is greater than that of member.


**chart(1)**

the sum of each member and casual 


```python
plt.figure(figsize=(10,8))
ax = sns.barplot(x='member_casual', 
            y='ride_length', data=bike, 
            estimator=sum, palette='viridis')

ax.set(title="the sum of each member and casual", xlabel = "member and casual", ylabel = "sum of ride length")

```




    [Text(0, 0.5, 'sum of ride length'),
     Text(0.5, 0, 'member and casual'),
     Text(0.5, 1.0, 'the sum of each member and casual')]




    
![png](Bike_files/Bike_52_1.png)
    


-**FINDING (3)**
- > As you can see in chart(1)  above, the sum of casual are  more than the sum of casual.

**chart(2)**

- the mean of ride length of each member and casual 


```python
plt.figure(figsize=(10,8))
ax = sns.barplot(x='member_casual', 
                 y='ride_length',  
                  data=bike, palette="husl")
ax.set(title = "The total of ride over the member and casual", xlabel = "member and casual", ylabel = "Total ride")
```




    [Text(0, 0.5, 'Total ride'),
     Text(0.5, 0, 'member and casual'),
     Text(0.5, 1.0, 'The total of ride over the member and casual')]




    
![png](Bike_files/Bike_55_1.png)
    


-**FINDING (4)**

- > In chart(2), the mean ride length of the casual is up to 1500, while the member didn't reach 1000.


Now let's see the number of ride length in member and casual at hours


```python
#the 
table = bike.groupby(['hour', 'member_casual']).agg({'ride_length':['sum', 'mean']})
table.sort_values

```




    <bound method DataFrame.sort_values of                     ride_length             
                                sum         mean
    hour member_casual                          
    0    casual          89613166.0  1800.111807
         member          28918606.0   792.051875
    1    casual          71153342.0  2169.706105
         member          18897836.0   832.503789
    2    casual          47650506.0  2294.640566
         member          10642252.0   825.044732
    3    casual          29671576.0  2448.149835
         member           6620405.0   838.875443
    4    casual          20099725.0  2301.583076
         member           7403995.0   813.983619
    5    casual          18468543.0  1397.121038
         member          22255769.0   656.009226
    6    casual          38989718.0  1333.164125
         member          63122792.0   684.732953
    7    casual          65461521.0  1235.799230
         member         124707051.0   711.895756
    8    casual          94201801.0  1330.740666
         member         143511171.0   697.160427
    9    casual         123162135.0  1585.588019
         member         104669111.0   705.279439
    10   casual         197028081.0  1930.077300
         member         106625224.0   753.604388
    11   casual         254851470.0  1932.156710
         member         130556415.0   771.742290
    12   casual         294087191.0  1902.947342
         member         146785007.0   754.962053
    13   casual         306748264.0  1892.561522
         member         145771690.0   761.782698
    14   casual         321866244.0  1894.300283
         member         148628877.0   785.685316
    15   casual         353661714.0  1893.152512
         member         177150278.0   793.285977
    16   casual         361264347.0  1757.216325
         member         236632986.0   805.238377
    17   casual         376053750.0  1623.903158
         member         290209836.0   817.310615
    18   casual         343865314.0  1639.116409
         member         235824140.0   805.534116
    19   casual         273358126.0  1689.377208
         member         167115566.0   793.917034
    20   casual         204264304.0  1714.518491
         member         115554476.0   781.037350
    21   casual         173682345.0  1701.850424
         member          88533971.0   771.914581
    22   casual         148252673.0  1586.795032
         member          67284398.0   766.066628
    23   casual         131887127.0  1893.026080
         member          46279984.0   793.498114>



**-FINDING (5)**

- The sum value of casual is **376053750** at five pm, (**17:00**), and the mean of it is 2448.15 at three am (**3**)
- The max vaule of member is **290209836** at five pm (**17:00**), and the mean value of the member is 838.9 , the same as casual at three am (**3**)

Lets make visual chart of what we say above. 
by first the mean or the average ride of both member and casual over the hour of a day, then seen the sum ride of each member and casual over hour of a day. 

chart(3): The average ride of each the member and casual over the hour of day



```python
plt.figure(figsize=(10,8))
ax = sns.barplot(x='hour', 
                 y='ride_length', hue='member_casual',  
                  data=bike, palette="husl")
ax.set(title = "The average ride of each the member and casual over the hour of day ", 
       xlabel = "hour", ylabel = "average ride")
```




    [Text(0, 0.5, 'average ride'),
     Text(0.5, 0, 'hour'),
     Text(0.5, 1.0, 'The average ride of each the member and casual over the hour of day ')]




    
![png](Bike_files/Bike_61_1.png)
    


-**FINDING (6)**
- > In the chart(3): there some pickes specaily in 3 am with highest error bars varaition, and that indicate to the high demand from casual customer 

chart(4): The sum ride of each the member and casual over the hour of day


```python
plt.figure(figsize=(10,8))
ax = sns.barplot(x='hour', 
                 y='ride_length', hue='member_casual',  
                  data=bike, palette="husl", estimator=sum)
ax.set(title = "The average ride of each the member and casual over the hour of day ", 
       xlabel = "hour", ylabel = "Total ride")
```




    [Text(0, 0.5, 'Total ride'),
     Text(0.5, 0, 'hour'),
     Text(0.5, 1.0, 'The average ride of each the member and casual over the hour of day ')]




    
![png](Bike_files/Bike_64_1.png)
    


-**Finding(7):**

- > In the chart(4): the demand keep increasing from 7 am to 8 pm for both member and casual. 


-compare the casual and member at weekday 


```python
table = bike.groupby(['day_of_week', 'member_casual']).agg({'ride_length':['sum', 'mean']})
table.sort_values

```




    <bound method DataFrame.sort_values of                            ride_length             
                                       sum         mean
    day_of_week member_casual                          
    Friday      casual         579017647.0  1673.330830
                member         356813857.0   755.673381
    Monday      casual         523173875.0  1790.563053
                member         354693279.0   747.008915
    Saturday    casual         980528463.0  1922.363592
                member         389591582.0   858.089656
    Sunday      casual         897296333.0  2051.211536
                member         350128314.0   865.529483
    Thursday    casual         486413798.0  1561.576165
                member         390158655.0   742.470613
    Tuesday     casual         432323426.0  1554.991587
                member         392019038.0   730.705354
    Wednesday   casual         440589441.0  1502.461904
                member         400297111.0   731.753756>



-**Finding(8):**
- > the max sum value for casual is 980528463 at Saturday, and mean at 2051.2 at Sunday.
-> the max sum value for member is 400297111  in Wednesday and the mean also at Sunday by 865.52


chart(5): The average ride of each the member and casual over the days of week 


```python
plt.figure(figsize=(10,8))
ax = sns.barplot(x='day_of_week', 
                 y='ride_length', hue='member_casual',  
                  data=bike, palette="tab10")
ax.set(title = "The average ride of each the member and casual over the day  of week", 
       xlabel = "day of week", ylabel = "avreage ride")
```




    [Text(0, 0.5, 'avreage ride'),
     Text(0.5, 0, 'day of week'),
     Text(0.5, 1.0, 'The average ride of each the member and casual over the day  of week')]




    
![png](Bike_files/Bike_70_1.png)
    


-**Finding(9):**
- > the memeber didn't exceed the mean of 1000 through out all seven days 
- > the casual are more varied secailly in sunday 

chart(6): The sum ride of each the member and casual over the days of week.


```python
plt.figure(figsize=(10,8))
ax = sns.barplot(x='day_of_week', 
                 y='ride_length', hue='member_casual',  
                  data=bike, palette="tab10", estimator=sum)
ax.set(title = "The total ride of each the member and casual over the day  of week", 
       xlabel = "day of week", ylabel = "Total ride")
```




    [Text(0, 0.5, 'Total ride'),
     Text(0.5, 0, 'day of week'),
     Text(0.5, 1.0, 'The total ride of each the member and casual over the day  of week')]




    
![png](Bike_files/Bike_73_1.png)
    


-**Finding(10)**
- > as you can see the chart(5) the highest value of ride are in Saturday and for casual, and wednesday for member 


##### let's see how the member and casual are behave differently in days 



```python
table = bike.groupby(['day', 'member_casual']).agg({'ride_length':['sum',  'mean']})

```

-Finding(11): 

 - The sum value  of causal is 195326017.0 located in day 10 , and the mean of it is 2009.04 in day five.
 - Day 12 is highest sum value for member is 102655507.0	and thier greater mean is 803.23 located in day five  



chart(7): The total ride of each the member and casual over the days



```python
plt.figure(figsize=(10,8))
ax = sns.barplot(x='day', 
                 y='ride_length', hue='member_casual',  
                  data=bike, palette="tab10")
ax.set(title = "The average ride of each the member and casual over the days", 
       xlabel = "days", ylabel = "avreage ride")
```




    [Text(0, 0.5, 'avreage ride'),
     Text(0.5, 0, 'days'),
     Text(0.5, 1.0, 'The average ride of each the member and casual over the days')]




    
![png](Bike_files/Bike_79_1.png)
    


-chart(8): The total ride of each the member and casual over the days





```python
plt.figure(figsize=(10,8))
ax = sns.barplot(x='day', 
                 y='ride_length', hue='member_casual',  
                  data=bike, palette="tab10", estimator=sum)

ax.set(title = "The total ride of each the member and casual over the days", 
       xlabel = "days", ylabel = "total ride")
```




    [Text(0, 0.5, 'total ride'),
     Text(0.5, 0, 'days'),
     Text(0.5, 1.0, 'The total ride of each the member and casual over the days')]




    
![png](Bike_files/Bike_81_1.png)
    


let's see the behaviour of member and casual in months 


```python
table = bike.groupby(['month', 'member_casual']).agg({'ride_length':['sum', 'mean']})
table.sort_values
```




    <bound method DataFrame.sort_values of                          ride_length             
                                     sum         mean
    month     member_casual                          
    April     casual         224004050.0  1771.945624
              member         168822497.0   689.544247
    August    casual         631206095.0  1758.606543
              member         342910399.0   803.053805
    December  casual          98306691.0  1409.657446
              member         117402011.0   660.296346
    February  casual          34319291.0  1602.507051
              member          64459262.0   684.331766
    January   casual          33755428.0  1822.647300
              member          61285423.0   718.890592
    July      casual         713316133.0  1756.698312
              member         343592953.0   823.109225
    June      casual         710814978.0  1926.061650
              member         336122603.0   839.985213
    March     casual         175930668.0  1957.351505
              member         139309748.0   717.499732
    May       casual         519378154.0  1852.176788
              member         284263503.0   802.000612
    November  casual         148446469.0  1388.271367
              member         171746973.0   678.710341
    October   casual         442568951.0  1720.438152
              member         280534421.0   750.124126
    September casual         607296075.0  1668.900148
              member         323252043.0   824.082280>



- > the sum value for casual is 713316133 located in July, and mean is 1957.35 located at March.
- > the sum value for member is 343592953.0 in July, and thier mean is 839.98 at June

- chart(9): The averge ride of each the member and casual over the Months





```python
plt.figure(figsize=(10,8))
ax = sns.barplot(x='month', 
                 y='ride_length', hue='member_casual',  
                  data=bike, palette="tab10",)

ax.set(title = "The averge ride of each the member and casual over the Months", 
       xlabel = "months", ylabel = "average ride")
```




    [Text(0, 0.5, 'average ride'),
     Text(0.5, 0, 'months'),
     Text(0.5, 1.0, 'The averge ride of each the member and casual over the Months')]




    
![png](Bike_files/Bike_86_1.png)
    


chart(10): The Total ride  of each the member and casual over the Months



```python
plt.figure(figsize=(10,8))
ax = sns.barplot(x='month', 
                 y='ride_length', hue='member_casual',  
                  data=bike, palette="tab10", estimator=sum)

ax.set(title = "The sum ride of each the member and casual over the Months", 
       xlabel = "months", ylabel = "average ride")
```




    [Text(0, 0.5, 'average ride'),
     Text(0.5, 0, 'months'),
     Text(0.5, 1.0, 'The sum ride of each the member and casual over the Months')]




    
![png](Bike_files/Bike_88_1.png)
    


*RECOMMENDATIONS**
-Based on the findings 
- > first thing of my recommendations is we need to do more surveys on times that the casual is more or equal to member because it is important to know why some casual users didn't prefer to be casual 
- > we need to do some marketing campings at times that there huge demand by casual 
- > offer some discount for the membership especially for long rides and during summer and increasing the cost of casual for the long ride

EXPORT SUMMARY FILE FOR FURTHER ANALYSIS



```python
bike.head()
```





  <div id="df-06ef0824-d7b5-45d6-99cc-424b7e49137b">
    <div class="colab-df-container">
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
      <th>ride_id</th>
      <th>rideable_type</th>
      <th>started_at</th>
      <th>ended_at</th>
      <th>start_station_name</th>
      <th>start_station_id</th>
      <th>end_station_name</th>
      <th>end_station_id</th>
      <th>start_lat</th>
      <th>start_lng</th>
      <th>...</th>
      <th>member_casual</th>
      <th>day</th>
      <th>month</th>
      <th>year</th>
      <th>day_of_week</th>
      <th>hour</th>
      <th>minute</th>
      <th>second</th>
      <th>ride_length</th>
      <th>ride_lengthMean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9DC7B962304CBFD8</td>
      <td>electric_bike</td>
      <td>2021-09-28 16:07:10</td>
      <td>2021-09-28 16:09:54</td>
      <td>Clark St &amp; Grace St</td>
      <td>TA1307000127</td>
      <td>Desplaines St &amp; Kinzie St</td>
      <td>TA1306000003</td>
      <td>41.89</td>
      <td>-87.68</td>
      <td>...</td>
      <td>casual</td>
      <td>28</td>
      <td>9</td>
      <td>2021</td>
      <td>Tuesday</td>
      <td>16</td>
      <td>7</td>
      <td>10</td>
      <td>164.0</td>
      <td>1185.211725</td>
    </tr>
    <tr>
      <th>1</th>
      <td>F930E2C6872D6B32</td>
      <td>electric_bike</td>
      <td>2021-09-28 14:24:51</td>
      <td>2021-09-28 14:40:05</td>
      <td>Clark St &amp; Grace St</td>
      <td>TA1307000127</td>
      <td>Desplaines St &amp; Kinzie St</td>
      <td>TA1306000003</td>
      <td>41.94</td>
      <td>-87.64</td>
      <td>...</td>
      <td>casual</td>
      <td>28</td>
      <td>9</td>
      <td>2021</td>
      <td>Tuesday</td>
      <td>14</td>
      <td>24</td>
      <td>51</td>
      <td>914.0</td>
      <td>1185.211725</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6EF72137900BB910</td>
      <td>electric_bike</td>
      <td>2021-09-28 00:20:16</td>
      <td>2021-09-28 00:23:57</td>
      <td>Clark St &amp; Grace St</td>
      <td>TA1307000127</td>
      <td>Desplaines St &amp; Kinzie St</td>
      <td>TA1306000003</td>
      <td>41.81</td>
      <td>-87.72</td>
      <td>...</td>
      <td>casual</td>
      <td>28</td>
      <td>9</td>
      <td>2021</td>
      <td>Tuesday</td>
      <td>0</td>
      <td>20</td>
      <td>16</td>
      <td>221.0</td>
      <td>1185.211725</td>
    </tr>
    <tr>
      <th>3</th>
      <td>78D1DE133B3DBF55</td>
      <td>electric_bike</td>
      <td>2021-09-28 14:51:17</td>
      <td>2021-09-28 15:00:06</td>
      <td>Clark St &amp; Grace St</td>
      <td>TA1307000127</td>
      <td>Desplaines St &amp; Kinzie St</td>
      <td>TA1306000003</td>
      <td>41.80</td>
      <td>-87.72</td>
      <td>...</td>
      <td>casual</td>
      <td>28</td>
      <td>9</td>
      <td>2021</td>
      <td>Tuesday</td>
      <td>14</td>
      <td>51</td>
      <td>17</td>
      <td>529.0</td>
      <td>1185.211725</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E03D4ACDCAEF6E00</td>
      <td>electric_bike</td>
      <td>2021-09-28 09:53:12</td>
      <td>2021-09-28 10:03:44</td>
      <td>Clark St &amp; Grace St</td>
      <td>TA1307000127</td>
      <td>Desplaines St &amp; Kinzie St</td>
      <td>TA1306000003</td>
      <td>41.88</td>
      <td>-87.74</td>
      <td>...</td>
      <td>casual</td>
      <td>28</td>
      <td>9</td>
      <td>2021</td>
      <td>Tuesday</td>
      <td>9</td>
      <td>53</td>
      <td>12</td>
      <td>632.0</td>
      <td>1185.211725</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 22 columns</p>
</div>
      <button class="colab-df-convert" onclick="convertToInteractive('df-06ef0824-d7b5-45d6-99cc-424b7e49137b')"
              title="Convert this dataframe to an interactive table."
              style="display:none;">

  <svg xmlns="http://www.w3.org/2000/svg" height="24px"viewBox="0 0 24 24"
       width="24px">
    <path d="M0 0h24v24H0V0z" fill="none"/>
    <path d="M18.56 5.44l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94zm-11 1L8.5 8.5l.94-2.06 2.06-.94-2.06-.94L8.5 2.5l-.94 2.06-2.06.94zm10 10l.94 2.06.94-2.06 2.06-.94-2.06-.94-.94-2.06-.94 2.06-2.06.94z"/><path d="M17.41 7.96l-1.37-1.37c-.4-.4-.92-.59-1.43-.59-.52 0-1.04.2-1.43.59L10.3 9.45l-7.72 7.72c-.78.78-.78 2.05 0 2.83L4 21.41c.39.39.9.59 1.41.59.51 0 1.02-.2 1.41-.59l7.78-7.78 2.81-2.81c.8-.78.8-2.07 0-2.86zM5.41 20L4 18.59l7.72-7.72 1.47 1.35L5.41 20z"/>
  </svg>
      </button>

  <style>
    .colab-df-container {
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
    }

    .colab-df-convert {
      background-color: #E8F0FE;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      display: none;
      fill: #1967D2;
      height: 32px;
      padding: 0 0 0 0;
      width: 32px;
    }

    .colab-df-convert:hover {
      background-color: #E2EBFA;
      box-shadow: 0px 1px 2px rgba(60, 64, 67, 0.3), 0px 1px 3px 1px rgba(60, 64, 67, 0.15);
      fill: #174EA6;
    }

    [theme=dark] .colab-df-convert {
      background-color: #3B4455;
      fill: #D2E3FC;
    }

    [theme=dark] .colab-df-convert:hover {
      background-color: #434B5C;
      box-shadow: 0px 1px 3px 1px rgba(0, 0, 0, 0.15);
      filter: drop-shadow(0px 1px 2px rgba(0, 0, 0, 0.3));
      fill: #FFFFFF;
    }
  </style>

      <script>
        const buttonEl =
          document.querySelector('#df-06ef0824-d7b5-45d6-99cc-424b7e49137b button.colab-df-convert');
        buttonEl.style.display =
          google.colab.kernel.accessAllowed ? 'block' : 'none';

        async function convertToInteractive(key) {
          const element = document.querySelector('#df-06ef0824-d7b5-45d6-99cc-424b7e49137b');
          const dataTable =
            await google.colab.kernel.invokeFunction('convertToInteractive',
                                                     [key], {});
          if (!dataTable) return;

          const docLinkHtml = 'Like what you see? Visit the ' +
            '<a target="_blank" href=https://colab.research.google.com/notebooks/data_table.ipynb>data table notebook</a>'
            + ' to learn more about interactive tables.';
          element.innerHTML = '';
          dataTable['output_type'] = 'display_data';
          await google.colab.output.renderOutput(dataTable, element);
          const docLink = document.createElement('div');
          docLink.innerHTML = docLinkHtml;
          element.appendChild(docLink);
        }
      </script>
    </div>
  </div>





```python
week = bike.groupby(['member_casual','day_of_week'])['ride_length'].agg(['mean', 'sum'])
day = bike.groupby(['member_casual','day'])['ride_length'].agg(['mean', 'sum'])
month = bike.groupby(['member_casual','month'])['ride_length'].agg(['mean', 'sum'])
hour = bike.groupby(['member_casual','hour'])['ride_length'].agg(['mean', 'sum'])

week.to_csv('bike_week.csv', index=False)
day.to_csv('bike_day.csv', index=False)
month.to_csv('bike_month.csv', index=False)
hour.to_csv('bike_hour.csv', index=False)
```


```python
bike.columns

```




    Index(['ride_id', 'rideable_type', 'started_at', 'ended_at',
           'start_station_name', 'start_station_id', 'end_station_name',
           'end_station_id', 'start_lat', 'start_lng', 'end_lat', 'end_lng',
           'member_casual', 'day', 'month', 'year', 'day_of_week', 'hour',
           'minute', 'second', 'ride_length', 'ride_lengthMean'],
          dtype='object')




```python

```
