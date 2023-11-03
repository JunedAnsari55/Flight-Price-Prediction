Flight Price Prediction
Importing the libraries

import numpy as np

import pandas as pd

import matplotlib.pyplot as plt

import seaborn as sns

sns.set()

Importing dataset

df = pd.read_excel(r"Data_Train.xlsx")

#To display all the columns in the dataset in the excel file

pd.set_option('display.max_columns', None)

df.head()

	Airline 	Date_of_Journey 	Source 	Destination 	Route 	Dep_Time 	Arrival_Time 	Duration 	Total_Stops 	Additional_Info 	Price
0 	IndiGo 	24/03/2019 	Banglore 	New Delhi 	BLR → DEL 	22:20 	01:10 22 Mar 	2h 50m 	non-stop 	No info 	3897
1 	Air India 	1/05/2019 	Kolkata 	Banglore 	CCU → IXR → BBI → BLR 	05:50 	13:15 	7h 25m 	2 stops 	No info 	7662
2 	Jet Airways 	9/06/2019 	Delhi 	Cochin 	DEL → LKO → BOM → COK 	09:25 	04:25 10 Jun 	19h 	2 stops 	No info 	13882
3 	IndiGo 	12/05/2019 	Kolkata 	Banglore 	CCU → NAG → BLR 	18:05 	23:30 	5h 25m 	1 stop 	No info 	6218
4 	IndiGo 	01/03/2019 	Banglore 	New Delhi 	BLR → NAG → DEL 	16:50 	21:35 	4h 45m 	1 stop 	No info 	13302

df.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 10683 entries, 0 to 10682
Data columns (total 11 columns):
 #   Column           Non-Null Count  Dtype 
---  ------           --------------  ----- 
 0   Airline          10683 non-null  object
 1   Date_of_Journey  10683 non-null  object
 2   Source           10683 non-null  object
 3   Destination      10683 non-null  object
 4   Route            10682 non-null  object
 5   Dep_Time         10683 non-null  object
 6   Arrival_Time     10683 non-null  object
 7   Duration         10683 non-null  object
 8   Total_Stops      10682 non-null  object
 9   Additional_Info  10683 non-null  object
 10  Price            10683 non-null  int64 
dtypes: int64(1), object(10)
memory usage: 918.2+ KB

print(df["Duration"].value_counts())

df["Duration"].value_counts().plot()

2h 50m     550
1h 30m     386
2h 45m     337
2h 55m     337
2h 35m     329
          ... 
31h 30m      1
30h 25m      1
42h 5m       1
4h 10m       1
47h 40m      1
Name: Duration, Length: 368, dtype: int64

<AxesSubplot:>

#To drop null values and overwrite the data using inplace = true

df.dropna(inplace = True)

df.isnull().sum()

Airline            0
Date_of_Journey    0
Source             0
Destination        0
Route              0
Dep_Time           0
Arrival_Time       0
Duration           0
Total_Stops        0
Additional_Info    0
Price              0
dtype: int64

EDA

Taking the day and month out from the date column

#To extract the date from the "Date of Journey" column 

df["Journey_day"] = pd.to_datetime(df.Date_of_Journey, format="%d/%m/%Y").dt.day

#To extract the month from the "Date of Journey" column

df["Journey_month"] = pd.to_datetime(df["Date_of_Journey"], format = "%d/%m/%Y").dt.month

#Displaying newly added columns Journey_day and Journey_month

df.head()

	Airline 	Date_of_Journey 	Source 	Destination 	Route 	Dep_Time 	Arrival_Time 	Duration 	Total_Stops 	Additional_Info 	Price 	Journey_day 	Journey_month
