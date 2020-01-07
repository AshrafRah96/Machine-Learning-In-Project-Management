# Machine Learning In Project Managment

Am I performing as expected to reach my target date?
By how many days am I over or under achieving?


```python
import pyodbc 
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

from random import randrange
import datetime 
from datetime import timedelta

plt.style.use('fivethirtyeight')
%matplotlib inline

server = 'devenviroment.database.windows.net'
database = 'DemoDB'
username = 'ashraf'
password = 'xxxxxx
driver= '{ODBC Driver 17 for SQL Server}'

cnxn = pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password)
```

Connecting to Azure DB


```python
task_df = pd.read_sql_query("SELECT * FROM [dbo].[Tasks]", cnxn)
task_df.head()
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
      <th>project_id</th>
      <th>task_id</th>
      <th>parent_task_id</th>
      <th>parent_task_name</th>
      <th>project_name</th>
      <th>task_actual_cost</th>
      <th>task_actual_duration</th>
      <th>task_actual_finish_date</th>
      <th>task_actual_fixed_cost</th>
      <th>task_actual_overtime_cost</th>
      <th>...</th>
      <th>task_remaining_cost</th>
      <th>task_remaining_duration</th>
      <th>task_remaining_overtime_cost</th>
      <th>task_remaining_overtime_work</th>
      <th>task_remaining_regular_cost</th>
      <th>task_remaining_regular_work</th>
      <th>task_remaining_work</th>
      <th>task_start_date</th>
      <th>task_start_date_string</th>
      <th>task_work</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>0f88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>0d88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Cactus Traffic Sensor</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>NaT</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>...</td>
      <td>44200.0</td>
      <td>520.0</td>
      <td>0</td>
      <td>0</td>
      <td>44200.0</td>
      <td>680.0</td>
      <td>1901-11-10 00:00:00</td>
      <td>2020-01-13 08:00:00</td>
      <td>None</td>
      <td>680.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1088c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>0f88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Phase 1 - Strategic Plan</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>NaT</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>...</td>
      <td>8320.0</td>
      <td>80.0</td>
      <td>0</td>
      <td>0</td>
      <td>8320.0</td>
      <td>128.0</td>
      <td>1900-05-07 00:00:00</td>
      <td>2020-01-13 08:00:00</td>
      <td>None</td>
      <td>128.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1188c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>1088c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Assessment</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>NaT</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>...</td>
      <td>2080.0</td>
      <td>32.0</td>
      <td>0</td>
      <td>0</td>
      <td>2080.0</td>
      <td>32.0</td>
      <td>1900-02-01 00:00:00</td>
      <td>2020-01-13 08:00:00</td>
      <td>None</td>
      <td>32.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1288c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>1088c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Assessment</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>NaT</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>...</td>
      <td>6240.0</td>
      <td>48.0</td>
      <td>0</td>
      <td>0</td>
      <td>6240.0</td>
      <td>96.0</td>
      <td>1900-04-05 00:00:00</td>
      <td>2020-01-17 08:00:00</td>
      <td>None</td>
      <td>96.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1488c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>0f88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Phase 1 - Strategic Plan</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>NaT</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>...</td>
      <td>7800.0</td>
      <td>88.0</td>
      <td>0</td>
      <td>0</td>
      <td>7800.0</td>
      <td>120.0</td>
      <td>1900-04-29 00:00:00</td>
      <td>2020-01-27 08:00:00</td>
      <td>None</td>
      <td>120.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 46 columns</p>
</div>



# Clean the dataset

Find null values and either replace or remove based on the number of values which are Null/Empty


```python
task_df.isnull().sum()
```




    project_id                         0
    task_id                            0
    parent_task_id                     0
    parent_task_name                   0
    project_name                       0
    task_actual_cost                   0
    task_actual_duration               0
    task_actual_finish_date         4244
    task_actual_fixed_cost             0
    task_actual_overtime_cost          0
    task_actual_overtime_work          0
    task_actual_regular_cost           0
    task_actual_regular_work           0
    task_actual_start_date          4079
    task_actual_work                   0
    task_cost                          0
    task_cost_variance                 0
    task_created_date                  0
    task_deadline                   4962
    task_duration                      0
    task_duration_is_estimated         0
    task_duration_string            4989
    task_duration_variance             0
    task_early_finish                209
    task_early_start                 209
    task_finish_date                 209
    task_fixed_cost                    0
    task_is_critical                   0
    task_late_finish                 209
    task_late_start                  209
    task_name                          0
    task_percent_completed             0
    task_percent_work_completed        0
    task_priority                      0
    task_regular_cost                  0
    task_regular_work                  0
    task_remaining_cost                0
    task_remaining_duration            0
    task_remaining_overtime_cost       0
    task_remaining_overtime_work       0
    task_remaining_regular_cost        0
    task_remaining_regular_work        0
    task_remaining_work                0
    task_start_date                  209
    task_start_date_string          4989
    task_work                          0
    dtype: int64



Columns which have over 4 thousand null values are obsolete, because the machine learning model cannot handle empty variables 


```python
task_df.drop(["task_actual_finish_date", "task_actual_start_date", "task_deadline", "task_duration_string", "task_start_date_string"]
              , axis=1, inplace=True)
