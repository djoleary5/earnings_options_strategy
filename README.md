# earnings_options_strategy

Earnings announcements frequently cause a volitile reaction in the price of the stock of the announcing company.  Following the announcement, the stock price can rise or fall significantly.  A long straddle (buying both call and put for the same stock, strike price, and expiration) is an options strategy profits from volitility without knowing in which direction the price will move.  This means however, one leg of the straddle will expire worthless.  The hope is that the price will move far enough in one direction to offset the worthless contract and still be profitable.

This is an analysis of stock price performance on the trading day following an earnings announcement.  The percent price change will be calculated for all S&P 500 companies, for the last five quarters and look for companies who's stock are the most volitile following earnings.

## Steps
### Scraping Yahoo Finance Earnings Calander
* My initial idea was to start with a list of companies.  I assumed that a site like Yahoo Finance, which has a page for every public company with extensive information about the stock, would have the dates of the company's earnings announcements.  Unfortunately, this is not the case.  I searched numerous sites, including the website of the brokerage with whom I have my account.  The information was available through APIs, but all of these charge a fee for earnings data.
* Yahoo finance does have a daily earnings calander that goes back for years [Yahoo Finance Earnings Calender](https://finance.yahoo.com/calendar/earnings/).  Each trading day has a page with a well organized table of all the companies that announced earnings that day that includes the `symbol`, `company`, `earnings call time` (before the open or after the close), and details about the estimated and actual EPS.  My plan is to go day by day and scrape all the companies that announced earnings each day along with the time they announced.
* The first step is to create a list of dates.  The format needed is YYYY-MM-DD and the date range to be used is 2019-01-01 throught 2020-03-30.  Yahoo Finances Earnings Calender is scraped for each day and a data frame `combined_df` is created and sorted by `Symbol`.  This can be considered raw data.

#Cleaning sorted data
PGR, symbols with no dates
