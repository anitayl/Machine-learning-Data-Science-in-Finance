# Machine-learning-The power of unbiased market forecast
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

We define our regressions model as follows:
y = α+β1x1t + β2x2+β3x3+β4x4
The dependent variable is the excess return for the short-period return around earnings days, from t = 1 to t = 5 where the earnings announcement is at t = 4. We calculate the excess return by subtracting the risk-free rate from the daily return and compounding it over the 5 day period:

y = (R(t+1)- Ri(t+1) +1) * (R(t+2)- Ri(t+2) +1) * (R(t+3)- Ri(t+3) +1) *(Rt+4-Ri(t+4)+1) * (R(t+5)- Ri(t+5) +1)-1
The independent variables we used are the following:

x1t = Standardized difference between Estimize and Wall Street EPS Consensus of today (t)

x2 = Sector

x3 = Number of Contributors

x4 = Caps (using Shares Outstanding as proxy)

<img width="468" alt="strategy" src="https://user-images.githubusercontent.com/102770592/223012597-83d9a8a0-9f50-4a77-b692-d51003740ced.png">

## Improvement Techniques
We implemented several techniques in improving our strategies. This was built on top of the previous framework and analysis. 

1. Fix a window of 250 tradings days (calendar year) to run rolling-window OLS regressions instead of looking at all previous history
2. Only build a portfolio if there are at least 10 positive return predictions on that day. Otherwise, we skip that day and continue on to the next day.
3. Create a grid of variables to modify one-by-one, similar to how hyperparameter tuning is done in machine learning models, except this is a brute force for loop over for loop, one for each variable. Our parameters to tune are:

       Top x tickers to choose (here we started with x = 5)
       
       Which x variables to include / exclude; we chose 3 subsets of combinations
      
       Trading pool must contain at least x positive return predictions (here we started with x = 10)
       
       The size of the window in number of trading days (we started with 250)


## Results
Our trading portfolio results in an average of more than 100 trading days out of 250 available days. This means that we are on average trading around one time for every two days. 
Even though there were roughly equal numbers of data points among different sectors in our dataframes, our portfolio sees a 68% share of information technology tickers, with the runner up, Industrial tickers, being a mere 13% of all the tickers traded by our portfolio. However, the same Industrial tickers seem to be more consistently traded as part of our portfolio than any other sector. Only some information technology companies are consistently traded: Microsoft and Cisco. This alludes to the possibility of more churn in the information technology sector, shorter lifetimes of companies in said sectors with more rapid growth and decline resulting in greater variability. 

<img width="468" alt="EDA3" src="https://user-images.githubusercontent.com/102770592/223012580-bc578d5b-2b11-438a-865b-8e30a7f84f42.png">

As we examine our cumulative portfolio returns and compare it with the market , we see that the variation of our portfolio highly corresponds to that of the market. In particular, the dip at around trading day 620 happens during the pandemic and we could see a trend in our portfolio that the magnitudes of dips are much greater, but its resilience manifests in the rebound rise after the dips (also seen right after trading day 100). While on the daily level , our initial portfolio strategy (light blue line) does not seem to do much better than the market (it mostly has greater volatility), if we tune the parameters, the cumulative returns almost double. 

Our best portfolio cumulative returns outperforms the market by 7.12% on average.

(Dark Blue) Our best portfolio cumulative returns after tuning top x tickers, which x variables to include/exclude, trading pool having at least x tickers, selecting positive predicted returns only; 

(Light Blue) Intermediate strategy portfolio returns; 

(Red) Cumulative market returns.

<img width="363" alt="results" src="https://user-images.githubusercontent.com/102770592/223012595-f5e9633e-819c-4173-a3fb-ea9dff4d7207.png">

## Conclusion
After running our regression model and performing our strategy using the model, three conclusions are drawn. Firstly, EPS difference can provide enough information to achieve abnormal returns. Secondly, Estimize data is more accurate for certain sectors and with more contributors. Thirdly, large market caps contribute more prediction power to returns.

