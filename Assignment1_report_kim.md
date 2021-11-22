# Assignment 1 - Report
Date: 20.11.2021  
Kim Yukyeong  
You can read this report in [my github repo](https://github.com/saugkim/abo_data2021/blob/main/Assignment1_report_kim.md), in case markdown file viewer not installed in local computer.


## Introduction

Our task(problem) is scraping and collecting meaningful data from website and analyzing and visualizing the data. Target website is blog which contains articles about food with recipes and also about food without recipes. The address is https://www.skinnytaste.com, it currently has 252 pages. Each article of food with recipe has its calory data and if available green, blue and purple smart points and many other information. Task in this assignment is *scrape* first 30 pages of target site and *collect* some features and *save* them for further study and *visualize* them. And finaly show the recommneded foods based on the user inputs.



## Data collection

*what is the data you are collecting and what is your method of choice?*  

Features collected:
   - name of the food
   - image
   - calories
   - green point
   - blue point
   - purple point
   - summary
   - recipe key
&nbsp;
&nbsp;
    
Tools (library) used: 
   - for webscraping:   BeautifulSoup (https://www.crummy.com/software/BeautifulSoup/)
   - for saving data:     csv library


I collected relevant data(features) using **BeautifulSoup** and saved them as a **csv** foramt into file during scraping. The file name of 'food_data.csv' will be created to the current working directory and data will be appended during looping through 30 pages. The saved image data type is string of image link in this work, not numerical pixel data. 


## Data analysis

*how did you visualise the data and how do you interpret the data visualisation?*
   
    
Tools (library) used:   
   - for data handling: Pandas (https://pandas.pydata.org/)
   - for visualization: Matplotlib (https://matplotlib.org/) and pandas


And data is loaded from the file and loaded data is anaylzed with **pandas**. Visualization is done with **Matplotlib** and pandas. Finally based on the user inputs (calories range and green, blue and purple points), searching records with pandas dataframe and showing outputs to the user. There is no extra library for user input, just input() method used.

The collected data has 210 records with relevant features, some of them has missing values in calories and green, blue and purple points features. The records with missing values are removed before visualization. 

**For calories data**, 201 records from 210 is used. The boxplot is used to visualize data distribution, it shows median, 25%, 75%, mean value and also outliers. And there was no other data processing exept removing null values.

**For green, blue, purple points**, 196 records is used to visualization. The boxplot shows those 3 colors smart points distribution. And also using pandas grouping method, the number of post for each point and color is collected and displayed using bar chart.  

processed dataframe from raw dataframe, values are number of posts  

|smart point (index)|green| blue|purple|
|:--:|--:|--:|--:|
|  0|  1|  5|  6|
|  1|  4| 18| 23|
|  2| 17| 25| 28|
|  3| 23| 24| 29|
|  4| 17| 20| 23|
|  5| 29| 24| 23|
|  6| 25| 26| 21|
|  7| 28| 25| 20|
|  8| 20| 13|  9|
|  9| 11| 10| 10|
| 10|  7|  2|  1|
| 11|  9|  3|  2|
| 12|  4|  1|  1|
| 13|  1|  0|  0|

 
**For recipe keys**, the list of recipe key is not numerical data but it is categorical data so we can analyze them as binary data. The unique recipe keys are total 14, so 14 new columns added (or created newly) to the current dataframe. Each column is unique recipe key and value will be True or False, if the a record has that recipe key(the column index) in its recipe keys list value is True(=1), if not False(=0). We have now numerical data, we can do some simple math. In my work, i just summed up for each key, and sorted them in descending order and display as bar chart.  

processed dataframe from raw data, numeric values are number of posts.

|Recipe Key|(InShort)|posts|
|:--|:--|--:|
|Pressure Cooker Recipes |(PC)|   4|
|Under 30 Minutes        |(Q) | 107|
|Paleo                   |(P) |  16|
|Dairy Free              |(DF)|  87|
|Whole 30 Recipes        |(W) |  25|
|Kid Friendly            |(KF)| 120|
|Vegetarian Meals        |(V) |  87|
|Air Fryer               |(AF)|  20|
|Slow Cooker Recipes     |(SC)|   4|
|Keto Recipes            |(K) |  38|
|Meal Prep Recipes       |(MP)|  28|
|Gluten Free             |(GF)| 141|
|Low Carb                |(LC)|  55|
|Freezer Meals           |(FM)|  21|


## Conclusion
*what were the “scientific” bottlenecks? How did you overcome them?*

Most of time was spended in that site (which has a lot of advertizements, which makes my web browser frozen often), to find proper tag and class name to scrape the right features, anything else but time helps. Also getting familiar with scraping tool, beautifulSoup is another issue, overcomed by practice.

I am familiar with pandas and matplotlib so the implementation is not the problem at least coding point of view. But real problem is answering the question of what kind of results I want to display (it needs data preprocessing from raw data). Calories data is easy to handle. But what to do with those smart points 3 different colors and with recipe keys, was real bottlenecks. How to overcome is studying(knowledge of feature like meaning of smart points) and thinking. Once I have something (when I got a picture) in mind, start coding is relatively easy, also plenty of helps found in internet like stackoverflow. My result is a collection of simple charts, finding correlation between recipe keys left behind.

