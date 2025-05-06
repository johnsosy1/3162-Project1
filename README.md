## Spring 2025 - ITSC 3162 - Data Mining - Project 1
#### Sydney Johnson
# Los Angeles Arrest Numbers

<hr>

## Introduction

Los Angeles, California is located in southern California and known for being the center of the nation's television and film industry. The beautiful city spands 502 square miles which is the equalivalent of 385,000 football fields. LA has a population of 3.281 million people, which makes it the second most populated city besides New York City, New York. Unfortunately, besides all the glitz and glamour of the city, LA has a high noteable crime rate compared to the rest of the state. In this project, I will covering a data set that contains Los Angele's crime rates from 2020 to the current year. 

I'll be answering the following questions:  
    - How are crime rates spread amoungest the different areas that make up Los Angeles?  
    - What do these crime rates statistics look like based off gender?   
    - Is there an age trend to those involved in these crime statistics?  

My dataset: https://www.kaggle.com/datasets/arsri1/arrest-data-in-los-angeles 
- The features of this dataset includes information on details such as the date of arrest, charges, demographics of the arrested individuals, and the location of incidents.


<hr>

## Pre-Processing

Let's start by getting the data cleaned up.  

- First, I installed the data software I wanted to use and imported it into the notebook. (I put the imports in comments to make it look neater)


```python
#!pip install pandas
#!pip install seaborn
#pip install matplotlib
```


```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
```

- Now, lets read the date file and talk through what we're looking at.


```python
arrest_df = pd.read_csv("arrest_data.csv")
arrest_df.head()
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
      <th>Report ID</th>
      <th>Report Type</th>
      <th>Arrest Date</th>
      <th>Time</th>
      <th>Area ID</th>
      <th>Area Name</th>
      <th>Reporting District</th>
      <th>Age</th>
      <th>Sex Code</th>
      <th>Descent Code</th>
      <th>...</th>
      <th>Disposition Description</th>
      <th>Address</th>
      <th>Cross Street</th>
      <th>LAT</th>
      <th>LON</th>
      <th>Location</th>
      <th>Booking Date</th>
      <th>Booking Time</th>
      <th>Booking Location</th>
      <th>Booking Location Code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6636966</td>
      <td>BOOKING</td>
      <td>07/06/2023 12:00:00 AM</td>
      <td>2250.0</td>
      <td>8</td>
      <td>West LA</td>
      <td>817</td>
      <td>46</td>
      <td>M</td>
      <td>B</td>
      <td>...</td>
      <td>MISDEMEANOR COMPLAINT FILED</td>
      <td>900    GAYLEY                       AV</td>
      <td>NaN</td>
      <td>34.0637</td>
      <td>-118.4482</td>
      <td>POINT (-118.4482 34.0637)</td>
      <td>07/07/2023 12:00:00 AM</td>
      <td>143.0</td>
      <td>METRO - JAIL DIVISION</td>
      <td>4273.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6637119</td>
      <td>BOOKING</td>
      <td>07/07/2023 12:00:00 AM</td>
      <td>1000.0</td>
      <td>3</td>
      <td>Southwest</td>
      <td>396</td>
      <td>39</td>
      <td>M</td>
      <td>B</td>
      <td>...</td>
      <td>MISDEMEANOR COMPLAINT FILED</td>
      <td>40TH                         PL</td>
      <td>VERMONT</td>
      <td>34.0100</td>
      <td>-118.2915</td>
      <td>POINT (-118.2915 34.01)</td>
      <td>07/07/2023 12:00:00 AM</td>
      <td>1156.0</td>
      <td>77TH ST</td>
      <td>4212.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6624479</td>
      <td>BOOKING</td>
      <td>06/15/2023 12:00:00 AM</td>
      <td>1850.0</td>
      <td>7</td>
      <td>Wilshire</td>
      <td>724</td>
      <td>33</td>
      <td>F</td>
      <td>H</td>
      <td>...</td>
      <td>MISDEMEANOR COMPLAINT FILED</td>
      <td>100    THE GROVE                    DR</td>
      <td>NaN</td>
      <td>34.0736</td>
      <td>-118.3563</td>
      <td>POINT (-118.3563 34.0736)</td>
      <td>06/15/2023 12:00:00 AM</td>
      <td>2251.0</td>
      <td>77TH ST</td>
      <td>4212.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6636128</td>
      <td>BOOKING</td>
      <td>07/05/2023 12:00:00 AM</td>
      <td>1550.0</td>
      <td>2</td>
      <td>Rampart</td>
      <td>218</td>
      <td>30</td>
      <td>F</td>
      <td>B</td>
      <td>...</td>
      <td>MISDEMEANOR COMPLAINT FILED</td>
      <td>1000    ECHO PARK                    AV</td>
      <td>NaN</td>
      <td>34.0741</td>
      <td>-118.2590</td>
      <td>POINT (-118.259 34.0741)</td>
      <td>07/05/2023 12:00:00 AM</td>
      <td>1940.0</td>
      <td>METRO - JAIL DIVISION</td>
      <td>4273.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>6636650</td>
      <td>BOOKING</td>
      <td>07/06/2023 12:00:00 AM</td>
      <td>1335.0</td>
      <td>12</td>
      <td>77th Street</td>
      <td>1258</td>
      <td>31</td>
      <td>M</td>
      <td>H</td>
      <td>...</td>
      <td>NaN</td>
      <td>7800 S  BROADWAY</td>
      <td>NaN</td>
      <td>33.9689</td>
      <td>-118.2783</td>
      <td>POINT (-118.2783 33.9689)</td>
      <td>07/06/2023 12:00:00 AM</td>
      <td>1345.0</td>
      <td>77TH ST</td>
      <td>4212.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 25 columns</p>
</div>



- I want to show all the columns included in this dataset before I remove most of them:


```python
arrest_df.columns
```




    Index(['Report ID', 'Report Type', 'Arrest Date', 'Time', 'Area ID',
           'Area Name', 'Reporting District', 'Age', 'Sex Code', 'Descent Code',
           'Charge Group Code', 'Charge Group Description', 'Arrest Type Code',
           'Charge', 'Charge Description', 'Disposition Description', 'Address',
           'Cross Street', 'LAT', 'LON', 'Location', 'Booking Date',
           'Booking Time', 'Booking Location', 'Booking Location Code'],
          dtype='object')



- For the purpose of this project I only want to use 3 out of the 25 columns given:     
      1. Area Name  
      2. Age  
      3. Sex Code

Let's drop several columns to make the dataset less overwhleming and focus on the key columns we want!
- I use the .drop() command to get rid of several columns to only focus on certain ones
  


```python
arrest_df = arrest_df.drop(['Report ID','Report Type', 'Arrest Date', 'Time', 'Area ID',
       'Reporting District', 'Descent Code','Charge Group Code', 'Charge Group Description', 'Arrest Type Code',
       'Charge', 'Charge Description', 'Disposition Description', 'Address',
       'Cross Street', 'LAT', 'LON', 'Location', 'Booking Date',
       'Booking Time', 'Booking Location', 'Booking Location Code'], axis = 1)