0 	IndiGo 	24/03/2019 	Banglore 	New Delhi 	BLR → DEL 	22:20 	01:10 22 Mar 	2h 50m 	non-stop 	No info 	3897 	24 	3
1 	Air India 	1/05/2019 	Kolkata 	Banglore 	CCU → IXR → BBI → BLR 	05:50 	13:15 	7h 25m 	2 stops 	No info 	7662 	1 	5
2 	Jet Airways 	9/06/2019 	Delhi 	Cochin 	DEL → LKO → BOM → COK 	09:25 	04:25 10 Jun 	19h 	2 stops 	No info 	13882 	9 	6
3 	IndiGo 	12/05/2019 	Kolkata 	Banglore 	CCU → NAG → BLR 	18:05 	23:30 	5h 25m 	1 stop 	No info 	6218 	12 	5
4 	IndiGo 	01/03/2019 	Banglore 	New Delhi 	BLR → NAG → DEL 	16:50 	21:35 	4h 45m 	1 stop 	No info 	13302 	1 	3

# Since we have converted Date_of_Journey column into integers, Now we can drop as it is of no use.

​

df.drop(["Date_of_Journey"], axis = 1, inplace = True)

# Similar to Date_of_Journey we can extract values from Dep_Time

​

#To extract the hours from the "Dep_Time" column and adding a new column named Dep_hour

df["Dep_hour"] = pd.to_datetime(df["Dep_Time"]).dt.hour

​

#To extract the minutes from the "Dep_Time" column and adding a new column named Dep_min

df["Dep_min"] = pd.to_datetime(df["Dep_Time"]).dt.minute

​

# Dropping the column Dep_Time 

df.drop(["Dep_Time"], axis = 1, inplace = True)

df.head()

	Airline 	Source 	Destination 	Route 	Arrival_Time 	Duration 	Total_Stops 	Additional_Info 	Price 	Journey_day 	Journey_month 	Dep_hour 	Dep_min
0 	IndiGo 	Banglore 	New Delhi 	BLR → DEL 	01:10 22 Mar 	2h 50m 	non-stop 	No info 	3897 	24 	3 	22 	20
1 	Air India 	Kolkata 	Banglore 	CCU → IXR → BBI → BLR 	13:15 	7h 25m 	2 stops 	No info 	7662 	1 	5 	5 	50
2 	Jet Airways 	Delhi 	Cochin 	DEL → LKO → BOM → COK 	04:25 10 Jun 	19h 	2 stops 	No info 	13882 	9 	6 	9 	25
3 	IndiGo 	Kolkata 	Banglore 	CCU → NAG → BLR 	23:30 	5h 25m 	1 stop 	No info 	6218 	12 	5 	18 	5
4 	IndiGo 	Banglore 	New Delhi 	BLR → NAG → DEL 	21:35 	4h 45m 	1 stop 	No info 	13302 	1 	3 	16 	50

# Similar to Date_of_Journey we can extract values from Arrival_Time

​

#To extract the hours from the "Dep_Time" column and adding a new column named Arrival_hour

df["Arrival_hour"] = pd.to_datetime(df.Arrival_Time).dt.hour

​

#To extract the minutes from the "Dep_Time" column and adding a new column named Arrival_min

df["Arrival_min"] = pd.to_datetime(df.Arrival_Time).dt.minute

​

# Dropping column Arrival_Time

df.drop(["Arrival_Time"], axis = 1, inplace = True)

df.head()

	Airline 	Source 	Destination 	Route 	Duration 	Total_Stops 	Additional_Info 	Price 	Journey_day 	Journey_month 	Dep_hour 	Dep_min 	Arrival_hour 	Arrival_min
0 	IndiGo 	Banglore 	New Delhi 	BLR → DEL 	2h 50m 	non-stop 	No info 	3897 	24 	3 	22 	20 	1 	10
1 	Air India 	Kolkata 	Banglore 	CCU → IXR → BBI → BLR 	7h 25m 	2 stops 	No info 	7662 	1 	5 	5 	50 	13 	15
2 	Jet Airways 	Delhi 	Cochin 	DEL → LKO → BOM → COK 	19h 	2 stops 	No info 	13882 	9 	6 	9 	25 	4 	25
3 	IndiGo 	Kolkata 	Banglore 	CCU → NAG → BLR 	5h 25m 	1 stop 	No info 	6218 	12 	5 	18 	5 	23 	30
4 	IndiGo 	Banglore 	New Delhi 	BLR → NAG → DEL 	4h 45m 	1 stop 	No info 	13302 	1 	3 	16 	50 	21 	35

