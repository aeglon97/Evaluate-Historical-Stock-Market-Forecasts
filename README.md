# Stock Market Antics: Evaluating Forecast Accuracy for the 2019 S&P 500 Index

---
 
## Motivation

---

For this project I gathered four datasets relating to the stock market through using Bloomberg Excel Functions. I assessed the quality of the data, cleaned it and rearranged it, and stored the data in clean CSVs. I posed my own research questions, explored the data, and reported my findings through a final thesis report and Jupyter slides.

[TK] here are the equations I plugged into Excel to get the data I need:

## Introduction

---

**Broad question:** How do price forecasts for each firm in the S&P 2019 Index compare to their corresponding actual prices? 

> In order to address this broad question, I narrowed my focus down these factors: Forecasted Earnings-per-Share (EPS), Actual EPS, Actual End-of-Day (EOD) Price, and Forecasted EPS 3-months prior to that fiscal term. Since Earnings-per-Share directly determines a company's worth, I figured this would be a principle feature to use in measuring company value over the past 20 years. I received all of my data through the Bloomberg Terminals and their Excel functions, most especially the Bloomberg Help Desk.

Here is a breakdown of my 4 factors, their explanations, and how they contribute to the broad question:

- **Forecasted EPS:** this is a central value to measure for the question of comparing historical forecasting accuracy of various firms. Forecasts are made based on a firm's ***fiscal period.***

> BDP("AAPL UW Equity","BEST_EPS","BEST_FPERIOD_OVERRIDE = 00Q1")

- **Actual EPS:** this will be subtracted from the forecasted EPS to measure how far off the forecasted and actual EPS values are. This is also measured based on ***fiscal period.***

> BDP("AAPL UW Equity","IS_EPS",  "FUND_PER=Q1", "EQY_FUND_YEAR=2000").

- **Actual EOD Price:** the dates used for this measure correspond to the start of each ***calendar period,*** so it will be used to generate interesting visualizations and expand my analysis.

> BDH("A UN Equity", "BEST_TARGET_PRICE", "03/31/1999", "03/31/2019")

- **Forecasted EPS 3 months prior:** EPS forecasts are made at the beginning of each ***fiscal period.*** This feature contains EPS forecasts made 3-months period to the current fiscal period. This will be an interesting metric to compare to forecasts at differing dates for the same firm.

> BDH("AAPL US Equity", "BEST_EPS", "7/1/2007", "BEST_FPERIOD_OVERRIDE=08Q1", "days=a", "fill=p")

> Initially, I intended to pick every firm present in the S&P 2019 Index for the past 20 years. However, the problem is that ***firms leave and join the S&P Index at unpredictable times,*** so analyzing EPS forecasts solely based on index alone instead of firms would be a highly misdirected approach. 

> Of course I could keep track of every firm that left and entered the S&P 500 index for the last 20 years. But due to the scope of this project, such an investigation was infeasible. I elaborate more on this under the **Future Applications** section.

> It was more important to keep the same companies consistent across all datasets. To keep true to this approach, I picked the S&P 2019 firms as of November 2019. 

### Focus 

For the rest of the project, I break down the broad question into these focuses:

- *compare both forecast and actual EPS for each firm in the S&P 2019.*
- *focus only and only on the firms present in the S&P 2019 Index.*
- *gather data projected from the past 20 years.*

---

## Research Questions

---

**Question 1:** What is the difference in means between average forecasted and actual EPS of each firm for the past 20 years?

**Question 2:** What is the relationship between forecasted EPS for both forecasts made at the beginning of the fiscal period ***and*** three months prior? 

**Question 3:** Does historical EOD price correlate with forecasted EPS? How about actual EPS?

**Question 4:** For both the highest-performing and lowest-performing companies, do EPS forecasts show a pessimistic and/or optimistic view of their company value?


## Findings

--- 

### Results:

### Limitations:

**Overall**

1. There was no explicit way to gather forecasted EOD price data for each firm in the S&P 2019, on top of calculating their quarterly averages. 

2. There is an incongruency between fiscal periods and calendar periods. 

3. The dataset for the forecasted EPS 3 months prior was missing the year 1999.

4. For the datasets of actual EPS and forecasted EPS 3-months prior, the firm Amcor PLC

**eps_fc_terms**

1. The year "1999" is missing data for all firms. This means when using this variable, we have to account only for the year 2000 onward.

**eps_act**

1. Contains 7 empty firms: *BRK/B UN Equity, FOX UW Equity, GOOG UW Equity, HCP UN Equity, SYMC UW Equity, UA UN Equity*

**eps_fc_act**

1. Has one firm with empty data: *AMCR UN Equity*

## Future Applications

--- 

1. Look at other dates forecasted, not just 3 months before