arrest_df
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
      <th>Area Name</th>
      <th>Age</th>
      <th>Sex Code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>West LA</td>
      <td>46</td>
      <td>M</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Southwest</td>
      <td>39</td>
      <td>M</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wilshire</td>
      <td>33</td>
      <td>F</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rampart</td>
      <td>30</td>
      <td>F</td>
    </tr>
    <tr>
      <th>4</th>
      <td>77th Street</td>
      <td>31</td>
      <td>M</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>334572</th>
      <td>Central</td>
      <td>35</td>
      <td>M</td>
    </tr>
    <tr>
      <th>334573</th>
      <td>Southwest</td>
      <td>16</td>
      <td>M</td>
    </tr>
    <tr>
      <th>334574</th>
      <td>N Hollywood</td>
      <td>62</td>
      <td>M</td>
    </tr>
    <tr>
      <th>334575</th>
      <td>Van Nuys</td>
      <td>27</td>
      <td>M</td>
    </tr>
    <tr>
      <th>334576</th>
      <td>Central</td>
      <td>53</td>
      <td>M</td>
    </tr>
  </tbody>
</table>
<p>334577 rows × 3 columns</p>
</div>



- The new dataset contains three columns with over 334,577 rows. Below I'm going to break apart each column to help analyze the data better.

- Here I'm checking for any null values present and as you can see with columns I've chosen, there aren't any.


```python
arrest_df.isnull().sum()
```




    Area Name    0
    Age          0
    Sex Code     0
    dtype: int64



<hr>

## #1: Area

In this section, we'll look deeper into the different areas that make up Los Angeles and where the least and most arrests have taken place.    
- First, I've sorted the data into a table grouped by the area name.


