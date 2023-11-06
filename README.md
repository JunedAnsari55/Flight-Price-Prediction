# Flight-Price-Prediction

**Overview**

We will conduct an analysis of flight fare prediction using a machine learning dataset. This analysis will involve employing fundamental data analysis techniques to make predictions about flight prices, considering various features like airline type, arrival and departure times, and other relevant factors.

**About the Dataset**

The dataset comprises 11 columns, namely Airline, Date_of_journey, Source, Destination, Route, Arrival_time, Duration, Total_stops, Additional info, and Price. It contains a total of 10,683 rows and 11 columns.

# Importing the libraries

```ruby
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
sns.set()
```
## Importing dataset
```ruby
df = pd.read_excel(r"Data_Train.xlsx")
```
To display all the columns in the dataset in the excel file
```ruby
pd.set_option('display.max_columns', None)
```
```ruby
df.head()
```
![1](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/fd6fc277-7f91-4f89-8a14-8c2b021e9bd1)
```ruby
df.info()
```

![2](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/1f7d2b2a-da09-47e0-b98a-2f29a5a9ccb7)
```ruby
print(df["Duration"].value_counts())
df["Duration"].value_counts().plot()
```
![3](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/71048ab7-12cf-4895-855a-0dcdb8247df3)
```ruby
df.dropna(inplace = True)
df.isnull().sum()
```
![4](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/1cad8481-bb6a-49dd-aa49-3aeb3bdb7f9c)


#EDA

To extract the date from the "Date of Journey" column 
```ruby
df["Journey_day"] = pd.to_datetime(df.Date_of_Journey, format="%d/%m/%Y").dt.day
```
To extract the month from the "Date of Journey" column
```ruby
df["Journey_month"] = pd.to_datetime(df["Date_of_Journey"], format = "%d/%m/%Y").dt.month
```
Displaying newly added columns Journey_day and Journey_month
```ruby
df.head()
```
![5](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/e425193a-186d-4bea-a429-b8614b657bf7)

 Since we have converted Date_of_Journey column into integers, Now we can drop as it is of no use.
```ruby

df.drop(["Date_of_Journey"], axis = 1, inplace = True)
```
 Similar to Date_of_Journey we can extract values from Dep_Time

To extract the hours from the "Dep_Time" column and adding a new column named Dep_hour
```ruby
df["Dep_hour"] = pd.to_datetime(df["Dep_Time"]).dt.hour
```
To extract the minutes from the "Dep_Time" column and adding a new column named Dep_min
```ruby
df["Dep_min"] = pd.to_datetime(df["Dep_Time"]).dt.minute
```
 Dropping the column Dep_Time 
```ruby
df.drop(["Dep_Time"], axis = 1, inplace = True)
df.head()
```
![6](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/810460d6-8c1f-449e-a316-d5a264eb587a)

 Similar to Date_of_Journey we can extract values from Arrival_Time

To extract the hours from the "Dep_Time" column and adding a new column named Arrival_hour
```ruby
df["Arrival_hour"] = pd.to_datetime(df.Arrival_Time).dt.hour
```
To extract the minutes from the "Dep_Time" column and adding a new column named Arrival_min
```ruby
df["Arrival_min"] = pd.to_datetime(df.Arrival_Time).dt.minute
```
 Dropping column Arrival_Time
```ruby
df.drop(["Arrival_Time"], axis = 1, inplace = True)
```
```ruby
df.head()
```
![7](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/b84aaee0-d4a8-49a7-b85d-3c57ea4a3f21)

 Journey time is equal to the differnce between Departure Time and Arrival time

 Assigning and converting Duration column into list
```ruby
journey_time = list(df["Duration"])

for i in range(len(journey_time)):
    if len(journey_time[i].split()) != 2:    # Check if duration contains only hour or mins
        if "h" in journey_time[i]:
            journey_time[i] = journey_time[i].strip() + " 0m"   # Adds 0 minute
        else:
            journey_time[i] = "0h " + journey_time[i]           # Adds 0 hour


journey_time_hours = []
journey_time_mins = []
for i in range(len(journey_time)):
    journey_time_hours.append(int(journey_time[i].split(sep = "h")[0]))    # Extract hours from duration
    journey_time_mins.append(int(journey_time[i].split(sep = "m")[0].split()[-1]))   # Extracts only minutes from duration
    # Adding journey_time_hours and journey_time_mins list to df dataframe
```
```ruby
df["Journey_time_hours"] = journey_time_hours
df["Journey_time_mins"] = journey_time_mins
```
```ruby
df.drop(["Duration"], axis = 1, inplace = True)
df.head(30)
```


![8](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/b83eba4f-436b-4720-9418-08cd5a4ca0b7)