```

Identifying null values and replacing them with random dates between 2000 and 2019 for variety (not best practice but i needed more data to work with)


```python
def GetDatesBetween(row):
    
    if row == False:
        start = datetime.datetime(2000, 1, 1)
        end = datetime.datetime(2019, 12, 1)

        delta = end - start
        int_delta = (delta.days * 24 * 60 * 60) + delta.seconds
        random_second = randrange(int_delta)
        return np.datetime64((start + timedelta(seconds=random_second)).strftime('%Y-%m-%d'))
    
    else:
        return row
```


```python
task_df["task_start_date"] = [GetDatesBetween(i) for i in task_df["task_start_date"].isnull()] 
```

As for the finish date, I am keeping this consistent, as I've already created random dates i do not need to apply the same process to the finsha date


```python
task_df[task_df["task_finish_date"].isnull()] = datetime.datetime(2030, 1, 1)
```


```python
task_df.dtypes
```




    project_id                              object
    task_id                                 object
    parent_task_id                          object
    parent_task_name                        object
    project_name                            object
    task_actual_cost                        object
    task_actual_duration                    object
    task_actual_fixed_cost                  object
    task_actual_overtime_cost               object
    task_actual_overtime_work               object
    task_actual_regular_cost                object
    task_actual_regular_work                object
    task_actual_work                        object
    task_cost                               object
    task_cost_variance                      object
    task_created_date                       object
    task_duration                           object
    task_duration_is_estimated              object
    task_duration_variance                  object
    task_early_finish               datetime64[ns]
    task_early_start                datetime64[ns]
    task_finish_date                datetime64[ns]
    task_fixed_cost                         object
    task_is_critical                        object
    task_late_finish                datetime64[ns]
    task_late_start                 datetime64[ns]
    task_name                               object
    task_percent_completed                  object
    task_percent_work_completed             object
    task_priority                           object
    task_regular_cost                       object
    task_regular_work                       object
    task_remaining_cost                     object
    task_remaining_duration                 object
    task_remaining_overtime_cost            object
    task_remaining_overtime_work            object
    task_remaining_regular_cost             object
    task_remaining_regular_work             object
    task_remaining_work                     object
    task_start_date                         object
    task_work                               object
    dtype: object



From the code above we can see that the numerical data is considered an object and so is the start date.
So the code below uses the "apply()" function to convert each value to numerical data (or date time)

Another option is to use the ".astype(float)" for simplicity but i found that it replaces non numerical vlaues as NaN which i do now want.


```python
def isFloat(x):
    try:
        float(x)
        return True
    except:
        return False