```python
#The '.groupby('Area Name') groups the data by the area, .size() counts the number of arrests listed in each area,
# .resetIndex(name = 'Arrest Count'), converts the data frame so the 'Arrest Count' column can be made and clearly have the total numbers listed in it
arrest_counts = arrest_df.groupby('Area Name').size().reset_index(name = 'Arrest Count')

#'.sort_values' makes sure the ouput is ordered clearly by area name
arrest_counts = arrest_counts.sort_values('Area Name')

#This line prints/shows the created table
arrest_counts
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
      <th>Area Name</th>
      <th>Arrest Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>77th Street</td>
      <td>22784</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Central</td>
      <td>26704</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Devonshire</td>
      <td>12197</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Foothill</td>
      <td>11820</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Harbor</td>
      <td>10275</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hollenbeck</td>
      <td>11252</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hollywood</td>
      <td>19002</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Mission</td>
      <td>15474</td>
    </tr>
    <tr>
      <th>8</th>
      <td>N Hollywood</td>
      <td>16355</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Newton</td>
      <td>19043</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Northeast</td>
      <td>11383</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Olympic</td>
      <td>14829</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Pacific</td>
      <td>20230</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Rampart</td>
      <td>26652</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Southeast</td>
      <td>15872</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Southwest</td>
      <td>17521</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Topanga</td>
      <td>13647</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Van Nuys</td>
      <td>18281</td>
    </tr>
    <tr>
      <th>18</th>
      <td>West LA</td>
      <td>10704</td>
    </tr>
    <tr>
      <th>19</th>
      <td>West Valley</td>
      <td>11282</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Wilshire</td>
      <td>9270</td>
    </tr>
  </tbody>
</table>
</div>



- Above, we Have 20 different areas listed with their corresponding total arrest counts. While this informative, I've made two visuals to better talk about with this data is showing us.

### Bar Graph
- For our first graph in this section, I'm going to use a bar graph. This graph will show the spread of numbers in each area. 


```python
#Using the matplotlib as plt

plt.figure(figsize = (12, 6)) #Sets the size of the graph

#plt.bar(x = area names, y = arrest_counts, and I set the color to purple because I thought it was pretty.
#Decided to use plt rather than sns for this because it gave me better manual control
plt.bar(arrest_counts['Area Name'], arrest_counts['Arrest Count'], color = 'purple')

#Set the axis labels and titles
plt.xlabel("Area Name")
plt.ylabel("Number of Arrests")
plt.title("Number of Arrests by Area in LA")
plt.xticks(rotation = 45, ha = "right") #Rotates area names for better readability
#Using .grid() to apply the grid to the y-axis and I wanted the line style to be dashed
plt.grid(axis = 'y', linestyle = '--')

```


    
![png](output_24_0.png)
    


### Heatmap
- Heatmaps are very good with capturing the eye to the different contrasts in a dataset through color density.


```python
heatmap_data = arrest_counts.set_index('Area Name') #Set the "Area Name" column as the index of the arrest_counts df

plt.figure(figsize = (12,6)) #Set the figure size

# Using sns is better here
# annot = True: displays the values inside of the heatmap
# "PuRd" is the purple color scheme (matches the bar chart theme)
# fmt = d: Format as integers
sns.heatmap(heatmap_data, annot = True, cmap = "PuRd", linewidths = 1, fmt = "d")

plt.title("Heatmap of Arrests by Area") 
plt.xlabel("Arrests")
plt.ylabel("Area Name")
```




    Text(120.72222222222221, 0.5, 'Area Name')




    
![png](output_26_1.png)
    


### Area Summary
- With the bar chart and the heatmap, we can identify the top three areas witht the most and least arrest counts:
    - Most:
        1. Central - 26,704 arrests     
        2. Rampart - 26,652 arrests
        3. 77th Street - 22,784 arrests
  
    - Least:
        1. Wilshire - 9,270 arrests
        2. Harbor - 10,275 arrests
        3. West LA - 10,704 arrests

When you are just looking at this data, you only see the number value of arrests in these areas. You don't see the deeper reason for why these areas may contain such high or low numbers. For instance, is there poverty, low or high education, low or high real estate, etc. The limited story that my data is telling are the contrasting arrest numbers for different parts of the city that you may want to keep in mind if you live there or are visiting.  


<hr>

## #2: Age
- In this section, we'll look into the age range of these arrest counts. We will able to see the spread of ages and the total number of arrests for that specfic group.


```python
#By defining bins and labels, we are categorizing the ages into predefined ranges.
bins = [0, 17, 24, 34, 44, 54, 64, 100]
labels = ['0-17', '18-24', '25-34', '35-44', '45-54', '55-64', '65+']

