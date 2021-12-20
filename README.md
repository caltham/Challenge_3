# Challenge_3

**The goal of this project is to capitalize on simultaneous price dislocations by using the powers of Pandas. This project will sort through historical trade data for Bitcoin on two exchanges: Bitstamp and Coinbase.**

## Technologies

<img width="225" alt="Imports" src="https://user-images.githubusercontent.com/94569323/146082562-a7ae14aa-0460-4d2d-9f6d-dce58b0bfbc7.png">

**Note(see above):** After importing these required technologies and following the steps below, the program should be capable of functioning properly.

## Installation Guide

### Collect and prepare the data:

**Read in the CSV file called "bitstamp.csv" using the Path module. Set the index to the column "Timestamp", set the parse_dates and infer_datetime_format parameters.**

~~~
csvpath = Path("Resources/bitstamp.csv")
bitstamp = pd.read_csv((csvpath),
    index_col = "Timestamp",
    parse_dates=True,
    infer_datetime_format=True)
~~~

**Read in the CSV file called "coinbase.csv" using the Path module. Set the index to the column "Timestamp", set the parse_dates and infer_datetime_format parameters.**

~~~
csvpath = Path("Resources/coinbase.csv")
coinbase = pd.read_csv((csvpath),
    index_col = "Timestamp",
    parse_dates=True,
    infer_datetime_format=True)
~~~

**Drop all NaN, or missing, values in the DataFrames.**

~~~
bitstamp = bitstamp.dropna()
~~~

~~~
coinbase = coinbase.dropna()
~~~

**Use the str.replace function to remove the dollar signs from the values in the "Close" columns.**

~~~
bitstamp.loc[:, "Close"] = bitstamp.loc[:, "Close"].str.replace("x", "")
~~~
**Note(see above):** "x" is in place of the dollar sign.

~~~
coinbase.loc[:, "Close"] = coinbase.loc[:, "Close"].str.replace("x", "")
~~~
**Note(see above):** "x" is in place of the dollar sign.

**Convert the data type of the Close column to a float.**

~~~
bitstamp.loc[:, "Close"] = bitstamp.loc[:, "Close"].astype("float")
~~~

~~~
coinbase.loc[:, "Close"] = coinbase.loc[:, "Close"].astype("float")
~~~

**Review the data for duplicated values, and drop them if necessary.**

~~~
bitstamp.duplicated()
bitstamp.duplicated().sum()
~~~

~~~
coinbase.duplicated()
coinbase.duplicated().sum()
~~~

## Usage

**After all of the required data has been collected and prepared, the data can be analyzed.**

### Step 1: Choose columns of data on which to focus your analysis.

~~~
bitstamp_sliced = bitstamp.loc[:, ['Close']]
~~~

~~~
boolean_filter = [False, False, False, True, False, False, False]
coinbase_sliced = coinbase.loc[:, boolean_filter]
~~~
**Note(see above):** As you can see above, users have the ability to choose columns of data through different methods.

### Step 2: Get summary statistics.

~~~
bitstamp_sliced.describe()
~~~

~~~
coinbase_sliced.describe()
~~~

#### Plot the data:

~~~
bitstamp_sliced.plot(figsize=(15, 8), title="Bitstamp", color="blue")
~~~
**Note(see above):** Creates a line plot for the bitstamp DataFrame for the full length of time in the dataset.

<img width="719" alt="Plot_1" src="https://user-images.githubusercontent.com/94569323/146127654-f8428ee5-896b-463d-af8c-9bf92d96d6c6.png">

~~~
coinbase_sliced.plot(figsize=(15, 8), title="Coinbase", color="red")
~~~
**Note(see above):** Creates a line plot for the coinbase DataFrame for the full length of time in the dataset.

<img width="710" alt="Plot_2" src="https://user-images.githubusercontent.com/94569323/146127717-a75ef5c9-4f6f-47d4-aa52-556876b87b59.png">

~~~
bitstamp_sliced['Close'].plot(legend=True, figsize=(15, 8), title="Bitstamp v. Coinbase", color="blue", label="Bitstamp")
coinbase_sliced['Close'].plot(legend=True, figsize=(15, 8), color="red", label="Coinbase")
~~~
**Note(see above):** Overlays the visualizations for the bitstamp and coinbase DataFrames in one plot. The plot should visualize the prices over the full lenth of the dataset

<img width="711" alt="Plot_3" src="https://user-images.githubusercontent.com/94569323/146127816-513b7198-0c93-474a-a6f5-13b4fb81f579.png">

~~~
bitstamp_sliced['Close'].loc["2018-01-01" : "2018-02-01"].plot(legend=True, figsize=(15, 8), title="January 2018", color="blue", label="Bitstamp")
coinbase_sliced['Close'].loc["2018-01-01" : "2018-02-01"].plot(legend=True, figsize=(15, 8), color="red", label="Coinbase")
~~~
**Note(see above):** Creates an overlay plot that visualizes the price action of both DataFrames for a one month period. By narrowing the focus of the plot, users have the ability to make a more informed analysis of the data.

