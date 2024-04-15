# Module 8 Challenge: prophet-challenge
## Problem Statement

The assignment is to analyse the Google Search Traffic for MercadoLibre, the most popular e-commerce site in Latin America. In particular, the questions are:
1. What are the patterns in the Google Search Traffic?
2. Is there any correlation between financial events of the company, such as the release of its quarterly financial results, and the Google Search Traffic?
3. Is there any correlation between the Google Search Traffic and either the stock price or the stock price volatility?

In addition, we were asked to predict the Google traffic about 80 days into the future.

## Solution

As given in the instructions to the Module 8 challenge, the solution proceeds through the following steps
1. Find Unusual Patterns in Hourly Google Search Traffic.
2. Find regular patterns/seasonality in the Search Traffic data.
3. Correlate the Search Traffic to stock price and stock price volatility.
4. Create a time series model with Prophet.

## Implementation
### Find Unusual Patterns in Hourly Google Search Traffic
After importing and cleaning the hourly Google Search Traffic data, the search traffic data is plotted for May of 2020. May is the month when MercadoLibre releases its quarterly financial results and indeed there is increased serach traffic on May 5 and 6.

In addition, there are regular daily and weekly patterns.

In order to determine whether May 2020 had more search traffic than other months, the total search traffic for May 2020 is compared with the median search traffic across all months in the data. It is found that May 2020 received about 8.5% more search traffic than the median across all months.

### Find regular patterns/seasonality in the Search Traffic data
In order to find seasonality in the data, it is plotted over different timescales. We already noticed a daily and a weekly pattern when plotting the data for the month of May 2020. To get a more detailed understanding of the regular patterns, we proceed as follows.
1. To determine daily patterns, we group the data by hour of the day, calculate the hourly average, and then plot the hourly average by hour of the day.  
   As a result one finds that the traffic is lowest between 7 and 9 AM and largest around midnight. Since we do not know what timezone is used for the data, we do not know whether this pattern makes sense.
2. To determine weekly patterns, we group the data by day of week, calculate the average for each day of the week, and plot it by day of the week.  
   As a result one finds that the traffic is highest on Tuesday and lowest on Sunday.
3. To determine yearly patterns, we group the data by week of the year, calculate the average for each week opf the year, and plot it by week of the year.  
   As a result one finds that the lowest traffic occurs in week 35 and weeks 52 and 1 of the year. The highest traffic occurs in week 4 and week 8 of the year.
   Since the day of the year is rather noisy, we also decided to group the data by month, calculate the monthly average, and plot it.
   Here one finds that February is the month with the highest traffic volume and October is the month with the lowest traffic volume.

### Correlate the Search Traffic to stock price and stock price volatility
After reading the stock price data we plot it over time. Generally, the stock price is going up over the timeperiod of the data, with a fairly sharp drop off a little into 2020. This drop off might correlate with the onset of the Covid19 epidemic.

To get a better understanding of whether there is a correlation between the Google search traffic and the stock price close during the first half of 2020, we combine the trend and close data into one dataframe and slice it to January 2020 through June 2020. When we visually compare the plot of the closing price with the plot of the trend, we do not find a conclusive correlation. The closing price declines from March through May of 2020 and there might be a corresponding slight decrease in the trend from April through May of 2020. However, it is not conclusive.

To obtain a more quantitative understanding of any correlation between the Google search traffic, the stock price closing and the stock price volatility, we add three columns to the dataframe that contains the stock price close and the Google search traffice trend.
1. A `Lagged Search Trends` column  
   The lag is one hour on the understanding that any change in stock price as a result of a change in search traffic will show up a little after a change in search traffic occurred.
2. A `Stock Volatility` column  
   It contains the standard deviation of the average stock price over the preceding four hours.
3. A `Hourly Stock Return` column  
   It lists the percentage change of the stock cloisng price from one hour to the next.

After plotting the stock volatility and noticing a larger volatility in 2020 as compared to 2016, we use the Pandas `corr` function to generate a correlation matrix between the stock volatility, the lagged search trends, and the hourly stock return. There is no strong correlation between the stock volatility, the lagged search trends, and the hourly stock return. There might be a very weak correlation between the lagged search trends and the stock volatility. However, it is so week that it is hard to distinguish it from statistical noise with the available data.

### Create a time series model with Prophet
The model accurately reproduces the seasonal patterns observed in the data. More specifically, it reproduces
1. That the hours of the day with the largest traffic are around midnight and the hours of the lowest traffic are between 7AM - 9AM.
2. That the day of the week with the most traffic is Tuesday and the day with the least traffic is Sunday.
3. That October followed by end of year/beginning of new year is the time of year with the least traffic and January and February are the months with the most traffic. It also fairly accurately reproduces the other ups and downs in Google search traffic throughout the year.

Using the model to forecast Google search traffic up to two months out does not lead to a prediction that differs much from the historical patterns. There might be a slight decrease in the trends of the prediction but it is difficult to say with certainty that it is not just statistical noise.