# Journey time is equal to the differnce between Departure Time and Arrival time

​

# Assigning and converting Duration column into list

journey_time = list(df["Duration"])

​

for i in range(len(journey_time)):

    if len(journey_time[i].split()) != 2:    # Check if duration contains only hour or mins

        if "h" in journey_time[i]:

            journey_time[i] = journey_time[i].strip() + " 0m"   # Adds 0 minute

        else:

            journey_time[i] = "0h " + journey_time[i]           # Adds 0 hour

​

journey_time_hours = []

journey_time_mins = []

for i in range(len(journey_time)):

    journey_time_hours.append(int(journey_time[i].split(sep = "h")[0]))    # Extract hours from duration

    journey_time_mins.append(int(journey_time[i].split(sep = "m")[0].split()[-1]))   # Extracts only minutes from duration

# Adding journey_time_hours and journey_time_mins list to df dataframe

​

df["Journey_time_hours"] = journey_time_hours

df["Journey_time_mins"] = journey_time_mins

df.drop(["Duration"], axis = 1, inplace = True)

df.head(30)

	Airline 	Source 	Destination 	Route 	Total_Stops 	Additional_Info 	Price 	Journey_day 	Journey_month 	Dep_hour 	Dep_min 	Arrival_hour 	Arrival_min 	Journey_time_hours 	Journey_time_mins
