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