#The code below create the range column
arrest_df['Age Range'] = pd.cut(arrest_df['Age'], bins=bins, labels=labels)
age_counts = arrest_df.groupby('Age Range').size().reset_index(name = 'Arrest Count')

age_counts
```

    C:\Users\sydne\AppData\Local\Temp\ipykernel_18188\3076759206.py:7: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.
      age_counts = arrest_df.groupby('Age Range').size().reset_index(name = 'Arrest Count')
    




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
      <th>Age Range</th>
      <th>Arrest Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0-17</td>
      <td>9384</td>
    </tr>
    <tr>
      <th>1</th>
      <td>18-24</td>
      <td>53801</td>
    </tr>
    <tr>
      <th>2</th>
      <td>25-34</td>
      <td>120731</td>
    </tr>
    <tr>
      <th>3</th>
      <td>35-44</td>
      <td>79706</td>
    </tr>
    <tr>
      <th>4</th>
      <td>45-54</td>
      <td>41371</td>
    </tr>
    <tr>
      <th>5</th>
      <td>55-64</td>
      <td>22755</td>
    </tr>
    <tr>
      <th>6</th>
      <td>65+</td>
      <td>6636</td>
    </tr>
  </tbody>
</table>
</div>



Now that we have the table data, let's make two visuals to see our data better.
### Age Bar Graph


```python
plt.figure(figsize = (10,5)) #Sets the size of the graph
sns.barplot(data = age_counts, hue ='Age Range', y = 'Arrest Count', palette = 'pastel')

plt.xlabel("Age Range")
plt.ylabel("number of Arrests")
plt.title("Number of Arrests by Age Range")
plt.xticks(rotation = 45, ha = "right")
plt.grid(axis = 'y', linestyle='--')

```


    
![png](output_32_0.png)
    


### Age Pie Chart


```python
plt.figure(figsize = (8,8))
plt.pie(age_counts['Arrest Count'], labels = age_counts['Age Range'], autopct = '%1.1f%%', colors = sns.color_palette("pastel"))
# 'autopct = '%1.1f%%' sets the percents to one decmial place
plt.title("Proportion of Arrests by Age Range")

```




    Text(0.5, 1.0, 'Proportion of Arrests by Age Range')




    
![png](output_34_1.png)
    


### Age Summary
In conclusion, we can see that the age range of '25-34' takes the win with 36.1% of arrests taking place in this category. The age range of '35-44' is next with 23.8% of arrests being in this category. Only 16.1% of arrests for the range '18-24' which surprises me because I though this age range would be the highest. In addition, I find it surpring the there is a 2% in the 65+ category, you would think at that age people would be slowing down. 

<hr>

## #3 Sex Code
In this section, we will be looking at the number of female vs. male arrest numbers. 


```python
sex_counts = arrest_df.groupby('Sex Code').size().reset_index(name = 'Arrest Count')
sex_counts
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
      <th>Sex Code</th>
      <th>Arrest Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>F</td>
      <td>68196</td>
    </tr>
    <tr>
      <th>1</th>
      <td>M</td>
      <td>266381</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize = (6,4)) #Sets the figure size.

#To avoid an error in the color palette, I decided to go ahead and assign them directly. 
color_map = {"M":"#6495ED", "F": "#ff69b4"}
sns.barplot(data=sex_counts, x ='Sex Code', y = 'Arrest Count', hue = 'Sex Code', palette = color_map, legend = False)

plt.xlabel("Sex Code")
plt.ylabel("Number of Arrests")
plt.title("# of Arrests by Sex Code")
plt.grid(axis = 'y', linestyle = '--')

```


    
![png](output_39_0.png)
    


### Sex Code Summary
This portion of the data is pretty self-explanatory. You can see that male arrests are severely higher than female arrests. This doesn't surprise me because this statistic is fairly shared outside the city of Los Angeles.

<hr>

## Impact 
This data provides valuable insight into arrest patterns across Los Angeles, allowing us to analyze variantions based on area, age, and gender. By examining these numbers, we can identify which communities experience higher or lower arrest rates, offering a deeper understanding of crime distribution across the city.   

However, the arrest numbers alone don not tell the full story. Demogrpahics, socioeconomic factors, and policing stratgies all play a role in shaping these statistics. While some areas may have higher arrest rates, understanding the underlying reasons, such as population density or law enforcement presence, wouldadd crucial context to the data.

<hr>

## References


https://www.kaggle.com/datasets/arsri1/arrest-data-in-los-angeles