0 	IndiGo 	Banglore 	New Delhi 	BLR → DEL 	non-stop 	No info 	3897 	24 	3 	22 	20 	1 	10 	2 	50
1 	Air India 	Kolkata 	Banglore 	CCU → IXR → BBI → BLR 	2 stops 	No info 	7662 	1 	5 	5 	50 	13 	15 	7 	25
2 	Jet Airways 	Delhi 	Cochin 	DEL → LKO → BOM → COK 	2 stops 	No info 	13882 	9 	6 	9 	25 	4 	25 	19 	0
3 	IndiGo 	Kolkata 	Banglore 	CCU → NAG → BLR 	1 stop 	No info 	6218 	12 	5 	18 	5 	23 	30 	5 	25
4 	IndiGo 	Banglore 	New Delhi 	BLR → NAG → DEL 	1 stop 	No info 	13302 	1 	3 	16 	50 	21 	35 	4 	45
5 	SpiceJet 	Kolkata 	Banglore 	CCU → BLR 	non-stop 	No info 	3873 	24 	6 	9 	0 	11 	25 	2 	25
6 	Jet Airways 	Banglore 	New Delhi 	BLR → BOM → DEL 	1 stop 	In-flight meal not included 	11087 	12 	3 	18 	55 	10 	25 	15 	30
7 	Jet Airways 	Banglore 	New Delhi 	BLR → BOM → DEL 	1 stop 	No info 	22270 	1 	3 	8 	0 	5 	5 	21 	5
8 	Jet Airways 	Banglore 	New Delhi 	BLR → BOM → DEL 	1 stop 	In-flight meal not included 	11087 	12 	3 	8 	55 	10 	25 	25 	30
9 	Multiple carriers 	Delhi 	Cochin 	DEL → BOM → COK 	1 stop 	No info 	8625 	27 	5 	11 	25 	19 	15 	7 	50
10 	Air India 	Delhi 	Cochin 	DEL → BLR → COK 	1 stop 	No info 	8907 	1 	6 	9 	45 	23 	0 	13 	15
11 	IndiGo 	Kolkata 	Banglore 	CCU → BLR 	non-stop 	No info 	4174 	18 	4 	20 	20 	22 	55 	2 	35
12 	Air India 	Chennai 	Kolkata 	MAA → CCU 	non-stop 	No info 	4667 	24 	6 	11 	40 	13 	55 	2 	15
13 	Jet Airways 	Kolkata 	Banglore 	CCU → BOM → BLR 	1 stop 	In-flight meal not included 	9663 	9 	5 	21 	10 	9 	20 	12 	10
14 	IndiGo 	Kolkata 	Banglore 	CCU → BLR 	non-stop 	No info 	4804 	24 	4 	17 	15 	19 	50 	2 	35
15 	Air India 	Delhi 	Cochin 	DEL → AMD → BOM → COK 	2 stops 	No info 	14011 	3 	3 	16 	40 	19 	15 	26 	35
16 	SpiceJet 	Delhi 	Cochin 	DEL → PNQ → COK 	1 stop 	No info 	5830 	15 	4 	8 	45 	13 	15 	4 	30
17 	Jet Airways 	Delhi 	Cochin 	DEL → BOM → COK 	1 stop 	In-flight meal not included 	10262 	12 	6 	14 	0 	12 	35 	22 	35
18 	Air India 	Delhi 	Cochin 	DEL → CCU → BOM → COK 	2 stops 	No info 	13381 	12 	6 	20 	15 	19 	15 	23 	0
19 	Jet Airways 	Delhi 	Cochin 	DEL → BOM → COK 	1 stop 	In-flight meal not included 	12898 	27 	5 	16 	0 	12 	35 	20 	35
20 	GoAir 	Delhi 	Cochin 	DEL → BOM → COK 	1 stop 	No info 	19495 	6 	3 	14 	10 	19 	20 	5 	10
21 	Air India 	Banglore 	New Delhi 	BLR → COK → DEL 	1 stop 	No info 	6955 	21 	3 	22 	0 	13 	20 	15 	20
22 	IndiGo 	Banglore 	Delhi 	BLR → DEL 	non-stop 	No info 	3943 	3 	4 	4 	0 	6 	50 	2 	50
23 	IndiGo 	Banglore 	Delhi 	BLR → DEL 	non-stop 	No info 	4823 	1 	5 	18 	55 	21 	50 	2 	55
24 	Jet Airways 	Kolkata 	Banglore 	CCU → BOM → BLR 	1 stop 	In-flight meal not included 	7757 	6 	5 	18 	55 	8 	15 	13 	20
25 	Jet Airways 	Delhi 	Cochin 	DEL → IDR → BOM → COK 	2 stops 	No info 	13292 	9 	6 	21 	25 	12 	35 	15 	10
26 	IndiGo 	Delhi 	Cochin 	DEL → LKO → COK 	1 stop 	No info 	8238 	1 	6 	21 	50 	3 	35 	5 	45
27 	GoAir 	Delhi 	Cochin 	DEL → BOM → COK 	1 stop 	No info 	7682 	15 	5 	7 	0 	12 	55 	5 	55
28 	Vistara 	Banglore 	Delhi 	BLR → DEL 	non-stop 	No info 	4668 	18 	6 	9 	45 	12 	35 	2 	50
29 	Vistara 	Chennai 	Kolkata 	MAA → CCU 	non-stop 	No info 	3687 	15 	6 	7 	5 	9 	20 	2 	15
Handling Categorical Data

Here to handle categorical data we use OneHotEncoding because the variables do not have any relation with each other

df["Airline"].value_counts()

Jet Airways                          3849
IndiGo                               2053
Air India                            1751
Multiple carriers                    1196
SpiceJet                              818
Vistara                               479
Air Asia                              319
GoAir                                 194
Multiple carriers Premium economy      13
Jet Airways Business                    6
Vistara Premium economy                 3
Trujet                                  1
Name: Airline, dtype: int64

# From graph we can see that Jet Airways Business have the highest Price.

# Apart from the first Airline almost all are having similar median

​

# Airline vs Price

sns.catplot(y = "Price", x = "Airline", data = df.sort_values("Price", ascending = False), kind="boxen", height = 7, aspect = 3)

plt.show()

# Airline is Nominal Categorical data, OneHotEncoding is performed

​

