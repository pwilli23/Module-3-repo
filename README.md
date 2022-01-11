# Bitstamp and Coinbase Arbitrage Opportunities

In this project I analyzed the arbitrage opportunities that existed between the prices for Bitcoin on the Bitstamp and Coinbase exchanges.  Arbitrage is the simulataneous purchase and sale of the same asset in different markets to make a profit.  I was able to do this through a 3 step process: **
1. Collecting the data 
2. Preparing the data 
3. Analyze the data
---
# **Part One: Collect the Data**
### To collect the data properly, I had to do the following:

### Step 1: Use the Pandas `read_csv` function and the `Path` module to import the data from `bitstamp.csv` and the `coinbase.csv` file while also setting the Datetimeindex as the Timestamp column:

`bitstamp = bitstamp_dataframe = pd.read_csv(
Path('~/Desktop/Starter_Code_5/Resources/bitstamp.csv'),
index_col= "Timestamp",
parse_dates= True,
infer_datetime_format=True
)`

### Step 2: Used the `tail` function to confirm that the import was successful for both csv files:

`bitstamp.tail()`

`coinbase.tail()`

### Now that the Coinbase and Bitstamp csv files have been successfully imported, the data can now be prepared 

---
# **Part Two: Prepare the Data**

### Step 1: Drop all NaN or missing value in the Bitstamp and Coinbase Dataframe:

* First we used the `isnull().sum()` function to check how many missing values there were in each Dataframe:

* Bitstamp had 212 NaN values while Coinbase had 0 NaN values:
* To solve this issue, I used the `.dropna()` function to drop all of the missing values in the **Bitstamp Closing price Dataframe**.  I did not have to use the `.dropna()` function for **Coinbase** since there were not any NaN values in its Dataframe
* I also did not use the `drop_duplicates` function because the Timestamp index is down to the minute so it is very possible that Bitcoin can trade at the same price just minutes apart.  

### Step 2: Use the str.replace function to remove the $ signs from the Closing Price Column:

`bitstamp.loc[:, 'Close'] = bitstamp.loc[:, 'Close'].str.replace('$','')`
* Using the `.loc` function I was able to focus on the Closing price column in the Dataframe.  
* I then used the `str.replace` function to remove the money signs

### Step 3: Convert the data type of the Close column to a float:

`bitstamp.loc[:,'Close']= bitstamp.loc[:, 'Close'].astype('float')
bitstamp`
* To convert the data type to float value I used the  `astype('float')` function 

---
# **Analyze the Data**

### Step 1: Choose the column of data to focus on and Generate Summary Statistics:

* Since we want to focus our analysis on the difference between the exchange's Closing prices I used the following code to select the Timestamp and Closing price columns:**  

`bitstamp_sliced= bitstamp.loc[:,'Close']` and  `coinbase_sliced = coinbase.loc[:, 'Close']` 

*  Get the Summary statistics and plot the data:

To describe the data I used the `.describe()` function for both `bitstamp_sliced` and `coinbase_sliced`

### Step 2:  Plot the data on two separate graphs using the following code: 

` bitstamp_sliced.plot(figsize=(10,5), title= "Bitstamp Closing Price", color= "blue")`

` coinbase_sliced.plot(figsize=(10,5), title= "Coinbase Closing Price", color= "red")`

### Step 3: Overlay the two Dataframes onto the same graph:

* Using the following code I created the visual:

` bitstamp_sliced.plot(legend=True, figsize=(15,10), title="Bitstamp vs Coinbase Price", color="blue", label="Bitstamp")
coinbase_sliced.plot(legend=True, figsize=(15,10), color="orange", label="Coinbase")`
* Using the `.plot()` function I was able to create a legend,  title, label, and color code the two Dataframes on the graph

**Plot the price action for 2 different 1 month time periods to overlay and plot on the same graph:**

**Using the following code I created the visual:**

`bitstamp_sliced.loc['2018-01-01' : '2018-02-01'].plot(
legend=True, figsize=(15,10), title='January 2018', color="blue", label="Bitstamp")
coinbase_sliced.loc['2018-01-01' : '2018-02-01'].plot(
legend=True, figsize=(15,10), color="orange", label= "Coinbase")`
* I used the `.loc[]` function to select the first month time period from the Dataframe. 
* I repeated the same process for the month of March to see the differences in Bitcoin price spread between the two exchanges
### Step 3: Focus the Anaysis on specific dates to evaluate arbitrage profitability
* The three dates I selected are: **January 5th, Febuary 23rd, and March 30th, 2018.**  I plotted the price action using the following code:

`bitstamp_sliced.loc['2018-01-04'].plot(
legend=True, figsize=(15,10), title="January 5th, 2018", color="blue", label="Bitstamp")
coinbase_sliced.loc['2018-01-04'].plot(
legend=True, figsize=(15,10), color="orange", label="Coinbase")`

* After plotting the two Dataframes over a one day time period, I did an arbitrage analysis for a one day time period between the two exchanges:

`arbitrage_spread_early = coinbase_sliced.loc['2018-01-04'] - bitstamp_sliced.loc['2018-01-04']`

* We then derived the summmary statistics from the arbitrage spread by using the `describe()` function
* After finding the arbitrage spread for each date, I created a box plot to visually show the arbitrage spread for each date:
`arbitrage_spread_early.plot(kind='box')`

---
## Calculate the Arbitrage Profits:

### Step 1: For each of the 3 dates measure the arbitrage spread between the two exchanges by subtracting the lower priced exchange from the upper priced exchange:
* Since I already calculated arbitage spread for each date I was able to look at the mean value from the summary statistics to determine which exchange on average had the higher price and which one had the lower price.  This statistic determined the upper and lower price exchange. 
* After calculating the arbitrage spread, we used the following conditional statement to show only positive spread values: `arbitrage_return_early = arbitrage_return_early[arbitrage_return_early> 0]` 




