task_df["task_start_date"] = task_df["task_start_date"].apply(lambda x: pd.to_datetime(str(x), errors='coerce')) 
task_df["task_actual_duration"] = task_df["task_actual_duration"].apply(lambda x: float(x) if isFloat(x) else 0)
task_df["task_actual_work"] = task_df["task_actual_work"].apply(lambda x: float(x) if isFloat(x) else 0)
task_df["task_cost"] = task_df["task_cost"].apply(lambda x: float(x) if isFloat(x) else 0)
task_df["task_percent_completed"] = task_df["task_percent_completed"].apply(lambda x: float(x) if isFloat(x) else 0)
task_df["task_percent_work_completed"] = task_df["task_percent_work_completed"].apply(lambda x: float(x) if isFloat(x) else 0)
task_df["task_work"] = task_df["task_work"].apply(lambda x: float(x) if isFloat(x) else 0)
task_df["task_regular_work"] = task_df["task_regular_work"].apply(lambda x: float(x) if isFloat(x) else 0)     
```


```python
task_df.dtypes
```




    project_id                              object
    task_id                                 object
    parent_task_id                          object
    parent_task_name                        object
    project_name                            object
    task_actual_cost                        object
    task_actual_duration                   float64
    task_actual_fixed_cost                  object
    task_actual_overtime_cost               object
    task_actual_overtime_work               object
    task_actual_regular_cost                object
    task_actual_regular_work                object
    task_actual_work                       float64
    task_cost                              float64
    task_cost_variance                      object
    task_created_date                       object
    task_duration                           object
    task_duration_is_estimated              object
    task_duration_variance                  object
    task_early_finish               datetime64[ns]
    task_early_start                datetime64[ns]
    task_finish_date                datetime64[ns]
    task_fixed_cost                         object
    task_is_critical                        object
    task_late_finish                datetime64[ns]
    task_late_start                 datetime64[ns]
    task_name                               object
    task_percent_completed                 float64
    task_percent_work_completed            float64
    task_priority                           object
    task_regular_cost                       object
    task_regular_work                      float64
    task_remaining_cost                     object
    task_remaining_duration                 object
    task_remaining_overtime_cost            object
    task_remaining_overtime_work            object
    task_remaining_regular_cost             object
    task_remaining_regular_work             object
    task_remaining_work                     object
    task_start_date                 datetime64[ns]
    task_work                              float64
    dtype: object



Machine learning models cannot handle datetime values so a different a approach is required. 
Instead of using the task start and finish dates as features, I have created a columns called "total days" which is a numerical value.

I wanted to also apply the Task_cost as part of my independant variable but the value was too correlated with Task_work. For example "as the task cost increases so does the Task_work" this will cause the root mean squared error to be 0.99 which is the result of a data leak and/or overfitting (see graph below)

To workaround this i will instead use the average cost of each task. 


```python
a = ["task_cost"]
b = ["task_work"]

plt.figure(num=None, figsize=(18, 50), dpi=80, facecolor='w', edgecolor='k')

for idx, variable in enumerate(a):
    ax = plt.subplot(16, 3, idx+1)
    ax.set_title(variable)
    ax.scatter(task_df[b],task_df["task_work"].tolist(),alpha=0.1)
    
    plt.subplots_adjust(wspace=0.5, hspace=0.5)
