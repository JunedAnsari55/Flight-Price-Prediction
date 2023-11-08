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
    journey_time_mins.append(int(journey_time[i].split(sep = "m")[0].split()[-1]))   # Extr
    
```ruby
df["Journey_time_hours"] = journey_time_hours
df["Journey_time_mins"] = journey_time_mins
```
```ruby
df.drop(["Duration"], axis = 1, inplace = True)
df.head(30)
```


![8](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/b83eba4f-436b-4720-9418-08cd5a4ca0b7)


**Handling Categorical Data**
Here to handle categorical data we use **OneHotEncoding** because the variables do not have any relation with each other 
```ruby

df["Airline"].value_counts()
```
![9](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/c5dd1bae-08a2-4ae8-b365-511196fcbba9)

From graph we can see that Jet Airways Business have the highest Price.
 Apart from the first Airline almost all are having similar median

 Airline vs Price
```ruby
sns.catplot(y = "Price", x = "Airline", data = df.sort_values("Price", ascending = False), kind="boxen", height = 7, aspect = 3)
plt.show()
```
![10](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/ebfc0082-014d-4396-bf05-1a01f9f335d9)

 Airline is Nominal Categorical data, OneHotEncoding is performed
```ruby

Airline = df[["Airline"]]

Airline = pd.get_dummies(Airline, drop_first= True)![9](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/c287e6e6-4264-4b52-98a4-09e829218ec7)
```
![11](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/784f596e-5408-467c-bebf-a8f32eb41c12)

```ruby
Airline.head()

df["Source"].value_counts()
```
![12](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/e97733cd-572f-4270-bc28-37f0912b8fd4)

 Source vs Price
```ruby

sns.catplot(y = "Price", x = "Source", data = df.sort_values("Price", ascending = False), kind="boxen", height = 4, aspect = 3)
plt.show()
```
![13](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/95ac7d19-a06c-48f7-84d0-0ec9d2b0845e)

 As Source is Nominal Categorical data we will perform OneHotEncoding
```ruby

Source = df[["Source"]]

Source = pd.get_dummies(Source, drop_first= True)

Source.head()

df["Destination"].value_counts()
```
![14](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/280a9035-b1bd-44a2-b60b-2bd3fc74ac84)

 As Destination is Nominal Categorical data we will perform OneHotEncoding
```ruby

Destination = df[["Destination"]]

Destination = pd.get_dummies(Destination, drop_first = True)

Destination.head()
```
![15](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/daff2ac5-ef0d-40cd-9a5c-724820af3a59)

 Additional_Info contains almost 80% no_info
 Route and Total_Stops are related to each other
```ruby

df.drop(["Route", "Additional_Info"], axis = 1, inplace = True)
```
 As this is case of Ordinal Categorical type we perform LabelEncoder
 Here Values are assigned with corresponding keys
```ruby

df.replace({"non-stop": 0, "1 stop": 1, "2 stops": 2, "3 stops": 3, "4 stops": 4}, inplace = True)
```
![16](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/8eedc338-8b13-414b-afb1-c4198e13983a)

 Concatenate dataframe --> df + Airline + Source + Destination
```ruby

df_train = pd.concat([df, Airline, Source, Destination], axis = 1)

df_train.drop(["Airline", "Source", "Destination"], axis = 1, inplace = True)

df_train.head()
```
![17](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/e9fb9f88-9683-4a09-8daa-964203304237)


**Feature Selection**

Finding out the best feature which will contribute and have good relation with target variable.
Following are some of the feature selection methods,

 Finds correlation between Independent and dependent attributes
```ruby

plt.figure(figsize = (10,10))
sns.heatmap(df.corr(), annot = True,cmap="YlGnBu" )

plt.show()

```
![18](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/85a73d39-f103-49e5-8368-0d9c460e09b3)


 Important feature using ExtraTreesRegressor
```ruby

from sklearn.ensemble import ExtraTreesRegressor
selection = ExtraTreesRegressor()
selection.fit(X, y)
print(selection.feature_importances_)

```
![19](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/83b6f8dc-5d3e-4a98-b8a6-20782e94a758)

plot graph of feature importances for better visualization
```ruby

plt.figure(figsize = (12,8))
feature_imp = pd.Series(selection.feature_importances_, index=X.columns)
feature_imp.nlargest(20).plot(kind='barh')
plt.show()

```
![20](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/1126e4c7-4556-48c4-97e3-c9257069dd04)

**Fitting model using Random Forest**
Why Random forest?

Random Forest uses Decision trees to process the and train the data. A number of decision trees are generated and the agregate of those decisions are chosen to finalize the random forest method results.

The reason why we do not use the decision tree classification is that it has its disadvantages such as overfitting and bias. To overcome that issues the Random forest method is used which reduces the overfitting and bias.
```ruby

from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.17, random_state = 16)
from sklearn.ensemble import RandomForestRegressor
reg_rf = RandomForestRegressor()
reg_rf.fit(X_train, y_train)
```

```ruby

y_pred = reg_rf.predict(X_test)
X_test

```
```ruby

y_pred

```
```ruby
reg_rf.score(X_train, y_train)
reg_rf.score(X_test, y_test)
y_pred = y_pred.reshape(1816,1)

```

```ruby
sns.displot(y_test-y_pred)
plt.show()
```
![21](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/2502b7df-ba62-4411-b508-aeee448f66ad)


```ruby
plt.scatter(y_test, y_pred, alpha = 0.5)
plt.xlabel("y_test")
plt.ylabel("y_pred")
plt.show()
```
![22](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/a91aac5e-d33f-4f23-a3c4-f7e6be926f0f)


```ruby
from sklearn import metrics
print('MAE:', metrics.mean_absolute_error(y_test, y_pred))
print('MSE:', metrics.mean_squared_error(y_test, y_pred))
print('RMSE:', np.sqrt(metrics.mean_squared_error(y_test, y_pred)))
```
![23](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/fc1dcbb4-debc-4ac1-a77b-1fd4907c3242)

```ruby
metrics.r2_score(y_test, y_pred)

```
![24](https://github.com/JunedAnsari55/Flight-Price-Prediction/assets/145773060/2cbd4c45-2d69-4c2a-93f6-b64fc6876078)
