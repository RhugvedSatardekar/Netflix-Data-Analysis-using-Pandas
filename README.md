

## Importing Necessary Libraries and Reading the CSV File

```python
import pandas as pd

netflix = pd.read_csv('file.csv', parse_dates=['Release_Date'])
```
- Import the pandas library.
- Read the CSV file and parse the 'Release_Date' column as datetime.

## Initial Data Exploration

```python
netflix
netflix.info(memory_usage='deep')
netflix.describe()
netflix.head(10)
netflix.tail(5)
netflix.shape
netflix.columns
netflix.size  # number of cells
netflix.dtypes
```
- Display the DataFrame.
- Get a concise summary of the DataFrame.
- Generate descriptive statistics.
- Display the first 10 rows.
- Display the last 5 rows.
- Get the shape of the DataFrame.
- Get the column names.
- Get the total number of cells.
- Get the data types of each column.

## Removing Duplicate Records

```python
netflix[netflix.duplicated()]
netflix.drop_duplicates(inplace=True)
```
- Identify duplicate records.
- Remove duplicate records in place.

## Handling Missing Values

```python
netflix.isna().sum()

import seaborn as sns
import numpy as np
import matplotlib as mpl

!pip uninstall numpy seaborn -y
!pip install numpy seaborn

import numpy as np

np.__version__
```
- Check for missing values.
- Import necessary libraries for visualization.
- Uninstall and reinstall numpy and seaborn.
- Check the numpy version.

## Specific Data Analysis

### For House of Cards, What is the Show ID and Who is the Director?

```python
netflix[['Show_Id', 'Director']][netflix.Title == 'House of Cards']
```
- Filter the DataFrame for the title 'House of Cards' and select the 'Show_Id' and 'Director' columns.

### In Which Year Were the Highest Number of TV Shows & Movies Released?

```python
netflix.tail()
netflix.drop('Year', axis=1, inplace=True)
netflix['Date_New'] = pd.to_datetime(netflix['Release_Date'], format='mixed')
netflix.dtypes
netflix.Date_New.dt.year.astype
netflix['Year'] = netflix.Date_New.dt.year
netflix['Year'] = netflix.Year.astype(str)
netflix['Year'] = netflix.Year.str.replace('.0', '')
netflix.drop('Release_Date', axis=1, inplace=True)
netflix
df = pd.DataFrame(netflix.pivot_table(index='Year', columns='Category', values='Show_Id', aggfunc='count'))
netflix[['Year', 'Category', 'Show_Id']].groupby(['Year', 'Category']).count().sort_values('Show_Id', ascending=False).plot(kind='bar')
netflix.Year.value_counts()
```
- Remove the 'Year' column and create a new 'Date_New' column by parsing 'Release_Date'.
- Extract the year from 'Date_New' and store it in the 'Year' column.
- Create a pivot table to count the number of TV shows and movies released each year.
- Plot the count of TV shows and movies by year and category.
- Count the occurrences of each year.

### How Many Movies and TV Shows Are in the Dataset?

```python
netflix['Category'].value_counts()
```
- Count the number of movies and TV shows in the dataset.

### Show All the Movies Released in Year 2020

```python
Movies_2020 = pd.DataFrame(netflix['Title'][(netflix.Year == '2020') & (netflix.Category == 'Movie')]).reset_index(drop=True)
Movies_2020
```
- Filter the DataFrame for movies released in 2020 and create a new DataFrame.

### Show the Titles of TV Shows Released in India Only

```python
Indian_TV_Shows = pd.DataFrame(netflix['Title'][(netflix.Country == 'India') & (netflix.Category == 'TV Show')]).reset_index(drop=True)
Indian_TV_Shows.Title.describe()
```
- Filter the DataFrame for TV shows released in India and create a new DataFrame.
- Describe the 'Title' column.

### Show the Top 10 Directors Who Gave the Highest Number of TV Shows and Movies to Netflix

```python
netflix.Director.value_counts().head(10)
```
- Get the top 10 directors with the highest number of TV shows and movies on Netflix.

### Show All the Records Where Category is Movie and Type is Comedies or Country is UK

```python
df = pd.DataFrame(netflix[((netflix.Category == 'Movie') & (netflix.Type.str.contains('[Cc]omed[yies]', regex=True, na=False)))])
netflix.Type.str.contains('[comedy]', regex=True)
df.Type.unique()
```
- Filter the DataFrame for movies that are comedies or movies from the UK.

### In How Many Movies or Shows Was Tom Cruise Cast?

```python
netflix[netflix.Cast.str.contains('[Tt]om [Cc]ruise', regex=True, na=False)]
```
- Filter the DataFrame for movies or shows featuring Tom Cruise.

### What Are the Different Ratings Defined by Netflix?

```python
df = pd.DataFrame(netflix.Rating.unique(), columns=['Ratings']).sort_values('Ratings').reset_index(drop=True)
df.nunique()
df
```
- Get the unique ratings defined by Netflix and sort them.

### How Many Movies Got TV-14 Rating in Canada?

```python
netflix['Show_Id'][(netflix.Rating == 'TV-14') & (netflix.Category == 'Movie') & (netflix.Country == 'Canada')].nunique()
```
- Count the number of movies with a TV-14 rating in Canada.

### How Many TV Shows R Rating After 2018?

```python
netflix[(netflix.Category == 'TV Show') & (netflix.Rating == 'R') & (netflix.Year > '2018')]
netflix['Year'] = netflix.Year.astype('int', errors='ignore')
```
- Filter the DataFrame for TV shows with an R rating released after 2018.

### What is the Maximum Duration of a Movie/Show on Netflix?

```python
netflix.head(2)
netflix.loc[netflix.Duration.str.contains('Season(s)?', regex=True) == True, 'Season/Time'] = 'Total Seasons'
netflix.loc[netflix.Duration.str.contains('min(s)?', regex=True) == True, 'Season/Time'] = 'Movie Duration'
netflix['Period'] = netflix.Duration.str.extract(r'(\d+)').astype('int')
netflix.groupby('Season/Time').Period.max()
netflix.dtypes
netflix[['Seasons/Min', 'Unit']] = netflix.Duration.str.split(' ', expand=True)
netflix
```
- Extract and categorize the duration of movies and TV shows.
- Split the 'Duration' column into 'Seasons/Min' and 'Unit' and find the maximum period.

### Which Individual Country Has the Highest Number of TV Shows?

```python
netflix[netflix.Category == 'TV Show'].Country.value_counts().head(1)
```
- Find the country with the highest number of TV shows.

### Sort the Dataset by Year

```python
netflix.sort_values('Year', ascending=True)
```
- Sort the DataFrame by the 'Year' column in ascending order.