Airline = df[["Airline"]]

​

Airline = pd.get_dummies(Airline, drop_first= True)

​

Airline.head()

	Airline_Air India 	Airline_GoAir 	Airline_IndiGo 	Airline_Jet Airways 	Airline_Jet Airways Business 	Airline_Multiple carriers 	Airline_Multiple carriers Premium economy 	Airline_SpiceJet 	Airline_Trujet 	Airline_Vistara 	Airline_Vistara Premium economy
0 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0
1 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0
2 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0
3 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0
4 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0

df["Source"].value_counts()

Delhi       4536
Kolkata     2871
Banglore    2197
Mumbai       697
Chennai      381
Name: Source, dtype: int64

# Source vs Price

​

sns.catplot(y = "Price", x = "Source", data = df.sort_values("Price", ascending = False), kind="boxen", height = 4, aspect = 3)

plt.show()

# As Source is Nominal Categorical data we will perform OneHotEncoding

​

Source = df[["Source"]]

​

Source = pd.get_dummies(Source, drop_first= True)

​

Source.head()

	Source_Chennai 	Source_Delhi 	Source_Kolkata 	Source_Mumbai
0 	0 	0 	0 	0
1 	0 	0 	1 	0
2 	0 	1 	0 	0
3 	0 	0 	1 	0
4 	0 	0 	0 	0

df["Destination"].value_counts()

Cochin       4536
Banglore     2871
Delhi        1265
New Delhi     932
Hyderabad     697
Kolkata       381
Name: Destination, dtype: int64

# As Destination is Nominal Categorical data we will perform OneHotEncoding

​

Destination = df[["Destination"]]

​

Destination = pd.get_dummies(Destination, drop_first = True)

​

Destination.head()

	Destination_Cochin 	Destination_Delhi 	Destination_Hyderabad 	Destination_Kolkata 	Destination_New Delhi
0 	0 	0 	0 	0 	1
1 	0 	0 	0 	0 	0
2 	1 	0 	0 	0 	0
3 	0 	0 	0 	0 	0
4 	0 	0 	0 	0 	1

df["Route"]

0                    BLR → DEL
1        CCU → IXR → BBI → BLR
2        DEL → LKO → BOM → COK
3              CCU → NAG → BLR
4              BLR → NAG → DEL
                 ...          
10678                CCU → BLR
10679                CCU → BLR
10680                BLR → DEL
10681                BLR → DEL
10682    DEL → GOI → BOM → COK
Name: Route, Length: 10682, dtype: object

# Additional_Info contains almost 80% no_info

# Route and Total_Stops are related to each other

​

df.drop(["Route", "Additional_Info"], axis = 1, inplace = True)

df["Total_Stops"].value_counts()

1 stop      5625
non-stop    3491
2 stops     1520
3 stops       45
4 stops        1
Name: Total_Stops, dtype: int64

# As this is case of Ordinal Categorical type we perform LabelEncoder

# Here Values are assigned with corresponding keys

​

df.replace({"non-stop": 0, "1 stop": 1, "2 stops": 2, "3 stops": 3, "4 stops": 4}, inplace = True)

df.head()

	Airline 	Source 	Destination 	Total_Stops 	Price 	Journey_day 	Journey_month 	Dep_hour 	Dep_min 	Arrival_hour 	Arrival_min 	Journey_time_hours 	Journey_time_mins
0 	IndiGo 	Banglore 	New Delhi 	0 	3897 	24 	3 	22 	20 	1 	10 	2 	50
1 	Air India 	Kolkata 	Banglore 	2 	7662 	1 	5 	5 	50 	13 	15 	7 	25
2 	Jet Airways 	Delhi 	Cochin 	2 	13882 	9 	6 	9 	25 	4 	25 	19 	0
3 	IndiGo 	Kolkata 	Banglore 	1 	6218 	12 	5 	18 	5 	23 	30 	5 	25
4 	IndiGo 	Banglore 	New Delhi 	1 	13302 	1 	3 	16 	50 	21 	35 	4 	45

