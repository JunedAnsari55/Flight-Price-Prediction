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
df.head()
