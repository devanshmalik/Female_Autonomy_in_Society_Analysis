
# Investigating Effect of Female Literacy and Autonomy in Society on Child Mortality Rates

## Table of Contents
<ul>
<li><a href="#intro">Introduction</a></li>
<li><a href="#questions">Research Questions</a></li>
<li><a href="#wrangling">Data Wrangling</a></li>
<li><a href="#eda">Exploratory Data Analysis</a></li>
<li><a href="#conclusions">Conclusions</a></li>
</ul>

## Introduction
The goal of this analysis was to investigate indicators related to lives of females in different regions of the world.  The factors explored are related to female literacy, autonomy and engagement in society and how these factors are correlated to each other. In addition, we look at how female literacy and autonomy can be related to the child mortality rates in a society.

<a id='questions'></a>
### Research Questions 
We'll analyze the data to research the following questions:
1. Have certain regions of the world been growing in selected metrics better than others?
2. Is female literacy in a country related to the mean age of females at first marriage? 
3. What factors affect the female labor participation in society, specifically, to what extent does female literacy and age at marriage impact female engagement in labor force? 
4. Do factors related to female literacy and autonomy in a society have an effect on the child mortality rate in a country? 

The main independent factors explored are adult female literacy rate and mean years in school. The effect of women literacy on mean age at first marriage is explored along with the cumulative effect of all these factors on female labor participation in the economy. Finally, the effect of women's literacy and autonomy on child mortality rate in a society is assessed.