```


![png](output_19_0.png)



```python
task_df["total_days"] = (task_df["task_finish_date"].dt.date - task_df["task_start_date"].dt.date).astype('timedelta64[D]')
task_df["average_cost"] = task_df["task_cost"] // task_df["total_days"]
task_df[task_df["average_cost"].isnull()] = task_df["average_cost"].mean() 
```


```python
task_df.head()
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
      <th>project_id</th>
      <th>task_id</th>
      <th>parent_task_id</th>
      <th>parent_task_name</th>
      <th>project_name</th>
      <th>task_actual_cost</th>
      <th>task_actual_duration</th>
      <th>task_actual_fixed_cost</th>
      <th>task_actual_overtime_cost</th>
      <th>task_actual_overtime_work</th>
      <th>...</th>
      <th>task_remaining_duration</th>
      <th>task_remaining_overtime_cost</th>
      <th>task_remaining_overtime_work</th>
      <th>task_remaining_regular_cost</th>
      <th>task_remaining_regular_work</th>
      <th>task_remaining_work</th>
      <th>task_start_date</th>
      <th>task_work</th>
      <th>total_days</th>
      <th>average_cost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>0f88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>0d88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Cactus Traffic Sensor</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>520</td>
      <td>0</td>
      <td>0</td>
      <td>44200</td>
      <td>680</td>
      <td>1901-11-10 00:00:00</td>
      <td>2011-09-26 00:00:00</td>
      <td>680.0</td>
      <td>3119.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1088c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>0f88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Phase 1 - Strategic Plan</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>80</td>
      <td>0</td>
      <td>0</td>
      <td>8320</td>
      <td>128</td>
      <td>1900-05-07 00:00:00</td>
      <td>2007-07-02 00:00:00</td>
      <td>128.0</td>
      <td>4589.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1188c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>1088c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Assessment</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>32</td>
      <td>0</td>
      <td>0</td>
      <td>2080</td>
      <td>32</td>
      <td>1900-02-01 00:00:00</td>
      <td>2006-02-28 00:00:00</td>
      <td>32.0</td>
      <td>5070.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1288c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>1088c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Assessment</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>48</td>
      <td>0</td>
      <td>0</td>
      <td>6240</td>
      <td>96</td>
      <td>1900-04-05 00:00:00</td>
      <td>2003-10-31 00:00:00</td>
      <td>96.0</td>
      <td>5929.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1488c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>0f88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Phase 1 - Strategic Plan</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>88</td>
      <td>0</td>
      <td>0</td>
      <td>7800</td>
      <td>120</td>
      <td>1900-04-29 00:00:00</td>
      <td>2019-10-18 00:00:00</td>
      <td>120.0</td>
      <td>115.0</td>
      <td>67.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 43 columns</p>
</div>




```python
task_df.describe()
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
      <th>task_actual_duration</th>
      <th>task_actual_work</th>
      <th>task_cost</th>
      <th>task_percent_completed</th>
      <th>task_percent_work_completed</th>
      <th>task_regular_work</th>
      <th>task_work</th>
      <th>total_days</th>
      <th>average_cost</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>count</td>
      <td>4989.000000</td>
      <td>4989.000000</td>
      <td>4.989000e+03</td>
      <td>4989.000000</td>
      <td>4989.000000</td>
      <td>4989.000000</td>
      <td>4989.000000</td>
      <td>4989.000000</td>
      <td>4989.000000</td>
    </tr>
    <tr>
      <td>mean</td>
      <td>28.311581</td>
      <td>32.348504</td>
      <td>1.221005e+04</td>
      <td>16.476756</td>
      <td>16.356090</td>
      <td>184.303372</td>
      <td>184.303372</td>
      <td>3664.542701</td>
      <td>4.528870</td>
    </tr>
    <tr>
      <td>std</td>
      <td>150.408731</td>
      <td>227.956038</td>
      <td>4.984518e+04</td>
      <td>36.057730</td>
      <td>36.002013</td>
      <td>749.637061</td>
      <td>749.637061</td>
      <td>2215.983558</td>
      <td>120.706317</td>
    </tr>
    <tr>
      <td>min</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000e+00</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>-238.000000</td>
      <td>-7340.000000</td>
    </tr>
    <tr>
      <td>25%</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>5.200000e+02</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>8.000000</td>
      <td>8.000000</td>
      <td>1741.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>50%</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>2.080000e+03</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>32.000000</td>
      <td>32.000000</td>
      <td>3679.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>75%</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>7.200000e+03</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>108.000000</td>
      <td>108.000000</td>
      <td>5563.000000</td>
      <td>3.000000</td>
    </tr>
    <tr>
      <td>max</td>
      <td>1997.241667</td>
      <td>5704.000000</td>
      <td>1.442275e+06</td>
      <td>100.000000</td>
      <td>100.000000</td>
      <td>22188.848333</td>
      <td>22188.848333</td>
      <td>8255.000000</td>
      <td>2888.000000</td>
    </tr>
  </tbody>
</table>
</div>



 Investigate and analyze data, finding the appropriate features to predict task_work


```python
independent_variables = ["task_actual_duration", "task_actual_work","total_days", "average_cost"]
dependent_variable = ["task_work"]

plt.figure(num=None, figsize=(18, 50), dpi=80, facecolor='w', edgecolor='k')

