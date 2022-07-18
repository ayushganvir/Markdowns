# Pandas
### [Cheat Sheet](https://pandas.pydata.org/Pandas_Cheat_Sheet.pdf) here.
## Series

> Each column of DataFrame is a Series

```python
df = pd.DataFrame(
    {
        "Name": [
            "Braund, Mr. Owen Harris",
            "Allen, Mr. William Henry",
            "Bonnell, Miss. Elizabeth",
        ],
        "Age": [22, 35, 58],
        "Sex": ["male", "male", "female"],
    }
)

df["Age"]
0    22
1    35
2    58
Name: Age, dtype: int64


df["Age"].max()
100
```
## DataFrame

A DataFrame is a 2-dimensional data structure that can store data of different types (including characters, integers, floating point values, categorical data and more) in columns. It is similar to a spreadsheet, a SQL table or the data.frame in R.
> Each column of a DataFrame is a Series


```python
import pandas as pd
titanic = pd.read_csv("data/titanic.csv")
```
> pandas provi the read_csv() function to read data stored as a csv file into a pandas DataFrame. pandas supports many different file formats or data sources out of the box (csv, excel, sql, json, parquet, …), each of them with the prefix read_*.



## Check on the data after reading in the data. 
When displaying a DataFrame, the first and last 5 rows will be shown by default.

```python
titanic
```
## See the first 8 rows of a pandas DataFrame.

```python
titanic.head(8)
```

## Column data types can be done by requesting the pandas dtypes attribute:

```python
titanic.dtypes
```

## Titanic data as a spreadsheet.

```python
titanic.to_excel("titanic.xlsx", sheet_name="passengers", index=False)
```
> read_* functions are used to read data to pandas, the to_* methods are used to store data.

## Technical summary of a DataFrame

```python
titanic.info()
```
## describe()
 describe() method provides a quick overview of the numerical data in a DataFrame.

 ```python
 df.describe()
              Age
count   3.000000
mean   38.333333
std    18.230012
min    22.000000
25%    28.500000
50%    35.000000
75%    46.500000
max    58.000000
 ```

 ## Subset of a DataFrame

```python
age_sex = titanic[["Age", "Sex"]]

age_sex.head()
Out[9]: 
    Age     Sex
0  22.0    male
1  38.0  female
2  26.0  female
3  35.0  female
4  35.0    male
```

## shape()

```python
titanic[["Age", "Sex"]].shape
Out[11]: (891, 2)
```

## Specific rows of DataFrame

```python
above_35 = titanic[titanic["Age"] > 35]

above_35.head()

class_23 = titanic[titanic["Pclass"].isin([2, 3])]

```
> the isin() conditional function returns a True for each row the values are in the provided list. To filter the rows based on such a function, use the conditional function inside the selection brackets [].

I’m interested in rows 10 till 25 and columns 3 to 5.
```python
titanic.iloc[9:25, 2:5]
```

## Changing values

```python
titanic.iloc[0:3, 3] = "anonymous"
```

## Plots in Pandas
```python
import pandas as pd

import matplotlib.pyplot as plot

air_quality = pd.read_csv("data/air_quality_no2.csv", index_col=0, parse_dates=True)

air_quality.head()


             station_antwerp  station_paris  station_london
datetime                                                           
2019-05-07 02:00:00              NaN            NaN            23.0
2019-05-07 03:00:00             50.5           25.0            19.0
2019-05-07 04:00:00             45.0           27.7            19.0
2019-05-07 05:00:00              NaN           50.4            16.0
2019-05-07 06:00:00              NaN           61.9             NaN

air_quality.plot()
# all columns plotted

air_quality["station_paris"].plot()

# only station_paris plotted
```

## Create new columns derived from existing column

```python
air_quality["london_mg_per_cubic"] = air_quality["station_london"] * 1.882
```
## Aggregating Statistics
```python
>>>titanic["Age"].mean()

>>>titanic[["Age", "Fare"]].median()

>>>titanic[["Age", "Fare"]].describe()

>>>titanic.agg(
    {
        "Age": ["min", "max", "median", "skew"],
        "Fare": ["min", "max", "median", "mean"],
    }
)

>>>titanic[["Sex", "Age"]].groupby("Sex").mean()

>>>titanic["Pclass"].value_counts()

>>>titanic.groupby("Pclass")["Pclass"].count()
```