## Dataset
**Source**: The data source for this analysis is [Gapminder](https://www.gapminder.org/data/) which has collected information about how people live their lives in different countries, tracked across the years, on a number of different indicators.

From their list of 500+ indicators, the following indicators related to women literacy and engagement in society peeked my interest:
1. Literacy rate, adult female (% of females ages 15 and above)
    * Adult literacy rate is the percentage of people ages 15 and above who can, with understanding, read and write a short, simple statement on their everyday life
2. Mean years in school (women of reproductive age, 15 to 44)
    * The average number of years of school attended by all people in the age and gender group specified, including primary, secondary and tertiary education. 
3. Mean age at 1st marriage of women
    * The average age, in years, of first marriage for women. Women who never married are excluded. Cohabitation is excluded
4. Age 15-64 Female Labor to population (%)
    * For age group 15-64, percentage of female labor to total female population.
5. Under-Five Mortality rate (per 1,000 live births)
    * The probability that children born in a specific year will die before reaching the age of five, if the age-specific mortality rates remain the same.

<a id='wrangling'></a>
# Data Wrangling
Since the data source for each indicator is different, the countries and years measured vary for each dataset. In order to perform comparative analysis between indicators, substantial data wrangling is required to match countries studied for each indicator and the years measured. 
## General Properties 
General exploration of structure of each dataset to determine required steps for cleanup 


```python
import pandas as pd
import numpy as np
```

### 1. Mean School Years 


```python
df_school = pd.read_csv('data/mean_years_school_women.csv')

# Uncomment to explore data further
#df_school.info()
df_school.head()
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
      <th>Row Labels</th>
      <th>1970</th>
      <th>1971</th>
      <th>1972</th>
      <th>1973</th>
      <th>1974</th>
      <th>1975</th>
      <th>1976</th>
      <th>1977</th>
      <th>1978</th>
      <th>...</th>
      <th>2000</th>
      <th>2001</th>
      <th>2002</th>
      <th>2003</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>0.1</td>
      <td>0.1</td>
      <td>0.1</td>
      <td>0.1</td>
      <td>0.1</td>
      <td>0.1</td>
      <td>0.1</td>
      <td>0.1</td>
      <td>0.2</td>
      <td>...</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.5</td>
      <td>0.6</td>
      <td>0.6</td>
      <td>0.6</td>
      <td>0.7</td>
      <td>0.7</td>
      <td>0.7</td>
      <td>0.8</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>5.6</td>
      <td>5.7</td>
      <td>5.9</td>
      <td>6.0</td>
      <td>6.2</td>
      <td>6.3</td>
      <td>6.5</td>
      <td>6.6</td>
      <td>6.8</td>
      <td>...</td>
      <td>9.8</td>
      <td>9.9</td>
      <td>10.0</td>
      <td>10.1</td>
      <td>10.2</td>
      <td>10.3</td>
      <td>10.4</td>
      <td>10.5</td>
      <td>10.6</td>
      <td>10.7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>1.4</td>
      <td>1.5</td>
      <td>1.6</td>
      <td>1.7</td>
      <td>1.8</td>
      <td>1.9</td>
      <td>2.1</td>
      <td>2.2</td>
      <td>2.3</td>
      <td>...</td>
      <td>5.8</td>
      <td>5.9</td>
      <td>6.1</td>
      <td>6.2</td>
      <td>6.4</td>
      <td>6.5</td>
      <td>6.7</td>
      <td>6.8</td>
      <td>6.9</td>
      <td>7.1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Angola</td>
      <td>0.9</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.1</td>
      <td>1.1</td>
      <td>1.2</td>
      <td>1.2</td>
      <td>1.3</td>
      <td>1.4</td>
      <td>...</td>
      <td>3.5</td>
      <td>3.6</td>
      <td>3.7</td>
      <td>3.8</td>
      <td>3.9</td>
      <td>4.0</td>
      <td>4.1</td>
      <td>4.3</td>
      <td>4.4</td>
      <td>4.5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Antigua and Barbuda</td>
      <td>8.1</td>
      <td>8.3</td>
      <td>8.5</td>
      <td>8.7</td>
      <td>8.8</td>
      <td>9.0</td>
      <td>9.2</td>
      <td>9.4</td>
      <td>9.6</td>
      <td>...</td>
      <td>12.7</td>
      <td>12.8</td>
      <td>12.9</td>
      <td>13.0</td>
      <td>13.1</td>
      <td>13.2</td>
      <td>13.3</td>
      <td>13.3</td>
      <td>13.4</td>
      <td>13.5</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 41 columns</p>
</div>



**Observations:**
* Data present for 175 countries for years 1970 - 2009 
* All years contain 175 non-null entries with no missing data for any country for any year

**Next Steps:**
* Extract revelant years data for analysis (determined later)  

### 2. Mean Age at Marriage 


```python
df_marriage = pd.read_csv('data/marriage_age_females.csv')

# Uncomment to explore data further
#df_marriage.count()
df_marriage.head()
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
      <th>Unnamed: 0</th>
      <th>1616</th>
      <th>1666</th>
      <th>1685</th>
      <th>1710</th>
      <th>1716</th>
      <th>1735</th>
      <th>1760</th>
      <th>1766</th>
      <th>1775</th>
      <th>...</th>
      <th>1996</th>
      <th>1997</th>
      <th>1998</th>
      <th>1999</th>
      <th>2000</th>
      <th>2001</th>
      <th>2002</th>
      <th>2003</th>
      <th>2004</th>
      <th>2005</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>17.839683</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23.326509</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>27.6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>29.600000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Angola</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Argentina</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23.263962</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 117 columns</p>
</div>



**Observations:**
* Data present for 185 countries for years starting at year 1616 to 2005
* No year contains data for all 185 countries with year 2005 containing highest number of non-null entries (175)

**Next Steps:**
* Due to extreme amount of missing data for most years, all data except for year 2005 will be dropped. 
* Final dataset will contain mean age of first marriage for year 2005 for 175 countries 

### 3. Female Labor Participation


```python
df_labor = pd.read_csv('data/female_labour_participation_rate.csv')

# Uncomment to explore data further
#df_labor.info()
df_labor.head()
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
      <th>Female 15-64 labour to population (%)</th>
      <th>1980</th>
      <th>1981</th>
      <th>1982</th>
      <th>1983</th>
      <th>1984</th>
      <th>1985</th>
      <th>1986</th>
      <th>1987</th>
      <th>1988</th>
      <th>...</th>
      <th>1998</th>
      <th>1999</th>
      <th>2000</th>
      <th>2001</th>
      <th>2002</th>
      <th>2003</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>31.100000</td>
      <td>30.900000</td>
      <td>30.700001</td>
      <td>30.500000</td>
      <td>30.400000</td>
      <td>30.200001</td>
      <td>30.000000</td>
      <td>29.900000</td>
      <td>29.799999</td>
      <td>...</td>
      <td>28.000000</td>
      <td>28.000000</td>
      <td>28.000000</td>
      <td>28.200001</td>
      <td>28.400000</td>
      <td>28.600000</td>
      <td>28.900000</td>
      <td>29.200001</td>
      <td>29.600000</td>
      <td>29.500000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>73.699997</td>
      <td>73.099998</td>
      <td>72.800003</td>
      <td>72.199997</td>
      <td>71.699997</td>
      <td>71.800003</td>
      <td>71.400002</td>
      <td>72.199997</td>
      <td>73.000000</td>
      <td>...</td>
      <td>55.400002</td>
      <td>55.599998</td>
      <td>55.700001</td>
      <td>55.700001</td>
      <td>55.700001</td>
      <td>55.700001</td>
      <td>55.700001</td>
      <td>55.599998</td>
      <td>55.599998</td>
      <td>56.099998</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>19.799999</td>
      <td>19.900000</td>
      <td>20.100000</td>
      <td>20.100000</td>
      <td>20.299999</td>
      <td>20.400000</td>
      <td>21.000000</td>
      <td>21.700001</td>
      <td>22.400000</td>
      <td>...</td>
      <td>30.400000</td>
      <td>31.299999</td>
      <td>32.099998</td>
      <td>33.000000</td>
      <td>33.799999</td>
      <td>34.700001</td>
      <td>35.500000</td>
      <td>36.200001</td>
      <td>36.900002</td>
      <td>38.099998</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Angola</td>
      <td>74.300003</td>
      <td>74.300003</td>
      <td>74.500000</td>
      <td>74.599998</td>
      <td>74.800003</td>
      <td>74.900002</td>
      <td>75.000000</td>
      <td>75.199997</td>
      <td>76.000000</td>
      <td>...</td>
      <td>74.699997</td>
      <td>75.099998</td>
      <td>74.699997</td>
      <td>74.599998</td>
      <td>75.000000</td>
      <td>74.900002</td>
      <td>75.199997</td>
      <td>75.500000</td>
      <td>76.199997</td>
      <td>76.400002</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Argentina</td>
      <td>32.799999</td>
      <td>32.099998</td>
      <td>31.600000</td>
      <td>31.299999</td>
      <td>31.600000</td>
      <td>32.099998</td>
      <td>32.200001</td>
      <td>32.400002</td>
      <td>32.900002</td>
      <td>...</td>
      <td>49.900002</td>
      <td>50.799999</td>
      <td>51.700001</td>
      <td>52.599998</td>
      <td>53.500000</td>
      <td>54.400002</td>
      <td>55.299999</td>
      <td>56.200001</td>
      <td>57.099998</td>
      <td>57.099998</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 29 columns</p>
</div>



**Observations:**
* Data present for 189 countries for years 1980 - 2007 
* All years contain 175 non-null entries with no missing data for any country for any year

**Next Steps:**
* Extract revelant years data for analysis (determined later)  

### 4. Child Mortality Rates


```python
df_cmr = pd.read_csv('data/mortality_rates_under5.csv')

# Uncomment to explore data
#df_cmr.info()
df_cmr.head()
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
      <th>CME under 5 mortality</th>
      <th>1931</th>
      <th>1932</th>
      <th>1933</th>
      <th>1934</th>
      <th>1935</th>
      <th>1936</th>
      <th>1937</th>
      <th>1938</th>
      <th>1939</th>
      <th>...</th>
      <th>2002</th>
      <th>2003</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>129.2</td>
      <td>125.9</td>
      <td>122.7</td>
      <td>119.4</td>
      <td>116.3</td>
      <td>113.4</td>
      <td>109.7</td>
      <td>106.7</td>
      <td>103.9</td>
      <td>101.1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>23.5</td>
      <td>22.2</td>
      <td>21.0</td>
      <td>19.7</td>
      <td>18.7</td>
      <td>17.8</td>
      <td>16.9</td>
      <td>15.8</td>
      <td>15.0</td>
      <td>14.3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>42.3</td>
      <td>40.7</td>
      <td>39.0</td>
      <td>37.7</td>
      <td>36.4</td>
      <td>34.9</td>
      <td>33.5</td>
      <td>32.1</td>
      <td>31.3</td>
      <td>29.8</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Andorra</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>4.6</td>
      <td>4.4</td>
      <td>4.3</td>
      <td>4.1</td>
      <td>4.0</td>
      <td>3.9</td>
      <td>3.7</td>
      <td>3.6</td>
      <td>3.5</td>
      <td>3.3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Angola</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>191.2</td>
      <td>187.1</td>
      <td>183.3</td>
      <td>179.1</td>
      <td>175.6</td>
      <td>172.0</td>
      <td>167.4</td>
      <td>164.5</td>
      <td>161.0</td>
      <td>157.6</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 82 columns</p>
</div>



**Observations:**
* Data present for 260 countries for years starting at year 1931 to 2011
* Missing data for years upto 1989. Highest number of non-null entries (195) present for years 1990-2011 

**Next Steps:**
* Columns upto year 1989 will be dropped due to extreme amount of missing data   
* Remove countries with no data present (260-195 = 65 countries) 
* Final dataset will contain child mortality rates for year 1990-2011 for 195 countries 

### 5. Female Literacy Rate 


```python
df_literacy = pd.read_csv('data/female_literacy_rate.csv')

# Uncomment to explore data
#df_literacy.info()
df_literacy.head()
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
      <th>Adult (15+) literacy rate (%). Female</th>
      <th>1975</th>
      <th>1976</th>
      <th>1977</th>
      <th>1978</th>
      <th>1979</th>
      <th>1980</th>
      <th>1981</th>
      <th>1982</th>
      <th>1983</th>
      <th>...</th>
      <th>2002</th>
      <th>2003</th>
      <th>2004</th>
      <th>2005</th>
      <th>2006</th>
      <th>2007</th>
      <th>2008</th>
      <th>2009</th>
      <th>2010</th>
      <th>2011</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.98746</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>13.00000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>94.681814</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>95.69148</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>60.075082</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>63.918785</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Andorra</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Angola</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>58.60846</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 38 columns</p>
</div>



**Observations:**
* Female literacy rate has data for 260 countries for years 1975 - 2011
* No year contains data for all 260 countries with year 2011 containing highest number of non-null entries(83)

**Next Steps:**
* **Due to extreme amounts of missing data for all years, female literacy rate will be dropped from the analysis**
* As a substitute to gauge women literacy, mean years in school for females will be used which has complete data for 175 countries from 1970 - 2009

## Data Cleaning 
### 1. Mean School Years 
Data for mean school years for females is already complete requiring minimal cleaning 


```python
# Uncomment to explore data further
#df_school.info()

# Raname first column to 'Country'
df_school.rename(columns={df_school.columns[0]: 'Country'}, inplace=True)
```

### 2. Mean Age at Marriage
**Todo:** Due to extreme amount of missing data for most years, drop all years except 2005 


```python
df_marriage.head()
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
      <th>Unnamed: 0</th>
      <th>1616</th>
      <th>1666</th>
      <th>1685</th>
      <th>1710</th>
      <th>1716</th>
      <th>1735</th>
      <th>1760</th>
      <th>1766</th>
      <th>1775</th>
      <th>...</th>
      <th>1996</th>
      <th>1997</th>
      <th>1998</th>
      <th>1999</th>
      <th>2000</th>
      <th>2001</th>
      <th>2002</th>
      <th>2003</th>
      <th>2004</th>
      <th>2005</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>17.839683</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23.326509</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>27.6</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>29.600000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Angola</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Argentina</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23.263962</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 117 columns</p>
</div>




```python
# Rename first column to 'Country'
df_marriage.rename(columns={df_marriage.columns[0]: 'Country'}, inplace=True)

# Drop all columns except 'Country' and '2005'
df_marriage = df_marriage.loc[:, ['Country', '2005']]
```

The new marriage age dataframe only contains data for year 2005, however, it still contains null values for many countries which must be dropped


```python
# Drop null values in year 2005
df_marriage.dropna(inplace=True)
df_marriage.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 175 entries, 0 to 184
    Data columns (total 2 columns):
    Country    175 non-null object
    2005       175 non-null float64
    dtypes: float64(1), object(1)
    memory usage: 4.1+ KB


### 3. Female Labour Participation
Data for female labour participation is already complete requiring minimal cleaning 


```python
# Uncomment to explore data
#df_labor.info()

# Raname first column to 'Country'
df_labor.rename(columns={df_labor.columns[0]: 'Country'}, inplace=True)
```

### 4. Child Mortality Rates
**Todo:** 
* Due to extreme amount of missing data in early years, drop all years upto and including 1989
* Clean up column names and drop any null values for years 1990-2011

To keep columns for years 1990-2011, need to find column index of year '1990'


```python
# Find column index of 1990 
index_1990 = df_cmr.columns.get_loc('1990')

# Drop all columns except 'Country' and 1990-2011
df_cmr.drop(df_cmr.iloc[:, 1:index_1990], inplace=True, axis=1)
```

The new Child Mortality Rate dataframe only contains data for years of interest, however, it still contains null values for many countries which must be dropped


```python
# Rename first column to 'Country'
df_cmr.rename(columns={df_cmr.columns[0]: 'Country'}, inplace=True)

# Drop null values for all remaining years 
df_cmr.dropna(inplace=True)

#Uncomment to explore final data
#df_cmr.info()
```

## Cleaned Datasets
1. **Mean Years in School**: Data for 175 countries for years 1970-2009
2. **Mean Age at Marriage**: Data for 175 countries for year 2005
3. **Female Labour Participation**: Data for 189 countries for years 1980-2007
4. **Child Mortality Rates**: Data for 195 countries for years 1990-2011

To perform comparative analysis between indicators, further manipulation is required by creating a new dataframe for every year of interest. 

**Merged Dataset**: All four cleaned datasets will be joined for year 2005 which has complete data for each indicator. 





### Year 2005 Merged 


```python
# Merged Dataset: Using reduce to merge multiple dataframes
from functools import reduce
join_cols = ["Country", "2005"]
dfs = [df_school.loc[:, join_cols], df_marriage.loc[:, join_cols], 
            df_labor.loc[:, join_cols], df_cmr.loc[:, join_cols]]

# Merge
df_2005 = reduce(lambda left,right: pd.merge(left,right,on='Country'), dfs)
df_2005.head()
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
      <th>Country</th>
      <th>2005_x</th>
      <th>2005_y</th>
      <th>2005_x</th>
      <th>2005_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>0.6</td>
      <td>17.839683</td>
      <td>29.200001</td>
      <td>119.4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>10.3</td>
      <td>23.326509</td>
      <td>55.599998</td>
      <td>19.7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>6.5</td>
      <td>29.600000</td>
      <td>36.200001</td>
      <td>37.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Argentina</td>
      <td>11.1</td>
      <td>23.263962</td>
      <td>56.200001</td>
      <td>17.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Armenia</td>
      <td>11.2</td>
      <td>22.986034</td>
      <td>64.599998</td>
      <td>23.2</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Rename columns 
col_names = ["Country", "school_years", "marriage_age", "labor_participation", "child_mortality"]
df_2005.columns = col_names
df_2005.head()
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
      <th>Country</th>
      <th>school_years</th>
      <th>marriage_age</th>
      <th>labor_participation</th>
      <th>child_mortality</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>0.6</td>
      <td>17.839683</td>
      <td>29.200001</td>
      <td>119.4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>10.3</td>
      <td>23.326509</td>
      <td>55.599998</td>
      <td>19.7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>6.5</td>
      <td>29.600000</td>
      <td>36.200001</td>
      <td>37.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Argentina</td>
      <td>11.1</td>
      <td>23.263962</td>
      <td>56.200001</td>
      <td>17.2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Armenia</td>
      <td>11.2</td>
      <td>22.986034</td>
      <td>64.599998</td>
      <td>23.2</td>
    </tr>
  </tbody>
</table>
</div>






<a id='eda'></a>
# Exploratory Data Analysis

## 1. Individual Indicator Analysis
This analysis focuses on first <a href="#questions">Research Question</a> which compares how female literacy has changed in different regions around world.

### Mean Years in School 
We're looking at the following trends over time: 
1. Top 4 countries with highest education in 1970
2. Top 4 countries with lowest education in 1970
3. Difference in education levels between 2009 and 1970 

Currently, data for each year is a separate column. To make further analysis easier, we're going to combine data for all years under one column "Year". This can be easily done using pandas `melt` function. 


```python
# Melt df_school 
melted_school = pd.melt(df_school, id_vars=["Country"], 
                 var_name="Year", value_name="MeanYearsInSchool")
melted_school.head()
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
      <th>Country</th>
      <th>Year</th>
      <th>MeanYearsInSchool</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>1970</td>
      <td>0.1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>1970</td>
      <td>5.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>1970</td>
      <td>1.4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Angola</td>
      <td>1970</td>
      <td>0.9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Antigua and Barbuda</td>
      <td>1970</td>
      <td>8.1</td>
    </tr>
  </tbody>
</table>
</div>



### Top 5 Countries in 1970
First, we're looking at how the education levels have changed over time in countries with the highest education in 1970. To observe trends for each country, group the data by `Country` and plot each series


```python
# Extract top 5 countries with highest education levels in 1970
highestEdu = melted_school.query("Year == '1970'").nlargest(4,'MeanYearsInSchool')

# Extract all rows where country is one of top 4 countries
highEdu_df = melted_school.loc[melted_school['Country'].isin(highestEdu['Country'])]

# Group data
groups = highEdu_df.groupby('Country')
```


```python
import matplotlib.pyplot as plt
import seaborn as sns 
% matplotlib inline

fig, ax = plt.subplots()
# Plot each country group
for name, group in groups:
    group.plot(x='Year', y='MeanYearsInSchool', ax=ax, label=name, figsize=(9,7))

# Limit tick labels to 10 
plt.locator_params(axis='x', nbins=10)
ax.xaxis.set(ticklabels=np.arange(1965, 2010, 5))

# Add plot elements
ax.legend()
plt.title('Female Literacy for Countries with Highest Education Levels in 1970')
plt.ylabel('Mean Years in School')
```




    Text(0,0.5,'Mean Years in School')




![png](output_45_1.png)


United States had the highest education for females in 1970 but the rate of increase has been considerably slower since then. Increasing costs of higher education is a potential factor in the slow increase. An interesting question to explore for readers would be to explore the relationship between tuition costs and years in school. 

However, female education in Canada has increased at a much greater pace with females having about one more mean year of education compared to United States. 

Australia and Czech Republic have both seen a greater increase than United States. 

### Bottom 5 Countries in 1970
Similarly, we're looking at how the education levels have changed over time in countries with the lowest education in 1970.


```python
# Extract bottom 5 countries with highest education levels in 1970
lowestEdu = melted_school.query("Year == '1970'").nsmallest(4,'MeanYearsInSchool')

# Extract all rows where country is one of bottom 5 countries
lowestEdu_df = melted_school.loc[melted_school['Country'].isin(lowestEdu['Country'])]

# Group data
groups = lowestEdu_df.groupby('Country')
```


```python
import matplotlib.pyplot as plt
import seaborn as sns 

fig, ax = plt.subplots()
# Plot each country group
for name, group in groups:
    group.plot(x='Year', y='MeanYearsInSchool', ax=ax, label=name, figsize=(9,7))

# Limit tick labels to 10 
plt.locator_params(axis='x', nbins=10)
ax.xaxis.set(ticklabels=np.arange(1965, 2010, 5))

# Add plot elements
ax.legend()
plt.title('Female Literacy for Countries with Lowest Education Levels in 1970')
plt.ylabel('Mean Years in School')
```




    Text(0,0.5,'Mean Years in School')




![png](output_49_1.png)


The increases in all bottom countries insignificant in terms of absolute increase in number of years. Yemen had the greatest increase of about 1.5 years over four decades. Even though the increase is a promising sign, more efforts must be made to observe more significant changes. 

The next analysis looks closer at countries that have had the highest or lowest changes in education of females. 

### Differences in Education from 1970 to 2009
We want to compare changes in mean years of schooling over time. To visualize the distribution of changes in schooling years around the world, a violin plot was purposely chosen over a box plot. This is because a violin plot highlights specific densities in data that deserve more attention compared to a boxplot that can sometimes hide features of the data.


```python
# Create new column of difference in mean years between 2009 and 1970
df_school['Diff'] = df_school["2009"] - df_school["1970"]

fig, ax = plt.subplots(figsize=(8,6))
sns.violinplot(x='Diff', data=df_school)
plt.xlabel('Increase in Female Schooling (Years) from 1970 to 2009');
```


![png](output_52_0.png)


Overall, the level of female education has increased by about 5 years for most countries. More importantly, the lower quartile which is the 25th percentile is approximately 4 years implying 75% of countries have better provisions for female literacy in 2009 than they did in 1970. 

The greater width of the violin plot around 4 to 6 years of schooling is a great sign as it indicates a higher proportion of countries have seen increases in the specific range.  

## 2. Female Literacy vs Mean Age at First Marriage
This analysis uses simple linear regression to investigate trends between female literacy and age at first marriage in a country. Intuitively, we except an increase in mean age at first marriage in countries where female literacy is higher. 

Ideally, we want to compare changes in the relationship over years but due to unavailability of clean data for marraige age, the regression analysis is done using data only for 2005. Prior to analyzing specific metrics of the linear model, it helps you visualize the data. 


```python
# Visualize marriage age vs years in school
fig, ax = plt.subplots(figsize=(8,6))
sns.regplot(x=df_2005["school_years"], y=df_2005["marriage_age"],ci=None, line_kws={"color":"b", "lw":2})

# Plot elements
plt.title('Female Age at First Marriage vs Years in School')
plt.xlabel('Mean Years in School')
plt.ylabel('Mean Age at First Marriage');
```


![png](output_55_0.png)


As expected, there is a relationship between education level and age at first marriage for females. To further examine this relationship quantitatively, a linear regression model is fitted to the data below. 


```python
import statsmodels.api as sm;

# Add intercept and Fit linear model for school years
df_2005['intercept'] = 1
lm = sm.OLS(df_2005['marriage_age'], df_2005[['intercept', 'school_years']])
results = lm.fit()
print(results.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:           marriage_age   R-squared:                       0.442
    Model:                            OLS   Adj. R-squared:                  0.438
    Method:                 Least Squares   F-statistic:                     117.3
    Date:                Sun, 22 Jul 2018   Prob (F-statistic):           1.69e-20
    Time:                        18:36:53   Log-Likelihood:                -358.55
    No. Observations:                 150   AIC:                             721.1
    Df Residuals:                     148   BIC:                             727.1
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ================================================================================
                       coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------
    intercept       18.4580      0.560     32.975      0.000      17.352      19.564
    school_years     0.6557      0.061     10.833      0.000       0.536       0.775
    ==============================================================================
    Omnibus:                        7.179   Durbin-Watson:                   1.982
    Prob(Omnibus):                  0.028   Jarque-Bera (JB):                7.409
    Skew:                           0.543   Prob(JB):                       0.0246
    Kurtosis:                       2.910   Cond. No.                         24.1
    ==============================================================================
    
    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.


<a id='regression'></a>
### Regression Interpretation
The output above looks intimidating to anyone without any background in statistics and linear regression models. Let's break down the most important aspects for our analysis. 

* **R-Squared**: Most people even vaguely familiar with linear regression have heard about R-squared. The R-squared is always between 0 and 100% and is a measure of how close the data are to the fitted line. Contrary to how widespread R-squared is, it is not as useful in indicating whether a model is a good fit. Although a high R-squared value is desired, a low R-squared does not mean the model is useless. **Standard Error** is a better indicator preferred by most statistians. 


* **Standard Error (`std err`)**: Standard error (S) is in the units of response variable (`marriage_age`) and is the average distance that the observed values fall from the regression line. In other words, it is a predictor of how wrong the regression model is on average. The value of S is 0.061 years which is a relatively low value for the purposes of our analysis as we're not interested in using the model for prediction purposes. 


* **P-value (`P>|t|`)**: Assuming that the null hypothesis is true, which in this analysis implies that the coefficient for `school_years` is actually 0 instead of 0.6557, p-value is the probability of how likely it is that we observe an effect equal to or greater than the effect observed in the analysis. Hence, a lower p-value is better implying that it is less likely that the observed effect is possible even if the actual value is 0. Thus, the p-value for `school_years` is extremely close to 0% which means `school_years` is a significant predictor in this model.  


* **Regression Coefficients (`coef(β)` )**: The two regression coefficients are `intercept` and `school_years` where the `intercept` value is almost always useless due to statistical reasons not necessary to dive deep into here. The coefficient for `school_years` is **0.6557** which means that for every extra year of education a female receives, on *average* the age at first marriage increases by 0.6557 years. 

* **Confidence Intervals (`95% CI`)**:  The 95% Confidence Interval means that if we were to redo this study with different data samples many more times, the **true** value of the coefficient (which is unknown and we'll probably never know) would lie in confidence intervals in 95% of the studies. Example: If we repeat this study 20 times, the true value of coefficient `school_years` will lie within respective confidence intervals of 19 studies. 


There are many other parameters that can be analyzed for the model but the factors above are a solid starting point in interpreting the fit. 




### Female Literacy vs Marriage Age Conclusion
The goal is to explore the general trend between the variables instead of using it for predictive purposes. Based on this purpose, the linear model does a satisfactory job in highlighting how the marriage age of females aorund the world increases as they acquire more education. 

Education allows women to gain greater autonomy over marital decisions and the next analysis tries to investigate this further and see whether female labor engagement is affected by literacy and autonomy in society. 

## 3. Effect of Female Literacy and Autonomy on Female Labor Engagement in Society 
We saw how female literacy is related to higher female age at first marriage. The next question is to investigate whether female literacy and autonomy (age at first marriage) affect female labor participation in society. 


```python
# Fit linear model 
lm = sm.OLS(df_2005['labor_participation'], df_2005[['intercept', 'school_years', 'marriage_age']])
results = lm.fit()
print(results.summary())
```

                                 OLS Regression Results                            
    ===============================================================================
    Dep. Variable:     labor_participation   R-squared:                       0.007
    Model:                             OLS   Adj. R-squared:                 -0.006
    Method:                  Least Squares   F-statistic:                    0.5240
    Date:                 Sun, 22 Jul 2018   Prob (F-statistic):              0.593
    Time:                         18:36:53   Log-Likelihood:                -628.89
    No. Observations:                  150   AIC:                             1264.
    Df Residuals:                      147   BIC:                             1273.
    Df Model:                            2                                         
    Covariance Type:             nonrobust                                         
    ================================================================================
                       coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------
    intercept       54.7728      9.839      5.567      0.000      35.329      74.217
    school_years     0.4158      0.493      0.843      0.400      -0.559       1.390
    marriage_age    -0.0637      0.500     -0.127      0.899      -1.052       0.925
    ==============================================================================
    Omnibus:                        3.716   Durbin-Watson:                   1.637
    Prob(Omnibus):                  0.156   Jarque-Bera (JB):                3.423
    Skew:                          -0.368   Prob(JB):                        0.181
    Kurtosis:                       3.075   Cond. No.                         193.
    ==============================================================================
    
    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.


### Regression Interpretation
My intuition that the female labor participation increases as female education and autonomy in society increases is not supported by the linear model at all. A quick review of the model parameters highlights there is almost no correlation between labor participation and female education, marriage age. 

* **R-squared**: The R-squared value is 0.7% which suggests almost no correlation between the variables 
* **P-value**: The p-values for both predictors (`school_years, marriage_age`) is extremely high suggesting that the effects observed in our analysis have a high probability of being due to chance. 

A scatterplot of `school_years` versus `labor_participation` highlights the lacks of relationship and how some countries with extremely low education levels have some of the highest levels of female labor participation. 


```python
fig, ax = plt.subplots(figsize=(8,6))
sns.regplot(x=df_2005["school_years"], y=df_2005["labor_participation"],ci=None, line_kws={"color":"b", "lw":2})

# Plot elements
plt.title('Female Labor Participation vs Years in School')
plt.xlabel('Mean Years in School')
plt.ylabel('Female Labor Participation');
```


![png](output_64_0.png)


## 4. Effect of Female Literacy and Autonomy in Society on Child Mortality Rates
Research has shown how empowering women through education pays dividends by providing opportunities and choice to women and uplifts the health status of whole family. Our previous analysis showed how women's education is correlated with a higher age at marriage. 

Furthermore, females with higher education tend to have smaller families. The following studies on effects of gender inequality and female literacy on child mortality rates suggest we'd expect to see a decrease in mortality rates as women have greater education and autonomy in society. 
* [Association between gender inequality index and child mortality rates](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4353466/)
* [The Impact of Female Literacy on Infant Mortality Rate in Indian States](https://www.alliedacademies.org/articles/the-impact-of-female-literacy-on-infant-mortality-rate-in-indian-states.pdf)

### Choosing Predictors 
Both age at marriage and schooling years have been separately shown to be correlated with child mortality rates. 

With lack of research on effect of labor participation on child mortality rates, we look at a plot of `child_mortality` versus `labor_participation`.


```python
fig, ax = plt.subplots(figsize=(8,6))
sns.regplot(x=df_2005["labor_participation"], y=df_2005["child_mortality"],ci=None, line_kws={"color":"b", "lw":2})

# Plot elements
plt.title('Female Labor Participation vs Child Mortality Rates')
plt.ylabel('Child Mortality Rate (per 1000 children)')
plt.xlabel('Female Labor Participation(%)');
```


![png](output_67_0.png)


Based on the scatterplot above, no trend can be observed as child mortality is spread over all ranges of female labor participation. Hence, female labor participation in a society will not be included as a predictor in the multiple regression model. 





```python
# Fit linear model without labor participation as a factor
lm = sm.OLS(df_2005['child_mortality'], df_2005[['intercept', 'school_years', 'marriage_age']])
results = lm.fit()
print(results.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:        child_mortality   R-squared:                       0.671
    Model:                            OLS   Adj. R-squared:                  0.666
    Method:                 Least Squares   F-statistic:                     149.8
    Date:                Sun, 22 Jul 2018   Prob (F-statistic):           3.34e-36
    Time:                        18:36:53   Log-Likelihood:                -711.50
    No. Observations:                 150   AIC:                             1429.
    Df Residuals:                     147   BIC:                             1438.
    Df Model:                           2                                         
    Covariance Type:            nonrobust                                         
    ================================================================================
                       coef    std err          t      P>|t|      [0.025      0.975]
    --------------------------------------------------------------------------------
    intercept      193.3387     17.065     11.329      0.000     159.614     227.063
    school_years    -8.9704      0.855    -10.489      0.000     -10.660      -7.280
    marriage_age    -2.8708      0.867     -3.310      0.001      -4.585      -1.157
    ==============================================================================
    Omnibus:                       12.186   Durbin-Watson:                   1.997
    Prob(Omnibus):                  0.002   Jarque-Bera (JB):               13.027
    Skew:                           0.615   Prob(JB):                      0.00148
    Kurtosis:                       3.755   Cond. No.                         193.
    ==============================================================================
    
    Warnings:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.


### Regression Results 
Child mortality rates are negatively associated with marriage age (`β = -2.8708; 95% CI [-4.585, -1.157]`) and schooling years (`β = -10.489; 95% CI [-10.660, -7.280]`). 

> Regression parameters recap: <a href="#regression">Regression Interpretation</a></li>

Schooling years has a greater impact on the child mortality rates with a decrease of 10.489 child deaths on *average* for every extra year of female schooling. The age of marriage also lowers the mortality rates on average by 2.87 fewer deaths. 

### Model checking 
Multicollinearity is an issue that occurs when independent variables in a regression model are correlated. This correlation is a problem because independent variables should be *independent*. If the degree of correlation between variables is high enough, it can cause problems when you fit the model and interpret the results.

Based on our previous analysis of `marriage age` and `schooling years`, we know that the two predictors are correlated to an extent. Hence, we perform a test to identify the strength of that correlation and whether the model requires changes to mitigate multicollinearity effects. 

#### Variance Inflation Factor (VIF)
Variance inflation factor (VIF) is a test that identifies correlation between independent variables and the strenght of that correlation. VIFs start at 1 and have no upper limit. 
* A value of 1 indicates that there is no correlation between this independent variable and any others. 
* VIFs between 1 and 5 suggest that there is a moderate correlation, but it is not severe enough to warrant corrective measures. 
* VIFs greater than 5 represent critical levels of multicollinearity where the coefficients are poorly estimated, and the p-values are questionable.

The following cells calculate the VIF for variables used in the previous model. 


```python
from patsy import dmatrices
import statsmodels.api as sm
from statsmodels.stats.outliers_influence import variance_inflation_factor
# get y and X dataframes based on regression:
y, X = dmatrices('child_mortality ~ school_years + marriage_age', df_2005, return_type='dataframe')
```


```python
# For each X, calculate VIF and save in dataframe
vif = pd.DataFrame()
vif["VIF Factor"] = [variance_inflation_factor(X.values, i) for i in range(X.shape[1])]
vif["features"] = X.columns

# Print VIF 
vif.round(1)
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
      <th>VIF Factor</th>
      <th>features</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>55.5</td>
      <td>Intercept</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.8</td>
      <td>school_years</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.8</td>
      <td>marriage_age</td>
    </tr>
  </tbody>
</table>
</div>



A value of 1.8 suggests that the extent of correlation between the predictors is not severe enough to warrant corrective measures. 

<a id='conclusions'></a>
## Conclusions

The different analyses performed provide the following insights: 
* The level of female education has improved all around the world between 1970 and 2005 with an average increase of 4 to 6 years of additional education in most countries. 
* Marriage age and female literacy are associated with an average increase of `0.6557 years (95% CI: [0.536, 0.775])` in age at first marriage for every extra year of education. 
* No corellations were observed between female literacy and autonomy on the labor participation of females in a society. 

### Limitations 
* The results of regression analysis are geared towards understanding the associations between the predictors and not to be used to accurate predictive purposes. 
* We cannot state that female autonomy and literacy **cause** lower child mortality rates as the data is obtained from observational studies. There are a multitude of factors that affect child mortality rates and the factors change depending on the region of the world and its social and economic norms. 

**We have observed statistically significant associations between female literacy, autonomy and child mortality rates. This suggests that initiatives to curtail child mortality rates should extent beyond providing better medical services and focus on prioritizing women's rights and autonomy in societies. Greater education for females provides greater autonomy over their decisions and focusing on social well being of females may augment the survival of children.**