for idx, variable in enumerate(independent_variables):
    ax = plt.subplot(16, 3, idx+1)
    ax.set_title(variable)
    ax.scatter(task_df[variable].tolist(),task_df["task_work"].tolist(),alpha=0.1)
    
    plt.subplots_adjust(wspace=0.5, hspace=0.5)
```


![png](output_24_0.png)


Split data for training, using a 80/20 split so i dont over fit the test sample


```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
from sklearn import model_selection
from sklearn.metrics import r2_score
from sklearn.model_selection import cross_val_score
```


```python
X = task_df[independent_variables] # features
y = task_df[dependent_variable] # label
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

Now its time to identify which model will be the best fit my dataset 

I have decided to use Linear regression as I have continuase data.

Linear Regression: Describes the relationship between a set of variables and a real value outcome

Rather than:

Logistic Regression: Used to find a probability of a yes or no (1 or 0) typically used with categorical data

KNN: A classification model that uses the "K" most similar observations to make a prediction 


-------------------------------------------------------------------------------------------------------------------------------

The model was unsuccessful as the R2_score is 0.075

R-Squared is a statistical measure of fit that indicates how much variation of a dependent variable is explained by the independent variable(s) in a regression model

By the explanation the model has a less then 1% fit.


```python
linreg = LinearRegression() 
linreg.fit(X_train, y_train)
y_pred = linreg.predict(X_test)
r2_score(y_test, y_pred)
```




    0.0758732885719221



The code bellow is the detailed view of how cross validation is working, it is essentially cross validating the test variable with the train variable, to accuratley represent how the model with behave with the practice sample.

fold_number = 1

linreg = LinearRegression()
accuracies = []
kf = model_selection.KFold(n_splits=5, shuffle=True)

for train_index, test_index in kf.split(X,y):
    # Print out the indexes of the training and testing sets for the current 'fold'
    #print("\n\nFold ", fold_number, "\nTraining indexes:", train_index, "\nTesting indexes:", test_index)
    X_arr = np.array(X)
    y_arr = np.array(y)

    X_train, X_test = X_arr[train_index].reshape(-1,4), X_arr[test_index].reshape(-1,4)
    y_train, y_test = y_arr[train_index], y_arr[test_index]
    
    #X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=1)
    
    linreg.fit(X_train,y_train)
    y_pred = linreg.predict(X_test)
    
    print("\nFold number", fold_number, "| Accuracies =", r2_score(y_test, y_pred))
    #use r^2
    
    fold_number+=1

However there are simpler methods to perform the same action


```python
cv_results = cross_val_score(linreg, X_train, y_train, cv=5)
print(cv_results)
```

    [ 0.29144678  0.3385442  -0.0010564   0.47450955  0.52624773]
    

## Now predicting with XGBoost

It is a highly flexible and versatile tool that can work through most regression, classification and ranking problems as well as user-built objective functions.

Features include:

    Gradiant boosting: Learning rate
    
    Continued Training so that you can further boost an already fitted model on new data
    
    Performance and strong execution speed, sutable for large datasets
    
Reference: https://machinelearningmastery.com/gentle-introduction-xgboost-applied-machine-learning/


```python
import xgboost as xgb
```


```python
model = xgb.XGBRegressor(objective="reg:squarederror", random_state=42, max_depth=2, n_estimators=80,  min_child_weight=5, 
                         subsample=0.6)
model.fit(X_train, y_train)
y_pred_train = model.predict(X_train)
y_pred_test = model.predict(X_test)
r2_score(y_train, y_pred_train), r2_score(y_test, y_pred_test)
```




    (0.8434255702879656, 0.7495243581533785)



Deatiled View

fold_number = 1

model = xgb.XGBRegressor(objective="reg:squarederror", random_state=42)
accuracies = []
kf = model_selection.KFold(n_splits=5, shuffle=True)

