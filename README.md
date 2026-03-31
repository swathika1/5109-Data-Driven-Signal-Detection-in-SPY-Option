# 5109-Data-Driven-Signal-Detection-in-SPY-Option


Dataset link https://www.kaggle.com/datasets/shankerabhigyan/s-and-p500-options-spy-implied-volatility-2019-24?select=spy_options_data_24.json




Idea: Data-Driven Arbitrage Signal Detection in SPY Options 

The core concept: 

Traditional arbitrage relies on theoretical pricing models such as Black-Scholes, which assume ideal market conditions. However, real markets exhibit frictions, asynchronous data, and behavioural effects. 

This project adopts a data-driven approach, where machine learning models are used to learn the pricing structure of options and identify systematic deviations between predicted and observed prices, which can serve as potential arbitrage signals. 

Data Exploration & Analysis (15 pts) 

Perform EDA on SPY options dataset:  

distribution of prices, IV, Greeks, and spreads  

Analyse option pricing behaviour across:  

moneyness (strike / spot)  

time to maturity  

implied volatility  

Visualize:  

price vs strike (smile)  

IV vs strike (volatility smile)  

price vs maturity  

Study relationships between:  

Greeks and option prices  

IV and option price deviations  

Identify regions where pricing behaves nonlinearly:  

deep OTM  

near expiry  

 

The Data Challenge (35 pts) 

Step 1: Build a predictive pricing model 

Inputs:  

strike, moneyness, T  

IV, Greeks  

Models:  

baseline regression  

advanced: neural network / tree-based model  

Step 2: Define mispricing signal 

Signal=Market Price−Predicted Price  

Step 3: Decision-making layer (Prescriptive analytics) 

If signal > threshold → overpriced → sell  

If signal < threshold → underpriced → buy  

Step 4: Evaluate strategy 

Simulate:  

returns from signals  

Sharpe ratio  

hit rate  

Compare:  

naive strategy vs model-based  

Step 5: Risk analysis 

Analyse:  

signal stability across time  

performance in high vs low volatility regimes 

 