# Concatenate dataframe --> df + Airline + Source + Destination

​

df_train = pd.concat([df, Airline, Source, Destination], axis = 1)

df_train.head()

	Airline 	Source 	Destination 	Total_Stops 	Price 	Journey_day 	Journey_month 	Dep_hour 	Dep_min 	Arrival_hour 	Arrival_min 	Journey_time_hours 	Journey_time_mins 	Airline_Air India 	Airline_GoAir 	Airline_IndiGo 	Airline_Jet Airways 	Airline_Jet Airways Business 	Airline_Multiple carriers 	Airline_Multiple carriers Premium economy 	Airline_SpiceJet 	Airline_Trujet 	Airline_Vistara 	Airline_Vistara Premium economy 	Source_Chennai 	Source_Delhi 	Source_Kolkata 	Source_Mumbai 	Destination_Cochin 	Destination_Delhi 	Destination_Hyderabad 	Destination_Kolkata 	Destination_New Delhi
0 	IndiGo 	Banglore 	New Delhi 	0 	3897 	24 	3 	22 	20 	1 	10 	2 	50 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1
1 	Air India 	Kolkata 	Banglore 	2 	7662 	1 	5 	5 	50 	13 	15 	7 	25 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0
2 	Jet Airways 	Delhi 	Cochin 	2 	13882 	9 	6 	9 	25 	4 	25 	19 	0 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	1 	0 	0 	0 	0
3 	IndiGo 	Kolkata 	Banglore 	1 	6218 	12 	5 	18 	5 	23 	30 	5 	25 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0
4 	IndiGo 	Banglore 	New Delhi 	1 	13302 	1 	3 	16 	50 	21 	35 	4 	45 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1

df_train.drop(["Airline", "Source", "Destination"], axis = 1, inplace = True)

df_train.head()

	Total_Stops 	Price 	Journey_day 	Journey_month 	Dep_hour 	Dep_min 	Arrival_hour 	Arrival_min 	Journey_time_hours 	Journey_time_mins 	Airline_Air India 	Airline_GoAir 	Airline_IndiGo 	Airline_Jet Airways 	Airline_Jet Airways Business 	Airline_Multiple carriers 	Airline_Multiple carriers Premium economy 	Airline_SpiceJet 	Airline_Trujet 	Airline_Vistara 	Airline_Vistara Premium economy 	Source_Chennai 	Source_Delhi 	Source_Kolkata 	Source_Mumbai 	Destination_Cochin 	Destination_Delhi 	Destination_Hyderabad 	Destination_Kolkata 	Destination_New Delhi
0 	0 	3897 	24 	3 	22 	20 	1 	10 	2 	50 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1
1 	2 	7662 	1 	5 	5 	50 	13 	15 	7 	25 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0
2 	2 	13882 	9 	6 	9 	25 	4 	25 	19 	0 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	1 	0 	0 	0 	0
3 	1 	6218 	12 	5 	18 	5 	23 	30 	5 	25 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0
4 	1 	13302 	1 	3 	16 	50 	21 	35 	4 	45 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1

df_train.shape

(10682, 30)

Feature Selection

Finding out the best feature which will contribute and have good relation with target variable. Following are some of the feature selection methods,

df_train.shape

(10682, 30)

df_train.columns

Index(['Total_Stops', 'Price', 'Journey_day', 'Journey_month', 'Dep_hour',
       'Dep_min', 'Arrival_hour', 'Arrival_min', 'Journey_time_hours',
       'Journey_time_mins', 'Airline_Air India', 'Airline_GoAir',
       'Airline_IndiGo', 'Airline_Jet Airways', 'Airline_Jet Airways Business',
       'Airline_Multiple carriers',
       'Airline_Multiple carriers Premium economy', 'Airline_SpiceJet',
       'Airline_Trujet', 'Airline_Vistara', 'Airline_Vistara Premium economy',
       'Source_Chennai', 'Source_Delhi', 'Source_Kolkata', 'Source_Mumbai',
       'Destination_Cochin', 'Destination_Delhi', 'Destination_Hyderabad',
       'Destination_Kolkata', 'Destination_New Delhi'],
      dtype='object')

