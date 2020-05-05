# Evaluating Stock Market Trends through the Bloomberg Terminal: An Analysis of Historical EPS Forecast Accuracy for All Firms in the 2019 S&P 500 Index

## This project was conducted as my undergraduate senior thesis. 

## Motivation


For this project, I gathered four datasets relating to the stock market through using Bloomberg Excel Functions. I assessed the quality of the data, cleaned it and rearranged it, and stored the data in clean CSVs. I posed my own research questions, explored the data, and reported my findings through a final thesis report and Jupyter slides.

## Introduction


This project was divided into 3 stages: 
1) **data wrangling**
2) **data exploration**
3) **data explanatory**

The IPython Notebook files, `data_cleaning.ipynb`, `data_exploratory.ipynb`, and `data_explanatory.ipynb` all document the above 3 stages respectively. The first 2 files have been converted to HTML for convenience.

### To view my Jupyter Notebook slides, clone the repository and navigate to the terminal. Follow these steps:

1) Navigate to `/Thesis/`
2) type `jupyter nbconvert pisa2012_explanatory.ipynb --to slides --template output_toggle.tpl --post serve`

**Broad question:** How do price forecasts for each firm in the S&P 2019 Index compare to their corresponding actual prices? 

> In order to address this broad question, I narrowed my focus down these factors: Forecasted Earnings-per-Share (EPS), Actual EPS, Actual End-of-Day (EOD) Price, and Forecasted EPS 3-months prior to that fiscal term. Since Earnings-per-Share directly determines a company's worth, I figured this would be a principle feature to use in measuring company value over the past 20 years. I received all of my data through the Bloomberg Terminals and their Excel functions, most especially the Bloomberg Help Desk.

Here is a breakdown of my 4 factors, their explanations, and how they contribute to the broad question:

- **Forecasted EPS:** this is will be subtracted from actual EPS to measure prediction errors of forecasts. This is also a central value to measure for the question of comparing historical forecasting accuracy of various firms. Forecasts are made based on a firm's ***fiscal period.***

> BDP("AAPL UW Equity","BEST_EPS","BEST_FPERIOD_OVERRIDE = 00Q1")

- **Actual EPS:** This is also measured based on ***fiscal period.***

> BDP("AAPL UW Equity","IS_EPS",  "FUND_PER=Q1", "EQY_FUND_YEAR=2000")

- **Actual EOD Price:** the dates used for this measure correspond to the start of each ***calendar period,*** so it will be used to generate interesting visualizations and expand my analysis.

> BDH("A UN Equity", "BEST_TARGET_PRICE", "03/31/1999", "03/31/2019")

- **Forecasted EPS 3 months prior:** EPS forecasts are made at the beginning of each ***fiscal period.*** This feature contains EPS forecasts made 3-months period to the current fiscal period. This will be an interesting metric to compare to forecasts at differing dates for the same firm.

> BDH("AAPL US Equity", "BEST_EPS", "7/1/2007", "BEST_FPERIOD_OVERRIDE=08Q1", "days=a", "fill=p")

Initially, I intended to pick every firm present in the S&P 2019 Index for the past 20 years. However, the problem is that ***firms leave and join the S&P Index at unpredictable times,*** so analyzing EPS forecasts solely based on index alone instead of firms would be a highly misdirected approach. 

Of course I could keep track of every firm that left and entered the S&P 500 index for the last 20 years. But due to the scope of this project, such an investigation was infeasible. I elaborate more on this under the **Future Applications** section.

It was more important to keep the same companies consistent across all datasets. To keep true to this approach, I picked the S&P 2019 firms as of November 2019. 

### Focus 

For the rest of the project, I break down the broad question into these focuses:

- *compare both forecast and actual EPS for each firm in the S&P 2019.*
- *focus only and only on the firms present in the S&P 2019 Index.*
- *gather data projected from the past 20 years.*


### Data Overview

Here is a breakdown of the features among the final clean CSVs:

**features.csv**
- *firm_id*
- *feature*
- *date*
- *term*
- *value*

**avgs.csv**
- *firm_id*
- *average*
- *average_type*
- *time_period*
- *feature*

**firms.csv**
- *firm_id*
- *firm*



## Research Questions

**Question 1:** Does average EPS prediction error depict any differences in trends whether by a yearly, quarterly, or full-term basis?

1. Forecasters were most **optimistic** in 2008Q4, and most **pessimistic** in 2000Q4.