for train_index, test_index in kf.split(X,y):
    #Print out the indexes of the training and testing sets for the current 'fold'
    #print("\n\nFold ", fold_number, "\nTraining indexes:", train_index, "\nTesting indexes:", test_index)
    X_arr = np.array(X)
    y_arr = np.array(y)

    X_train, X_test = X_arr[train_index].reshape(-1,4), X_arr[test_index].reshape(-1,4)
    y_train, y_test = y_arr[train_index], y_arr[test_index]
    
    #X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=1)
    
    model.fit(X_train,y_train)
    y_pred = model.predict(X_test)
    
    print("\nFold number", fold_number, "| Accuracies =", r2_score(y_test, y_pred))
    #use r^2
    
    fold_number+=1

Cross validating XGBoost


```python
cv_results = cross_val_score(model, X_train, y_train, cv=3)
print(cv_results)
```

    [0.81193999 0.66607856 0.70690967]
    

Display XGBoost using graphviz

Sample:
    
    Varibales are automatically named
    
    You can see the split decisions within each node and the different colors for left and right splits (blue and red)


```python
from xgboost import plot_tree
fig, ax = plt.subplots(figsize=(35, 35))
xgb.plot_tree(model, num_trees=2, ax=ax)
plt.show()
```


![png](output_46_0.png)


I created a pkl file to store the model


```python
from joblib import load, dump
dump(model, 'ashrafs_model.pkl')
```




    ['ashrafs_model.pkl']



Now im loading the model and passing the independant variable i want to be predicted, NOTE: this is not test or training data

And creating a new columns with al predicted values