<img width="708" alt="Plot_4" src="https://user-images.githubusercontent.com/94569323/146127871-92d89ceb-da5d-4921-b401-4347b654dcca.png">

### Step 3: Focus Your Analysis on Specific Dates.

~~~
bitstamp_sliced['Close'].loc["2018-01-01"].plot(legend=True, figsize=(15, 8), title="January 1, 2018", color="blue", label="Bitstamp")
coinbase_sliced['Close'].loc["2018-01-01"].plot(legend=True, figsize=(15, 8), color="red", label="Coinbase")
~~~
**Note(see above):** Creates an overlay plot that visualizes the price action of both DataFrames for a one day period. By narrowing the focus of the plot, users have the ability to make a more informed analysis of the data.

<img width="709" alt="Plot_5" src="https://user-images.githubusercontent.com/94569323/146127933-a21a995c-5c2b-425b-9978-e3ffa0f14f5b.png">

#### Generate the summary statistics and then create a box plot.

~~~
arbitrage_spread_early =  coinbase_sliced['Close'].loc['2018-01-01'] - bitstamp_sliced['Close'].loc['2018-01-01']
~~~
**Note(see above):** Calculates the arbitrage spread by subtracting the bitstamp lower closing prices from the coinbase higher closing prices.

~~~
arbitrage_spread_early.describe()
~~~
**Note(see above):** Generates summary statistics for the DataFrame.

<img width="232" alt="Arb_1" src="https://user-images.githubusercontent.com/94569323/146128013-6fa69414-53b1-45a0-a2ca-936056707036.png">

~~~
arbitrage_spread_early.plot(kind = "box")
~~~
**Note(see above):** Visualizes the arbitrage spread in a box plot.

<img width="406" alt="Plot_6" src="https://user-images.githubusercontent.com/94569323/146128050-01dcf809-d34c-4263-9c69-c1d7c11e3cee.png">

### Step 4: Calculate the Arbitrage Profits

~~~
arbitrage_spread_early[arbitrage_spread_early > 0].describe()
~~~
**Note(see above):** Uses a conditional statement to generate the summary statistics for the arbitrage_spread DataFrames where the spread is greater than 0.

<img width="234" alt="Arb_2" src="https://user-images.githubusercontent.com/94569323/146128213-cfa5cb82-e07f-4416-8528-45e5b8dd528e.png">

~~~
spread_return_early = arbitrage_spread_early[arbitrage_spread_early > 0] / bitstamp_sliced['Close'].loc['2018-01-01']
~~~
**Note(see above):** Calculates the spread returns by dividing the instances when the arbitrage spread is positive (> 0) by the price of Bitcoin from the exchange you are buying on (the lower-priced exchange).

~~~
profitable_trades_early = spread_return_early[spread_return_early > .01]
~~~
**Note(see above):** Determines the number of times your trades with positive returns exceed the 1% minimum threshold (.01) that you need to cover your costs.

~~~
profit_early = profitable_trades_early * bitstamp_sliced['Close'].loc['2018-01-01']
profit_per_trade_early = profit_early.dropna()
~~~
**Note(see above):** Calculates the potential profit per trade in dollars by multiplying the profitable trades by the cost of the Bitcoin that was purchased.

~~~
profit_per_trade_early.describe()
~~~
**Note(see above):** Generates the summary statistics for the profit per trade DataFrame.

<img width="241" alt="Arb_3" src="https://user-images.githubusercontent.com/94569323/146128973-2b00a958-ec3a-4ff7-9265-20b6ba49cbb9.png">

~~~
profit_per_trade_early.plot(kind = 'hist', figsize = (15, 8))
~~~
**Note(see above):** Plots the results for the profit per trade DataFrame.

<img width="735" alt="Plot_7" src="https://user-images.githubusercontent.com/94569323/146128307-0fe30cce-9e5f-4820-b223-ef5f65440dab.png">

~~~
potential_profits_early = profit_per_trade_early.sum()
~~~
**Note(see above):** Calculates the sum of the potential profits for the profit per trade DataFrame.

~~~
cumulative_profit_early = profit_per_trade_early.cumsum()
~~~
**Note(see above):** Calculates the cumulative profits over time for the profit per trade DataFrame.

~~~
cumulative_profit_early.plot(figsize=(15, 8), title="Bitcoin Profits")
~~~
**Note(see above):** Plots the cumulative sum of profits for the profit per trade DataFrame.

<img width="718" alt="Plot_8" src="https://user-images.githubusercontent.com/94569323/146128343-844e71b6-3e43-4525-83c5-3ed74eb2302b.png">

## Contributors

Chancie Altham:
Developer

## License

MIT License has been added to the project.