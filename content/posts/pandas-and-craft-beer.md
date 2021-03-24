---
title: Pandas and craft beer
description: Little observation with Python Pandas
date: 2021-02-19T23:00:00.000+00:00

---
Hi, today we will discuss example of using [Python Pandas](http://pandas.pydata.org/) on little interesting dataset from [Kaggle](https://www.kaggle.com/). Dataset contains [2K+ craft canned beers from the US and 500+ breweries in the United States](https://www.kaggle.com/nickhould/craft-cans). In the beginning we need to prepare our virtual environment. The easiest way to do it is to use [Anaconda](https://www.continuum.io/downloads). In this little research I will use Python 3, and if you have not already switched from 2 to 3, you can now. You can access all materials via IPython Notebook via this [link](https://www.kaggle.com/moonchel/d/nickhould/craft-cans/little-exploration-of-craft-beer/).

At the beginning we need make out data a little bit clearer. In real data analysis world it is called [data wrangling](https://en.m.wikipedia.org/wiki/Data_wrangling). You can understand it as cleaning data from error or empty values, connect datasets through columns, like in SQL. Ok, here we go…

    import pandas as pd beers = pd.read_csv('beers.csv')
    
    breweries = pd.read_csv('breweries.csv') 
    beers.head() 

<table>
   <thead>
      <tr>
         <th>Unnamed: 0</th>
         <th>abv</th>
         <th>ibu</th>
         <th>id</th>
         <th>name</th>
         <th>style</th>
         <th>brewery_id</th>
         <th>ounces</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>0</td>
         <td>0</td>
         <td>0.050</td>
         <td>NaN</td>
         <td>1436</td>
         <td>Pub Beer</td>
         <td>American Pale Lager</td>
         <td>408</td>
         <td>12.0</td>
      </tr>
      <tr>
         <td>1</td>
         <td>1</td>
         <td>0.066</td>
         <td>NaN</td>
         <td>2265</td>
         <td>Devil&#39;s Cup</td>
         <td>American Pale Ale (APA)</td>
         <td>177</td>
         <td>12.0</td>
      </tr>
      <tr>
         <td>2</td>
         <td>2</td>
         <td>0.071</td>
         <td>NaN</td>
         <td>2264</td>
         <td>Rise of the Phoenix</td>
         <td>American IPA</td>
         <td>177</td>
         <td>12.0</td>
      </tr>
   </tbody>
</table>

This table contains some evident problems, such as NaN values in **ibu** column and useless columns **Unnamed: 0** and **id**. Firstly we can remove useless column in beers table with:

    beers.drop('Unnamed: 0', axis=1, inplace=True) 

In breweries table we will just renamed this column, because of future use in tables merge on **brewery_id** column.

    breweries.rename(columns={'Unnamed: 0' : 'brewery_id'}, inplace=True) 
    beers.drop('id', axis=1, inplace=True) 
    beers.head()

<table>
   <thead>
      <tr>
         <th></th>
         <th>abv</th>
         <th>ibu</th>
         <th>name</th>
         <th>style</th>
         <th>brewery_id</th>
         <th>ounces</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>0</td>
         <td>0.050</td>
         <td>NaN</td>
         <td>Pub Beer</td>
         <td>American Pale Lager</td>
         <td>408</td>
         <td>12.0</td>
      </tr>
      <tr>
         <td>1</td>
         <td>0.066</td>
         <td>NaN</td>
         <td>Devil&#39;s Cup</td>
         <td>American Pale Ale (APA)</td>
         <td>177</td>
         <td>12.0</td>
      </tr>
      <tr>
         <td>2</td>
         <td>0.071</td>
         <td>NaN</td>
         <td>Rise of the Phoenix</td>
         <td>American IPA</td>
         <td>177</td>
         <td>12.0</td>
      </tr>
   </tbody>
</table>

    breweries.head() 

<table>
   <thead>
      <tr>
         <th></th>
         <th>brewery_id</th>
         <th>name</th>
         <th>city</th>
         <th>state</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>0</td>
         <td>0</td>
         <td>NorthGate Brewing</td>
         <td>Minneapolis</td>
         <td>MN</td>
      </tr>
      <tr>
         <td>1</td>
         <td>1</td>
         <td>Against the Grain Brewery</td>
         <td>Louisville</td>
         <td>KY</td>
      </tr>
      <tr>
         <td>2</td>
         <td>2</td>
         <td>Jack&#39;s Abby Craft Lagers</td>
         <td>Framingham</td>
         <td>MA</td>
      </tr>
   </tbody>
</table>
Now, when we have column in beers table **brewery_id**, which is simple row index in breweries table, we can merge two tables.

    beers = beers.merge(breweries, on='brewery_id') 
    beers.head()

<table>
   <thead>
      <tr>
         <th></th>
         <th>abv</th>
         <th>ibu</th>
         <th>name_x</th>
         <th>style</th>
         <th>brewery_id</th>
         <th>ounces</th>
         <th>name_y</th>
         <th>city</th>
         <th>state</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>0</td>
         <td>0.050</td>
         <td>NaN</td>
         <td>Pub Beer</td>
         <td>American Pale Lager</td>
         <td>408</td>
         <td>12.0</td>
         <td>10 Barrel Brewing Company</td>
         <td>Bend</td>
         <td>OR</td>
      </tr>
      <tr>
         <td>1</td>
         <td>0.066</td>
         <td>NaN</td>
         <td>Devil&#39;s Cup</td>
         <td>American Pale Ale (APA)</td>
         <td>177</td>
         <td>12.0</td>
         <td>18th Street Brewery</td>
         <td>Gary</td>
         <td>IN</td>
      </tr>
      <tr>
         <td>2</td>
         <td>0.071</td>
         <td>NaN</td>
         <td>Rise of the Phoenix</td>
         <td>American IPA</td>
         <td>177</td>
         <td>12.0</td>
         <td>18th Street Brewery</td>
         <td>Gary</td>
         <td>IN</td>
      </tr>
   </tbody>
</table>

It's 99% prepared for analysis, and last things can be describe as cosmetic correction. We will replace all **NaN** with **0.0** values, and after it multiple **abv** column with 100 for increasing human readability.

    beers['abv'] *= 100 beers['ibu'].fillna(0.0, inplace=True) 
    beers['abv'].fillna(0.0, inplace=True) 
    beers.head()

<table>
   <thead>
      <tr>
         <th></th>
         <th>abv</th>
         <th>ibu</th>
         <th>name_x</th>
         <th>style</th>
         <th>brewery_id</th>
         <th>ounces</th>
         <th>name_y</th>
         <th>city</th>
         <th>state</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>0</td>
         <td>5.0</td>
         <td>0.0</td>
         <td>Pub Beer</td>
         <td>American Pale Lager</td>
         <td>408</td>
         <td>12.0</td>
         <td>10 Barrel Brewing Company</td>
         <td>Bend</td>
         <td>OR  </td>
      </tr>
      <tr>
         <td>1</td>
         <td>6.6</td>
         <td>0.0</td>
         <td>Devil&#39;s Cup</td>
         <td>American Pale Ale (APA)</td>
         <td>177</td>
         <td>12.0</td>
         <td>18th Street Brewery</td>
         <td>Gary</td>
         <td>IN</td>
      </tr>
      <tr>
         <td>2</td>
         <td>7.1</td>
         <td>0.0</td>
         <td>Rise of the Phoenix</td>
         <td>American IPA</td>
         <td>177</td>
         <td>12.0</td>
         <td>18th Street Brewery</td>
         <td>Gary</td>
         <td>IN</td>
      </tr>
   </tbody>
</table>

Ups, I forgot about **name_x** and **name_y**. When we have merged two tables, columns with same names were changed to make them unique.

    beers.rename(columns={'name_x' : 'beer', 'name_y' : 'brewery'}, inplace=True) 
    beers.drop('brewery_id', axis=1, inplace=True) 
    beers.head()

<table>
   <thead>
      <tr>
         <th></th>
         <th>abv</th>
         <th>ibu</th>
         <th>beer</th>
         <th>style</th>
         <th>ounces</th>
         <th>brewery</th>
         <th>city</th>
         <th>state</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>0</td>
         <td>5.0</td>
         <td>0.0</td>
         <td>Pub Beer</td>
         <td>American Pale Lager</td>
         <td>12.0</td>
         <td>10 Barrel Brewing Company</td>
         <td>Bend</td>
         <td>OR</td>
      </tr>
      <tr>
         <td>1</td>
         <td>6.6</td>
         <td>0.0</td>
         <td>Devil&#39;s Cup</td>
         <td>American Pale Ale (APA)</td>
         <td>12.0</td>
         <td>18th Street Brewery</td>
         <td>Gary</td>
         <td>IN</td>
      </tr>
      <tr>
         <td>2</td>
         <td>7.1</td>
         <td>0.0</td>
         <td>Rise of the Phoenix</td>
         <td>American IPA</td>
         <td>12.0</td>
         <td>18th Street Brewery</td>
         <td>Gary</td>
         <td>IN</td>
      </tr>
   </tbody>
</table>

Ok, we can start with simple analysis. At the beginning we need to understand what we want to achieve. **We can do anything we want with this data.**

How many breweries in each state?

    beers.state.value_counts().head()
    
     CO    265
     CA    183
     MI    162
     IN    139
     TX    130
    Name: state, dtype: int64

Ok, that was simple, but what if I want to know top 5 states with highest **abv** in beer?

    beers[['abv', 'state']].sort_values(by='abv', ascending=False).head() 

<table>
   <thead>
      <tr>
         <th></th>
         <th>abv</th>
         <th>state</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>2278</td>
         <td>12.8</td>
         <td>CO</td>
      </tr>
      <tr>
         <td>70</td>
         <td>12.5</td>
         <td>KY</td>
      </tr>
      <tr>
         <td>2189</td>
         <td>12.0</td>
         <td>IN</td>
      </tr>
      <tr>
         <td>2279</td>
         <td>10.4</td>
         <td>CO</td>
      </tr>
      <tr>
         <td>1857</td>
         <td>10.0</td>
         <td>NY</td>
      </tr>
   </tbody>
</table>

People usually understand statistical data better if they are represented in graphs. Pandas can help with it.

## Show me graph with sorted states by mean abv in each state.

    beers.groupby('state')['abv'].mean().sort_values( ascending=False).plot(kind='bar', ylim=(5,7), colormap='summer') 

![](https://res.cloudinary.com/ducpttwlw/image/upload/v1613839393/blog/Untitled_13_1_e5nvef.png)

Cool, here we can see one interesting thing. Even though **DC** is first in mean **abv**, **CO** has beer with highest **abv** among all states and I want to find which brewery craft it.

### REMARK - you can not just use beers\[beers\['state'\] == 'CO'\] if you didn't strip all values with str.strip

    beers[beers['state'].str.contains('CO')].sort_values(by='abv', ascending=False).head()

<table>
   <thead>
      <tr>
         <th></th>
         <th>abv</th>
         <th>ibu</th>
         <th>beer</th>
         <th>style</th>
         <th>ounces</th>
         <th>brewery</th>
         <th>city</th>
         <th>state</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>2278</td>
         <td>12.8</td>
         <td>0.0</td>
         <td>Lee Hill Series Vol. 5 - Belgian Style Quadrup...</td>
         <td>Quadrupel (Quad)</td>
         <td>19.2</td>
         <td>Upslope Brewing Company</td>
         <td>Boulder</td>
         <td>CO</td>
      </tr>
      <tr>
         <td>2279</td>
         <td>10.4</td>
         <td>0.0</td>
         <td>Lee Hill Series Vol. 4 - Manhattan Style Rye Ale</td>
         <td>Rye Beer</td>
         <td>19.2</td>
         <td>Upslope Brewing Company</td>
         <td>Boulder</td>
         <td>CO</td>
      </tr>
      <tr>
         <td>1586</td>
         <td>9.9</td>
         <td>98.0</td>
         <td>Ten Fidy Imperial Stout (2007)</td>
         <td>Russian Imperial Stout</td>
         <td>12.0</td>
         <td>Oskar Blues Brewery</td>
         <td>Lyons</td>
         <td>CO</td>
      </tr>
      <tr>
         <td>1578</td>
         <td>9.9</td>
         <td>98.0</td>
         <td>Ten Fidy Imperial Stout</td>
         <td>Russian Imperial Stout</td>
         <td>12.0</td>
         <td>Oskar Blues Brewery</td>
         <td>Longmont</td>
         <td>CO</td>
      </tr>
      <tr>
         <td>2286</td>
         <td>9.9</td>
         <td>90.0</td>
         <td>Upslope Imperial India Pale Ale</td>
         <td>American Double / Imperial IPA</td>
         <td>19.2</td>
         <td>Upslope Brewing Company</td>
         <td>Boulder</td>
         <td>CO</td>
      </tr>
   </tbody>
</table>

Ok, I have another question to my dataset. Can I please have a graph of top 5 cities sorted by breweries?

    beers.groupby('city')['brewery'].nunique().sort_values( ascending=False).head().plot(kind='bar', title='Yep, you can.', colormap='summer')

![](https://res.cloudinary.com/ducpttwlw/image/upload/v1613839393/blog/Untitled_16_1_zsoogm.png)

### Same data as table

    beers.groupby('city')['brewery'].nunique().sort_values(ascending=False).head()
    
    city
    Portland    17
    Boulder      9
    Seattle      9
    Chicago      9
    Denver       8
    Name: brewery, dtype: int64

And now I want to make beer test drive in Portland. But wait, can I consider Portland as city with highest number of beer styles?

    beers.groupby('city')['beer'].nunique().sort_values(ascending=False).head()
    
    city
    Grand Rapids    66
    Portland        53
    Chicago         52
    Indianapolis    43
    Boulder         41
    Name: beer, dtype: int64

Nope, you can see that Grand Rapids has more craft beer styles than Portland, but lower number of breweries. Grand Rapids is not even in top 5. Ok, I have decided to beer travel to Grand Rapids. Which breweries I must visit?

    beers[beers['city'].str.contains('Grand Rapids')].drop( ['ounces', 'state', 'city'], axis=1)

<table>
   <thead>
      <tr>
         <th></th>
         <th>abv</th>
         <th>ibu</th>
         <th>beer</th>
         <th>style</th>
         <th>brewery</th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>388</td>
         <td>6.7</td>
         <td>0.0</td>
         <td>Belfort</td>
         <td>Saison / Farmhouse Ale</td>
         <td>Brewery Vivant</td>
      </tr>
      <tr>
         <td>389</td>
         <td>6.0</td>
         <td>0.0</td>
         <td>Star Runner</td>
         <td>Belgian Pale Ale</td>
         <td>Brewery Vivant</td>
      </tr>
      <tr>
         <td>390</td>
         <td>9.8</td>
         <td>0.0</td>
         <td>Tart Side of the Barrel</td>
         <td>American Double / Imperial Stout</td>
         <td>Brewery Vivant</td>
      </tr>
      <tr>
         <td>391</td>
         <td>6.0</td>
         <td>0.0</td>
         <td>Linnaeus Mango IPA</td>
         <td>American IPA</td>
         <td>Brewery Vivant</td>
      </tr>
      <tr>
         <td>...</td>
         <td>...</td>
         <td>...</td>
         <td>...</td>
         <td>...</td>
         <td>...</td>
      </tr>
   </tbody>
</table>

66 rows × 5 columns

I hope, you understand style in which I have represented analysis. Firstly understand dataset, each column of it. If you have not already "googled" what is **abv** and **ibu** please search it now. Then define your questions to dataset. Finally translate your questions to Pandas and get results.