```python
model_new = load('ashrafs_model.pkl')
task_df['y_pred']= model_new.predict(X)
task_df
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
      <th>project_id</th>
      <th>task_id</th>
      <th>parent_task_id</th>
      <th>parent_task_name</th>
      <th>project_name</th>
      <th>task_actual_cost</th>
      <th>task_actual_duration</th>
      <th>task_actual_fixed_cost</th>
      <th>task_actual_overtime_cost</th>
      <th>task_actual_overtime_work</th>
      <th>...</th>
      <th>task_remaining_overtime_cost</th>
      <th>task_remaining_overtime_work</th>
      <th>task_remaining_regular_cost</th>
      <th>task_remaining_regular_work</th>
      <th>task_remaining_work</th>
      <th>task_start_date</th>
      <th>task_work</th>
      <th>total_days</th>
      <th>average_cost</th>
      <th>y_pred</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>0f88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>0d88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Cactus Traffic Sensor</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>44200</td>
      <td>680</td>
      <td>1901-11-10 00:00:00</td>
      <td>2011-09-26 00:00:00</td>
      <td>680.0</td>
      <td>3119.0</td>
      <td>14.0</td>
      <td>677.673462</td>
    </tr>
    <tr>
      <td>1</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1088c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>0f88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Phase 1 - Strategic Plan</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>8320</td>
      <td>128</td>
      <td>1900-05-07 00:00:00</td>
      <td>2007-07-02 00:00:00</td>
      <td>128.0</td>
      <td>4589.0</td>
      <td>1.0</td>
      <td>93.678665</td>
    </tr>
    <tr>
      <td>2</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1188c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>1088c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Assessment</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>2080</td>
      <td>32</td>
      <td>1900-02-01 00:00:00</td>
      <td>2006-02-28 00:00:00</td>
      <td>32.0</td>
      <td>5070.0</td>
      <td>0.0</td>
      <td>62.680740</td>
    </tr>
    <tr>
      <td>3</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1288c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>1088c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Assessment</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>6240</td>
      <td>96</td>
      <td>1900-04-05 00:00:00</td>
      <td>2003-10-31 00:00:00</td>
      <td>96.0</td>
      <td>5929.0</td>
      <td>1.0</td>
      <td>103.153214</td>
    </tr>
    <tr>
      <td>4</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1488c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>0f88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Phase 1 - Strategic Plan</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>7800</td>
      <td>120</td>
      <td>1900-04-29 00:00:00</td>
      <td>2019-10-18 00:00:00</td>
      <td>120.0</td>
      <td>115.0</td>
      <td>67.0</td>
      <td>843.338135</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>4984</td>
      <td>46173730-ee79-e711-80c7-ff2430a91da8</td>
      <td>2ece28cd-1b02-ea11-8560-000d3af939ce</td>
      <td>29ce28cd-1b02-ea11-8560-000d3af939ce</td>
      <td>Integration Testing</td>
      <td>Bradford Solid Material Laser Scanning System</td>
      <td>0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2004-03-09 00:00:00</td>
      <td>0.0</td>
      <td>6230.0</td>
      <td>0.0</td>
      <td>72.155289</td>
    </tr>
    <tr>
      <td>4985</td>
      <td>46173730-ee79-e711-80c7-ff2430a91da8</td>
      <td>3cce28cd-1b02-ea11-8560-000d3af939ce</td>
      <td>38ce28cd-1b02-ea11-8560-000d3af939ce</td>
      <td>Documentation</td>
      <td>Bradford Solid Material Laser Scanning System</td>
      <td>0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>2080</td>
      <td>32</td>
      <td>32</td>
      <td>2010-12-07 00:00:00</td>
      <td>32.0</td>
      <td>3748.0</td>
      <td>0.0</td>
      <td>55.727913</td>
    </tr>
    <tr>
      <td>4986</td>
      <td>46173730-ee79-e711-80c7-ff2430a91da8</td>
      <td>44ce28cd-1b02-ea11-8560-000d3af939ce</td>
      <td>42ce28cd-1b02-ea11-8560-000d3af939ce</td>
      <td>Pilot</td>
      <td>Bradford Solid Material Laser Scanning System</td>
      <td>0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>7800</td>
      <td>120</td>
      <td>120</td>
      <td>2006-08-26 00:00:00</td>
      <td>120.0</td>
      <td>5066.0</td>
      <td>1.0</td>
      <td>93.678665</td>
    </tr>
    <tr>
      <td>4987</td>
      <td>46173730-ee79-e711-80c7-ff2430a91da8</td>
      <td>50ce28cd-1b02-ea11-8560-000d3af939ce</td>
      <td>49ce28cd-1b02-ea11-8560-000d3af939ce</td>
      <td>Deployment</td>
      <td>Bradford Solid Material Laser Scanning System</td>
      <td>0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2007-08-10 00:00:00</td>
      <td>0.0</td>
      <td>5068.0</td>
      <td>0.0</td>
      <td>62.680740</td>
    </tr>
    <tr>
      <td>4988</td>
      <td>46173730-ee79-e711-80c7-ff2430a91da8</td>
      <td>52ce28cd-1b02-ea11-8560-000d3af939ce</td>
      <td>51ce28cd-1b02-ea11-8560-000d3af939ce</td>
      <td>Project Monitoring</td>
      <td>Bradford Solid Material Laser Scanning System</td>
      <td>0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>17225</td>
      <td>265</td>
      <td>265</td>
      <td>2000-05-26 00:00:00</td>
      <td>265.0</td>
      <td>7718.0</td>
      <td>2.0</td>
      <td>136.786804</td>
    </tr>
  </tbody>
</table>
<p>4989 rows × 44 columns</p>
</div>



I want to calculate the predicted finsh date so i first get the difference between the task work and my y_pred


```python
task_df["pred_difference"] = task_df["task_work"] - task_df['y_pred']
```

Then i add to the start date my predicted difference to get an accurate estimate of my finish date


```python
pred_finish_date = (pd.to_datetime(task_df['task_start_date'], errors='coerce') 
                    + (task_df["task_work"] - task_df["pred_difference"]).map(datetime.timedelta))
task_df["pred_finish_date"] = pred_finish_date
```

Creating a status column to check if the task finish date is as expected or running behind schedule


```python
def GetStatus(x):
    
    if x["pred_finish_date"] < x["task_finish_date"]:
        return "Red"
    elif x["pred_finish_date"] > x["task_finish_date"]:
        return "Green"
    else:
        return "Yellow"

