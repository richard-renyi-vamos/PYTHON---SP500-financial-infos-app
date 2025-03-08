CHATGPT CONVERSATION

*************

create a PYTHON code that shows financial infos about the SP500 companies
ChatGPT said:
Here is a Python script that fetches financial information about S&P 500 companies. It uses the yfinance library to retrieve data like stock price, market cap, and P/E ratio.

Sp500 Financials

Answer in chat instead
This script:

Fetches the list of S&P 500 companies from Wikipedia.
Retrieves financial data for the first 10 companies using yfinance.
Displays key financial metrics such as market cap, P/E ratio, and stock price.
Let me know if you want any modifications! 🚀

*************


import yfinance as yf
import pandas as pd

def get_sp500_companies():
    url = "https://en.wikipedia.org/wiki/List_of_S%26P_500_companies"
    tables = pd.read_html(url)
    df = tables[0]  # The first table contains the S&P 500 company list
    return df[['Symbol', 'Security', 'GICS Sector', 'GICS Sub-Industry']]

def get_stock_info(symbol):
    stock = yf.Ticker(symbol)
    info = stock.info
    return {
        "Name": info.get("longName", "N/A"),
        "Sector": info.get("sector", "N/A"),
        "Industry": info.get("industry", "N/A"),
        "Market Cap": info.get("marketCap", "N/A"),
        "P/E Ratio": info.get("trailingPE", "N/A"),
        "Price": info.get("regularMarketPrice", "N/A"),
    }

def main():
    sp500 = get_sp500_companies()
    symbols = sp500["Symbol"].tolist()[:10]  # Fetching data for the first 10 companies
    
    financials = []
    for symbol in symbols:
        try:
            data = get_stock_info(symbol)
            data["Symbol"] = symbol
            financials.append(data)
        except Exception as e:
            print(f"Error fetching data for {symbol}: {e}")
    
    df_financials = pd.DataFrame(financials)
    print(df_financials)

if __name__ == "__main__":
    main()
