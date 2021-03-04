# Netflix Data Analysis {Python}
This dataset consists of TV Shows and Movies listed on Netflix, [sourced from Kaggle](https://www.kaggle.com/shivamb/netflix-shows)

Packages used:
```
import numpy as np # linear algebra
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
from time import strptime # date formatting
import matplotlib.pyplot as plt # plotting output
import matplotlib.ticker as mtick # plot formatting
from scipy.stats import norm # stats package for data aggregations
import seaborn as sns # plotting output
```

### Analysis 1: United States shows by type and month added
###### This demonstrates the quantity of shows in the United States available on Netflix over time
###### The steps taken for to construct this plot are as follows:
* Parse the data by date_added
* Combine ratings into viewer maturity category (youth, teen or adult)
* Group and pivot data to create proper series
* Construct plots

### Analysis 2: Show type by maturity and year added 
###### This demonstrates the % of shows that are movies each year, by maturity group (adult, teen, youth) for a specified country
###### The steps taken for to construct this plot are as follows:
~~* Parse the data by date_added~~
~~* Combine ratings into viewer maturity category (youth, teen or adult)~~
~~* Group and pivot data to create proper series~~
~~* Construct plots~~

## Analysis 1: United States shows by type and month added
#### Step 1: Parse the data by date_added
In this series of code block we...
1. Read the data into a dataframe
2. Parse the date strings into month and year columns
3. Drop all rows with null values in the analyzed columns
4. Convert month and year to a datetime value
5. Filter out early years of netflix streaming to ensure plentiful data in plotting
6. Drop the old index from the original dataframe to not be confused in new clean and structured dataframe (`country_titles`)
```
df = pd.read_csv('netflix_titles.csv')
df['month_added'] = df['date_added'].str.split().str[0]
df['year_added'] = df['date_added'].str.split().str[2]
df_clean = df.dropna(subset=['date_added','country', 'month_added', 'year_added'])
df_clean['mmyy_added'] = pd.to_datetime(df_clean.year_added.astype(str) + '/' + df_clean.month_added.astype(str) + '/01')
df_clean = df_clean[(df_clean['mmyy_added'] > '2014-12-31')].reset_index()
country_titles = df_clean[df_clean['country'].str.contains('United States', na = False)].reset_index()
country_titles = country_titles.drop('level_0', 1) # Drop intermediate index column from date transform
# print(country_titles.head())
```
#### 
#### 
#### Step 2: Combine ratings into viewer maturity category (youth, teen or adult)
In this series of code block we...
1. Re-classify each `rating` into a `rating_class` based on maturity level
2. Drop all rows with null values in the new `rating_class` column
3. Separate the dataframe into one dataframe for each `type` of show (`country_movies` and `country_tv`)
4. Drop the old index from the original dataframe to not be confused in new clean and structured dataframes
```
country_titles.loc[(country_titles['rating'] == 'R') | 
                   (country_titles['rating'] == 'TV-MA') |
                   (country_titles['rating'] == 'NC-17')
                       , 'rating_class'] = 'Adult'
country_titles.loc[(country_titles['rating'] == 'TV-Y') | 
                   (country_titles['rating'] == 'TV-Y7') | 
                   (country_titles['rating'] == 'G') | 
                   (country_titles['rating'] == 'TV-G') |
                   (country_titles['rating'] == 'PG') | 
                   (country_titles['rating'] == 'TV-PG')
                       , 'rating_class'] = 'Youth'
country_titles.loc[(country_titles['rating'] == 'PG-13') | 
                   (country_titles['rating'] == 'TV-14')
                       , 'rating_class'] = 'Teen'
country_titles.dropna(subset=['rating_class'], inplace=True) 
country_movies = country_titles[country_titles['type'] == 'Movie'].reset_index()
country_tv = country_titles[country_titles['type'] == 'TV Show'].reset_index()
country_movies = country_movies.drop('level_0', 1)
country_tv = country_tv.drop('level_0', 1)
```
#### 
#### 
#### Step 3: Group and pivot data to create proper series
#### 
#### 
#### Step 4: Construct plots