2. The **later the quarter, the more optimistic** forecasters become in their average quarterly EPS predictions.

3. The year 2000 contains the highest variance among average prediction errors, ranging from -0.25 to -1.5.

4. ***The trends depicting EPS prediction error by quarter and year, separately, is consistent.*** All average EPS forecasts gather around 0 per year from 1999 - 2000, when ignoring outliers.

**Question 2:** I generate “dumb EPS forecasts” by calculating the rolling mean of actual EPS from the past 2 quarters. How do my forecasted EPS forecasts compare to Bloomberg forecasts? 

1. My average predicted EPS ***more closely follows the average actual EPS*** trend instead of the Bloomberg forecasters'.

2. This is a key takeaway: my method of using 2-quarter moving average to predict EPS was ***much more effective*** than the method used by Bloomberg forecasters.

3. My personal forecasts "spiked" and "troughed" in the terms 2000Q4 and 2008Q4—the ***exact same pattern*** as Bloomberg EPS forecasts.

4. All of my EPS predictions contain higher variance than actual EPS. This means my method is less credible once accounting for all individual data points.


**Question 3:** What differences and similarities emerge when analyzing the prediction error and percentage error of EPS forecasts?

1. Forecasters, on average, are likely to be more inaccurate in their predictions in Q4 of any given year.

2. For percentage errors, EPS forecasts percentage errors have become more ***inaccurate*** in the more recent terms, starting from ***2014Q1.***

3. The top 5 most inaccurate firm tickers for absolute ***prediction error*** are AGN, AIG, CHTR, LCRX, and VRSN. The most notable outlier is AIG, this is the *only outlier* on average for any given year.

4. The top 5 most inaccurate firm tickers for absolute ***prediction error*** are IBM, IRM, MCK, PXD, and QRVO. The most notable outlier is IRM, with a percentage error of over -1000 for EPS in the term 2014Q3.

**Question 4:** Does statistical significance stay true among the relationships between actual EPS and forecasted EPS, regardless of forecasted type (raw data along with all yearly, quarterly, and twenty-year averages)?

1. Short answer: **Yes.** All p-values are recorded at 0.00, which is less than the Type I error threshold of 0.05.

2. All r-values are positive, with values varying ***between 0.3 and 0.8*** for *average forecasted EPS* data. 

3. Actual EPS vs raw forecasted EPS (not averages) depicts a ***weak positive relationship.*** This intuitively makes sense, since I would expect trends to become more unpredictable over longer spans of time, thus giving rise to more outliers.

4. Out of all the regression plots depicting actual vs. forecasted EPS, the ***raw data displays the least amount of variance.*** 

**Question 5:** How do EOD Prices trend across all firms from 1999 - 2019?

1. EOD prices show a general positive trend from 2009, as can be seen with figure (features-eod-term). 

2. EOD prices see a "trough" from 2007 - 2009: the ***years of the Great Recession.***


### Limitations:

**Overall**

1. There was no explicit Bloomberg function to gather forecasted EOD price data by *fiscal period* for each firm in the S&P 2019.

2. There is an incongruence between fiscal periods and calendar periods. Forecasted and actual EPS are recorded by **fiscal period**, while EOD Prices and 3-month-priorly forecasted EPS are recorded by **calendar period.**

3. The dataset for the forecasted EPS 3 months prior was missing the year 1999.


**eps_fc_terms**

1. The year "1999" is missing data for all firms. This means when using this variable, we have to account only for the year 2000 onward.

**eps_act**

1. Contains 7 empty firms: *BRK/B UN Equity, FOX UW Equity, GOOG UW Equity, HCP UN Equity, SYMC UW Equity, UA UN Equity*

**eps_fc_act**

1. Has one firm with empty data: *AMCR UN Equity*

## Future Applications


1. Look at other dates forecasted, not just 3 months before.
2. Improve linear regression models. Investigate multicollinearity to see if any other categorical or numerical value had any direct influence on affecting the generated p-values.
3. Incorporate `eps_fc_terms` into the analysis. Look at other forecasts made from different periods, not just 3 months before. This would give me a greater insight into EPS forecasting trends made at various points in time before the current fiscal period.
4. Refocus my broad question to analyze variable relationships occurring during the **Great Recession.** Expand on how firms in the S&P 500 Index and their EPS and EOD prices fluctuate and responded to the stock market shifts in dynamic as a response to the economic recession.