X = df_train.loc[:, ['Total_Stops', 'Journey_day', 'Journey_month', 'Dep_hour',

       'Dep_min', 'Arrival_hour', 'Arrival_min', 'Journey_time_hours',

       'Journey_time_mins', 'Airline_Air India', 'Airline_GoAir', 'Airline_IndiGo',

       'Airline_Jet Airways', 'Airline_Jet Airways Business',

       'Airline_Multiple carriers',

       'Airline_Multiple carriers Premium economy', 'Airline_SpiceJet',

       'Airline_Vistara', 'Airline_Vistara Premium economy',

       'Source_Chennai', 'Source_Delhi', 'Source_Kolkata', 'Source_Mumbai',

       'Destination_Cochin', 'Destination_Delhi', 'Destination_Hyderabad',

       'Destination_Kolkata', 'Destination_New Delhi']]

X.head()

	Total_Stops 	Journey_day 	Journey_month 	Dep_hour 	Dep_min 	Arrival_hour 	Arrival_min 	Journey_time_hours 	Journey_time_mins 	Airline_Air India 	Airline_GoAir 	Airline_IndiGo 	Airline_Jet Airways 	Airline_Jet Airways Business 	Airline_Multiple carriers 	Airline_Multiple carriers Premium economy 	Airline_SpiceJet 	Airline_Vistara 	Airline_Vistara Premium economy 	Source_Chennai 	Source_Delhi 	Source_Kolkata 	Source_Mumbai 	Destination_Cochin 	Destination_Delhi 	Destination_Hyderabad 	Destination_Kolkata 	Destination_New Delhi
0 	0 	24 	3 	22 	20 	1 	10 	2 	50 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1
1 	2 	1 	5 	5 	50 	13 	15 	7 	25 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0
2 	2 	9 	6 	9 	25 	4 	25 	19 	0 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	1 	0 	0 	0 	0
3 	1 	12 	5 	18 	5 	23 	30 	5 	25 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0
4 	1 	1 	3 	16 	50 	21 	35 	4 	45 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1

y = df_train.loc[:, ['Price']]

y.head()

	Price
0 	3897
1 	7662
2 	13882
3 	6218
4 	13302

# Finds correlation between Independent and dependent attributes

​

plt.figure(figsize = (10,10))

sns.heatmap(df.corr(), annot = True,cmap="YlGnBu" )

​

plt.show()

# Important feature using ExtraTreesRegressor

​

from sklearn.ensemble import ExtraTreesRegressor

selection = ExtraTreesRegressor()

selection.fit(X, y)

C:\Users\Soft\AppData\Local\Temp\ipykernel_3340\2896054759.py:5: DataConversionWarning: A column-vector y was passed when a 1d array was expected. Please change the shape of y to (n_samples,), for example using ravel().
  selection.fit(X, y)

ExtraTreesRegressor()

print(selection.feature_importances_)

[2.52833648e-01 1.43311871e-01 5.39996212e-02 2.41778854e-02
 2.11434776e-02 2.76861114e-02 1.96224519e-02 8.43756596e-02
 1.81158239e-02 9.41816415e-03 1.72274989e-03 1.71060482e-02
 1.44854449e-01 6.77343839e-02 1.95148892e-02 8.75377563e-04
 2.80651162e-03 5.18744872e-03 8.68864081e-05 5.70397729e-04
 1.41153737e-02 3.03919132e-03 6.77281843e-03 1.04637006e-02
 1.78327141e-02 7.46296932e-03 4.15106547e-04 2.47542696e-02]

#plot graph of feature importances for better visualization

​

plt.figure(figsize = (12,8))

feature_imp = pd.Series(selection.feature_importances_, index=X.columns)

feature_imp.nlargest(20).plot(kind='barh')

plt.show()

