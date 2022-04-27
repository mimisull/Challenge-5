# Challenge-5
The Directory for this respoitory is:
[Summary](https://github.com/mimisull/Challenge-5/blob/main/README.md)
[Code](https://github.com/mimisull/Challenge-5/blob/main/financial_planning_tools.ipynb)

**FinTech Consulting for Credit Union**

Specifically, I am building a tool to help credit union members evaluate their financial health. They can assess their monthly budgets as well as forecast their retirement plan based on current holdings of cypto, stock, and bonds with this tool.

*User Story*:
As a user, I need to determine my monthly budget and if I should invest it as well as how I can invest it.

As a user, I need to be able to visualize my current savings.

As a user, I need to forecast how I should manage my money so that I can retire at the right time.

*Acceptance Criteria*:
Given that I’m unable to determine when I should invest and how I should balance my portfolio, I need to use quantitative analysis to determine the best choice based on the criteria above.


## Usage
The project works by retrieving accurate data using APIs, and determining savings based on real-time data on certain exchanges.
Imported the necessary tools with this code:
'''import os
import requests
import json
import pandas as pd
from dotenv import load_dotenv
import alpaca_trade_api as tradeapi
from MCForecastTools import MCSimulation

%matplotlib inline'''

Call APIs for crypto:
'''btc_response = requests.get(btc_url).json()'''
'''print(json.dumps(btc_response, indent=4, sort_keys=True))'''
And the same for ETH.

Calculating current value with this code:
'''btc_value = btc_coins *btc_price'''
'''eth_value = eth_coins * eth_price'''
'''total_crypto_wallet = btc_value + eth_value'''

Generating Alpaca APIs for Stock/Bond holdings with this code:
'''alpaca_api_key = os.getenv("ALPACA_API_KEY")
alpaca_secret_key = os.getenv("ALPACA_SECRET_KEY")

alpaca = tradeapi.REST(
    alpaca_api_key,
    alpaca_secret_key,
    api_version="v2")'''

Found current prices with this code:
'''tickers = ["SPY", "AGG"]
timeframe = "1Day"'''
'''start_date = pd.Timestamp("2022-04-25", tz="America/New_York").isoformat()
end_date = pd.Timestamp("2022-04-25", tz="America/New_York").isoformat()'''

'''df_portfolio = alpaca.get_bars(
    tickers,
    timeframe,
    start = start_date,
    end = end_date
).df'''
'''SPY = df_portfolio[df_portfolio['symbol']=='SPY'].drop('symbol', axis=1)
AGG = df_portfolio[df_portfolio['symbol']=='AGG'].drop('symbol', axis=1)


df_portfolio = pd.concat([SPY, AGG],axis=1, keys=['SPY','AGG'])

df_portfolio.head(5)'''

Calculated value in USD with this code:
'''agg_close_price = float(df_portfolio["AGG"]["close"])'''
'''spy_close_price = float(df_portfolio["SPY"]["close"])'''
'''agg_value = agg_close_price * agg_shares'''
'''spy_value = spy_close_price * spy_shares'''
'''total_stocks_bonds = spy_value + agg_value'''
'''total_portfolio = total_stocks_bonds + total_crypto_wallet'''

Created a dataframe for the emergency fund with this code:
'''savings_df = pd.DataFrame(
    {'amount':[total_stocks_bonds, total_crypto_wallet]},
    index=['stock/bond', 'crypto']'''

Plotted a pie chart so that thye could evaluate their portfolio with this code:
'''savings_df.plot.pie(y='amount', title='Portfolio (stock/bond and crypto) - 2022-04-24 ')'''

Determined if they should invest with this if statement:
'''if total_portfolio > emergency_fund_value:
    print("Congrats, you have enough money for the fund!")
elif total_portfolio == emergency_fund_value:
    print("Congrats on reaching this important financial goal.")
else:
    print(f"You are ${total_portfolio - emergency_fund_value} away from reaching this goal.")'''

Made an API call to get 3 years of pricing data with this code:
''' df_portfolio = alpaca.get_bars(
    tickers,
    timeframe,
    start = start_date,
    end = end_date
).df'''

Organized that with this code:
'''# Separate ticker data
SPY = df_portfolio[df_portfolio['symbol']=='SPY'].drop('symbol', axis=1)
AGG = df_portfolio[df_portfolio['symbol']=='AGG'].drop('symbol', axis=1)

df_portfolio = pd.concat([SPY, AGG],axis=1, keys=['SPY','AGG'])'''

Ran Monte Carlo Simulations for both 30 and 10 year forecasts respectively with this code:
'''MC_split_weight = MCSimulation(
    portfolio_data = df_portfolio,
    weights = [.60,.40],
    num_simulation = 500,
    num_trading_days = 252*30'''
'''MC_spy_weight = MCSimulation(
    portfolio_data = df_portfolio,
    weights = [.80,.20],
    num_simulation = 500,
    num_trading_days = 252*10'''

Plotted each and analyzed with this code:
'''stock_split_line_plot = MC_split_weight.plot_simulation()'''
'''split_weight_distribution_plot = MC_split_weight.plot_distribution()'''
'''split_weight_table = MC_split_weight.summarize_cumulative_return()'''
'''spy_heavy_line_plot = MC_spy_weight.plot_simulation()'''
'''spy_heavy_distribution_plot = MC_spy_weight.plot_distribution()'''
'''spy_weight_table = MC_spy_weight.summarize_cumulative_return()'''

Finally,determined which time horizon and weight would be best based on upper and lower bound CIs with this code:
'''ci_lower_thirty_cumulative_return = round(split_weight_table[8]*12000,2)
ci_upper_thirty_cumulative_return = round(split_weight_table[9]*12000,2)'''
'''ci_lower_ten_cumulative_return = round(spy_weight_table[8]*12000,2)
ci_upper_ten_cumulative_return = round(spy_weight_table[9]*12000,2)'''

## Contributors
Michael Sullivan

Cal Berkeley Fintech 

Kevin Lee