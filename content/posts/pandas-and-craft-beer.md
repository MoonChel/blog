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

| Unnamed: 0 | abv | ibu | id | name | style | brewery_id | ounces |  
| 0 | 0 | 0.050 | NaN | 1436 | Pub Beer | American Pale Lager | 408 | 12.0 |  
| 1 | 1 | 0.066 | NaN | 2265 | Devil's Cup | American Pale Ale (APA) | 177 | 12.0 |  
| 2 | 2 | 0.071 | NaN | 2264 | Rise of the Phoenix | American IPA | 177 | 12.0 |  

This table contains some evident problems, such as NaN values in **ibu** column and useless columns **Unnamed: 0** and **id**. Firstly we can remove useless column in beers table with:

    beers.drop('Unnamed: 0', axis=1, inplace=True) 

In breweries table we will just renamed this column, because of future use in tables merge on **brewery_id** column.

    breweries.rename(columns={'Unnamed: 0' : 'brewery_id'}, inplace=True) 
    beers.drop('id', axis=1, inplace=True) 
    beers.head()

|  | abv | ibu | name | style | brewery_id | ounces |
| 0 | 0.050 | NaN | Pub Beer | American Pale Lager | 408 | 12.0 |
| 1 | 0.066 | NaN | Devil's Cup | American Pale Ale (APA) | 177 | 12.0 |
| 2 | 0.071 | NaN | Rise of the Phoenix | American IPA | 177 | 12.0 |

    breweries.head() 

|  | brewery_id | name | city | state |
| 0 | 0 | NorthGate Brewing | Minneapolis | MN |
| 1 | 1 | Against the Grain Brewery | Louisville | KY |
| 2 | 2 | Jack's Abby Craft Lagers | Framingham | MA |

Now, when we have column in beers table **brewery_id**, which is simple row index in breweries table, we can merge two tables.

    beers = beers.merge(breweries, on='brewery_id') 
    beers.head()

|  | abv | ibu | name_x | style | brewery_id | ounces | name_y | city | state |
| 0 | 0.050 | NaN | Pub Beer | American Pale Lager | 408 | 12.0 | 10 Barrel Brewing Company | Bend | OR |
| 1 | 0.066 | NaN | Devil's Cup | American Pale Ale (APA) | 177 | 12.0 | 18th Street Brewery | Gary | IN |
| 2 | 0.071 | NaN | Rise of the Phoenix | American IPA | 177 | 12.0 | 18th Street Brewery | Gary | IN |

It's 99% prepared for analysis, and last things can be describe as cosmetic correction. We will replace all **NaN** with **0.0** values, and after it multiple **abv** column with 100 for increasing human readability.

    beers['abv'] *= 100 beers['ibu'].fillna(0.0, inplace=True) 
    beers['abv'].fillna(0.0, inplace=True) 
    beers.head()

|  | abv | ibu | name_x | style | brewery_id | ounces | name_y | city | state |  
| 0 | 5.0 | 0.0 | Pub Beer | American Pale Lager | 408 | 12.0 | 10 Barrel Brewing Company | Bend | OR  | 1 | 6.6 | 0.0 | Devil's Cup | American Pale Ale (APA) | 177 | 12.0 | 18th Street Brewery | Gary | IN |  
| 2 | 7.1 | 0.0 | Rise of the Phoenix | American IPA | 177 | 12.0 | 18th Street Brewery | Gary | IN |  

Ups, I forgot about **name_x** and **name_y**. When we have merged two tables, columns with same names were changed to make them unique.

    beers.rename(columns={'name_x' : 'beer', 'name_y' : 'brewery'}, inplace=True) 
    beers.drop('brewery_id', axis=1, inplace=True) 
    beers.head()

|  | abv | ibu | beer | style | ounces | brewery | city | state |  
| 0 | 5.0 | 0.0 | Pub Beer | American Pale Lager | 12.0 | 10 Barrel Brewing Company | Bend | OR |  
| 1 | 6.6 | 0.0 | Devil's Cup | American Pale Ale (APA) | 12.0 | 18th Street Brewery | Gary | IN |  
| 2 | 7.1 | 0.0 | Rise of the Phoenix | American IPA | 12.0 | 18th Street Brewery | Gary | IN |  

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

|  | abv | state |  
| 2278 | 12.8 | CO |  
| 70 | 12.5 | KY |  
| 2189 | 12.0 | IN |  
| 2279 | 10.4 | CO |  
| 1857 | 10.0 | NY |

People usually understand statistical data better if they are represented in graphs. Pandas can help with it.

## Show me graph with sorted states by mean abv in each state.

    beers.groupby('state')['abv'].mean().sort_values( ascending=False).plot(kind='bar', ylim=(5,7), colormap='summer') 

![](https://res.cloudinary.com/ducpttwlw/image/upload/v1613839393/blog/Untitled_13_1_e5nvef.png)

Cool, here we can see one interesting thing. Even though **DC** is first in mean **abv**, **CO** has beer with highest **abv** among all states and I want to find which brewery craft it.

### REMARK - you can not just use beers\[beers\['state'\] == 'CO'\] if you didn't strip all values with str.strip

    beers[beers['state'].str.contains('CO')].sort_values(by='abv', ascending=False).head()

|  | abv | ibu | beer | style | ounces | brewery | city | state |  
| 2278 | 12.8 | 0.0 | Lee Hill Series Vol. 5 - Belgian Style Quadrup... | Quadrupel (Quad) | 19.2 | Upslope Brewing Company | Boulder | CO |  
| 2279 | 10.4 | 0.0 | Lee Hill Series Vol. 4 - Manhattan Style Rye Ale | Rye Beer | 19.2 | Upslope Brewing Company | Boulder | CO |  
| 1586 | 9.9 | 98.0 | Ten Fidy Imperial Stout (2007) | Russian Imperial Stout | 12.0 | Oskar Blues Brewery | Lyons | CO | | 1578 | 9.9 | 98.0 | Ten Fidy Imperial Stout | Russian Imperial Stout | 12.0 | Oskar Blues Brewery | Longmont | CO |  
| 2286 | 9.9 | 90.0 | Upslope Imperial India Pale Ale | American Double / Imperial IPA | 19.2 | Upslope Brewing Company | Boulder | CO |

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

|  | abv | ibu | beer | style | brewery |  
| 388 | 6.7 | 0.0 | Belfort | Saison / Farmhouse Ale | Brewery Vivant |  
| 389 | 6.0 | 0.0 | Star Runner | Belgian Pale Ale | Brewery Vivant |  
| 390 | 9.8 | 0.0 | Tart Side of the Barrel | American Double / Imperial Stout | Brewery Vivant |  
| 391 | 6.0 | 0.0 | Linnaeus Mango IPA | American IPA | Brewery Vivant |  
| ... | ... | ... | ... | ... | ... |

66 rows × 5 columns

I hope, you understand style in which I have represented analysis. Firstly understand dataset, each column of it. If you have not already "googled" what is **abv** and **ibu** please search it now. Then define your questions to dataset. Finally translate your questions to Pandas and get results.