Fitting model using Random Forest
Why Random forest?

Random Forest uses Decision trees to process the and train the data. A number of decision trees are generated and the agregate of those decisions are chosen to finalize the random forest method results.

The reason why we do not use the decision tree classification is that it has its disadvantages such as overfitting and bias. To overcome that issues the Rando forest method is used which reduces the overfitting and bias

from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.17, random_state = 16)

from sklearn.ensemble import RandomForestRegressor

reg_rf = RandomForestRegressor()

reg_rf.fit(X_train, y_train)

C:\Users\Soft\AppData\Local\Temp\ipykernel_3340\2751168992.py:3: DataConversionWarning: A column-vector y was passed when a 1d array was expected. Please change the shape of y to (n_samples,), for example using ravel().
  reg_rf.fit(X_train, y_train)

RandomForestRegressor()

y_pred = reg_rf.predict(X_test)

X_test

	Total_Stops 	Journey_day 	Journey_month 	Dep_hour 	Dep_min 	Arrival_hour 	Arrival_min 	Journey_time_hours 	Journey_time_mins 	Airline_Air India 	Airline_GoAir 	Airline_IndiGo 	Airline_Jet Airways 	Airline_Jet Airways Business 	Airline_Multiple carriers 	Airline_Multiple carriers Premium economy 	Airline_SpiceJet 	Airline_Vistara 	Airline_Vistara Premium economy 	Source_Chennai 	Source_Delhi 	Source_Kolkata 	Source_Mumbai 	Destination_Cochin 	Destination_Delhi 	Destination_Hyderabad 	Destination_Kolkata 	Destination_New Delhi
686 	0 	15 	3 	5 	15 	7 	35 	2 	20 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0 	1 	0
1479 	2 	15 	6 	9 	40 	19 	0 	9 	20 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	1 	0 	0 	0 	0
8849 	0 	9 	3 	10 	30 	13 	20 	2 	50 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1
2654 	2 	6 	3 	5 	30 	19 	45 	38 	15 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	1 	0 	0 	0 	0
5625 	0 	6 	6 	8 	20 	11 	20 	3 	0 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	0
... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	... 	...
3189 	1 	21 	3 	5 	45 	21 	20 	15 	35 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1
6818 	1 	3 	3 	16 	10 	22 	20 	6 	10 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	1 	0 	0 	0 	0
9325 	0 	27 	5 	10 	20 	11 	50 	1 	30 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1 	0 	0 	1 	0 	0
8536 	1 	1 	3 	14 	5 	9 	30 	19 	25 	0 	0 	0 	1 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	0 	1
9592 	1 	9 	3 	7 	30 	19 	45 	12 	15 	0 	0 	0 	0 	0 	1 	0 	0 	0 	0 	0 	1 	0 	0 	1 	0 	0 	0 	0

1816 rows × 28 columns

y_pred

array([ 6142.31      , 12495.22516667,  6631.63333333, ...,
        6538.625     , 21640.        , 14988.051     ])

reg_rf.score(X_train, y_train)

0.95189327595574

reg_rf.score(X_test, y_test)

0.8319631670370436

y_pred = y_pred.reshape(1816,1)

print(y_pred)

[[ 6142.31      ]
 [12495.22516667]
 [ 6631.63333333]
 ...
 [ 6538.625     ]
 [21640.        ]
 [14988.051     ]]

sns.displot(y_test-y_pred)

plt.show()

​

plt.scatter(y_test, y_pred, alpha = 0.5)

plt.xlabel("y_test")

plt.ylabel("y_pred")

plt.show()

from sklearn import metrics

print('MAE:', metrics.mean_absolute_error(y_test, y_pred))

print('MSE:', metrics.mean_squared_error(y_test, y_pred))

print('RMSE:', np.sqrt(metrics.mean_squared_error(y_test, y_pred)))

MAE: 1151.33447439846
MSE: 3693762.711920007
RMSE: 1921.916416476015

metrics.r2_score(y_test, y_pred)

0.8319631670370436

