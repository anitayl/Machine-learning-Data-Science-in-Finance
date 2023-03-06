# Machine-learning-Data Science in Finance
## Goal
This is a Wharton capstone project that cooperates with Estimize.We use their stock market consensus data, comparing with Wall Street Forecasts to build a portfolio strategy to see whether we could generate abnormal returns. 
## Introduction
Estimize is a data company that owns a platform for all its users to make daily forecasts of certain indicators in the finance and economic market, varying from listed companies’ quarterly revenue to countries’ economic indicators. It covers many areas, but in our research, we only focus on the EPS estimation. Unlike Wall Street Consensus forecasts, which are made only by sell-side analysts, Estimize EPS consensus forecasts have contributors from sell-side and buy-side, as well as from non-professionals. Since it covers broader contributors, we are assuming Estimize data have more predictive power to reflect the true market expectation. Therefore, we want to research whether Estimize EPS forecasts could provide abnormal returns compared with Wall Street consensus, taking into consideration the sectorial & size effects.
## Data Source
Our primary dataset is Estimize Consensus Equity data, which contains 6,264k data of 10 sectors, 66 industries and 2738 companies from 2011 to 2022. We also use Wharton Research Data Services (WRDS) as our alternative dataset. Our variables include Estimize Consensus EPS forecast, number of contributors, Wall Street EPS forecast, daily return, sector and market cap. 
The file final_df in the dataset is the final version of our data we used for our further model building, after cleaning and merging through our raw data.
## Exploratory Data Analysis of Estimize Data
Our statistics show only a small fraction of the estimates have contributors larger than 20
<img width="441" alt="EDA1" src="https://user-images.githubusercontent.com/102770592/223012263-3a88ad4f-a7a4-4f38-abe3-6eef27b6c657.png">

Our statistics show Estimize eps performance varies by sector

<img width="443" alt="EDA2" src="https://user-images.githubusercontent.com/102770592/223012565-2046b00c-546f-4c31-9e6f-dc0c5085d143.png">

## Strategy
We built a regression model to do our analysis by regressing the 5-day compound return with EPS differentiation, sector, number of contributors and market cap. To avoid look-ahead bias, we made predictions by rolling out samples. Our strategy is to make predictions of the stocks which will make their announcement in four days(day 4) and build a portfolio of the top-5 stocks with highest predicted returns and trade them between day 1 and day 5. We then use the equal-weighted actual compound returns of this portfolio to compare with our benchmark S&P 500 to see by how much our strategy outperforms the market.
<img width="468" alt="strategy" src="https://user-images.githubusercontent.com/102770592/223012597-83d9a8a0-9f50-4a77-b692-d51003740ced.png">
## Conclusion
After running our regression model and performing our strategy using the model, three conclusions are drawn. Firstly, EPS difference can provide enough information to achieve abnormal returns. Secondly, Estimize data is more accurate for certain sectors and with more contributors. Thirdly, large market caps contribute more prediction power to returns.
<img width="468" alt="EDA3" src="https://user-images.githubusercontent.com/102770592/223012580-bc578d5b-2b11-438a-865b-8e30a7f84f42.png">
(Dark Blue) Our best portfolio cumulative returns after tuning top x tickers, which x variables to include/exclude, trading pool having at least x tickers, selecting positive predicted returns only; (Light Blue) Intermediate strategy portfolio returns; (Red) Cumulative market returns.
<img width="363" alt="results" src="https://user-images.githubusercontent.com/102770592/223012595-f5e9633e-819c-4173-a3fb-ea9dff4d7207.png">