task_df["status"] = task_df.apply(GetStatus, axis=1)
```


```python
task_df.head()
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
      <th>project_id</th>
      <th>task_id</th>
      <th>parent_task_id</th>
      <th>parent_task_name</th>
      <th>project_name</th>
      <th>task_actual_cost</th>
      <th>task_actual_duration</th>
      <th>task_actual_fixed_cost</th>
      <th>task_actual_overtime_cost</th>
      <th>task_actual_overtime_work</th>
      <th>...</th>
      <th>task_remaining_regular_work</th>
      <th>task_remaining_work</th>
      <th>task_start_date</th>
      <th>task_work</th>
      <th>total_days</th>
      <th>average_cost</th>
      <th>y_pred</th>
      <th>pred_difference</th>
      <th>pred_finish_date</th>
      <th>status</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>0f88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>0d88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Cactus Traffic Sensor</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>680</td>
      <td>1901-11-10 00:00:00</td>
      <td>2011-09-26 00:00:00</td>
      <td>680.0</td>
      <td>3119.0</td>
      <td>14.0</td>
      <td>677.673462</td>
      <td>2.326538</td>
      <td>2013-08-03 16:09:47.109375</td>
      <td>Red</td>
    </tr>
    <tr>
      <td>1</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1088c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>0f88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Phase 1 - Strategic Plan</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>128</td>
      <td>1900-05-07 00:00:00</td>
      <td>2007-07-02 00:00:00</td>
      <td>128.0</td>
      <td>4589.0</td>
      <td>1.0</td>
      <td>93.678665</td>
      <td>34.321335</td>
      <td>2007-10-03 16:17:16.669922</td>
      <td>Red</td>
    </tr>
    <tr>
      <td>2</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1188c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>1088c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Assessment</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>32</td>
      <td>1900-02-01 00:00:00</td>
      <td>2006-02-28 00:00:00</td>
      <td>32.0</td>
      <td>5070.0</td>
      <td>0.0</td>
      <td>62.680740</td>
      <td>-30.680740</td>
      <td>2006-05-01 16:20:15.966797</td>
      <td>Red</td>
    </tr>
    <tr>
      <td>3</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1288c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>1088c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Assessment</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>96</td>
      <td>1900-04-05 00:00:00</td>
      <td>2003-10-31 00:00:00</td>
      <td>96.0</td>
      <td>5929.0</td>
      <td>1.0</td>
      <td>103.153214</td>
      <td>-7.153214</td>
      <td>2004-02-11 03:40:37.646484</td>
      <td>Red</td>
    </tr>
    <tr>
      <td>4</td>
      <td>c3ec3f4b-7254-e711-80d7-00155d38270c</td>
      <td>1488c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>0f88c9f1-1a02-ea11-81ff-000d3a6dccdb</td>
      <td>Phase 1 - Strategic Plan</td>
      <td>Cactus Traffic Sensor</td>
      <td>1899-12-30 00:00:00</td>
      <td>0.0</td>
      <td>1899-12-30 00:00:00</td>
      <td>0</td>
      <td>1899-12-30 00:00:00</td>
      <td>...</td>
      <td>120</td>
      <td>1900-04-29 00:00:00</td>
      <td>2019-10-18 00:00:00</td>
      <td>120.0</td>
      <td>115.0</td>
      <td>67.0</td>
      <td>843.338135</td>
      <td>-723.338135</td>
      <td>2022-02-07 08:06:54.843750</td>
      <td>Green</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 47 columns</p>
</div>




```python
grouped = task_df["status"].value_counts()
grouped.plot.pie();
print(grouped)
```

    Red       4603
    Yellow     209
    Green      177
    Name: status, dtype: int64
    


![png](output_58_1.png)



```python
task_df.plot(x='task_work', y='y_pred', style='o')  
plt.title('Task Work vs Predicted Work')  
plt.xlabel('Task Work')  
plt.ylabel('Predicted Work')  
plt.show()
```


![png](output_59_0.png)



```python
df = pd.DataFrame({'Actual': task_df["task_work"], 'Predicted': task_df["y_pred"]})
df1 = df.head(25)

df1.plot(kind='bar',figsize=(10,8))
plt.grid(which='major', linestyle='-', linewidth='0.5', color='green')
plt.grid(which='minor', linestyle=':', linewidth='0.5', color='black')
plt.show()
```


![png](output_60_0.png)



```python
export_cvs = task_df.to_csv("./data/PredictionResults.csv", index = None, header=True)
```

