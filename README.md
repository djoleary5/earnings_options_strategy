# earnings_options_strategy

Earnings announcements frequently cause a volitile reaction in the price of the stock of the announcing company.  Following the announcement, the stock price can rise or fall significantly.  A long straddle (buying both call and put for the same stock, strike price, and expiration) is an options strategy profits from volitility without knowing in which direction the price will move.  This means however, one leg of the straddle will expire worthless.  The hope is that the price will move far enough in one direction to offset the worthless contract and still be profitable.

This is an analysis of stock price performance on the trading day following an earnings announcement.  The percent price change will be calculated for all S&P 500 companies, for the last five quarters and look for companies who's stock are the most volitile following those earnings announcements.

## Notebooks
### ehlc_scrape_quarter
Getting the earnings dates:
* My initial idea was to start with a list of all S&P companies and go one by one colle.  I assumed that a site like Yahoo Finance, which has a page for every public company with extensive information about the stock, would have the dates of the company's earnings announcements.  Unfortunately, this is not the case.  I searched numerous sites, including the website of the brokerage with whom I have my account.  The information was available through APIs, but all of these charge a fee for earnings data.
* Yahoo finance does have a daily earnings calander that goes back for years [Yahoo Finance Earnings Calender](https://finance.yahoo.com/calendar/earnings/).  Each trading day has a page with a well organized table of all the companies that announced earnings that day that includes the `symbol`, `company`, `earnings call time` (before the open or after the close), and details about the estimated and actual EPS.  My plan is to go day by day and scrape all the companies that announced earnings each day along with the time they announced.
* So instead of a list of stocks, we need a list of dates.  The format needed is YYYY-MM-DD and the date range to be used is 2019-01-01 throught 2020-03-30.  Going quarter by quarter, Yahoo Finance Earnings Calender is scraped for each day and a dataframe `combined_df` is created and sorted by `Symbol`.  This can be considered raw data and is written to a `YYYYQQ_raw_earnings.csv`.
* The next step is to filter this list for S&P 500 companies.  We scrape [Wikipedia](https://en.wikipedia.org/wiki/List_of_S%26P_500_companies) for a list of the S&P 500 symbols and fitler `combined_df` for these symbols and create a data frame `sp_earnings` with columns: `Symbol`, `YYYYQQ Time`, `YYYYQQ Date` and write it to a `YYYYQQ_sp_earnings.csv` file.  If there is no earnings data for a symbol during the quarter, then the value will display `NaN`.  If the same symbol shows up more than once in the same quarter, then the value will display `Multi` and the symbol and dates will be added to a data frame `multiples` and written to a .csv file.
### els_ohlc
Getting the quotes:
* We start by creating a dataframe by importing a quarterly `sp_earnings.csv`.  
* I defined a function `ohlc(sym, earn_date)`, which, given a stock symbol and date will return the opening, high for the day, low for the day, and closing prices for that stock on that day.  Using the list of S&P 500 symbols from our `sp_earnings` dataframe, we cycle through each symbol-date pair and get the OHLC for the earnings date along with the previous day and next day.  (For most symbols it is not specified whether the earnings were announced.)
* A dataframe `dp_df` is created from this data and we create two additional columns `Before Open %Change` (the percent change from the previous day's close to the earnings day's open) and `After Close %Change` (the percent change from the earnings day's close to the next day's open). This dataframe is written to our `YYYYQQ_sp_earnings.csv` file.
### els_changes
Brining it all together:
* We then read the %Change data from each of our quarterly `YYYYQQ_sp_earnings.csv` files and create a final dataframe `change_df` and write the it to `all_qtr_changes.csv` so we can use Excel to look for the stocks with the most volitile earnings history.




