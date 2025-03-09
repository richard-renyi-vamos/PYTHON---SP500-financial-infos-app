CHATGPT CONVERSATION

****************

add a GUI to this

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
ChatGPT said:
Here's a GUI version of your script using Tkinter. It allows users to view S&P 500 companies in a dropdown menu, fetch stock data, and display it in a table.

Sp500 Gui

Answer in chat instead
This GUI allows users to select an S&P 500 company from a dropdown, fetch stock data using Yahoo Finance, and display it in a table. Let me know if you want any enhancements! 🚀

***********

import tkinter as tk
from tkinter import ttk, messagebox
import yfinance as yf
import pandas as pd

def get_sp500_companies():
    url = "https://en.wikipedia.org/wiki/List_of_S%26P_500_companies"
    tables = pd.read_html(url)
    df = tables[0]  # The first table contains the S&P 500 company list
    return df[['Symbol', 'Security', 'GICS Sector', 'GICS Sub-Industry']]

def get_stock_info(symbol):
    try:
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
    except Exception as e:
        messagebox.showerror("Error", f"Could not fetch data: {e}")
        return None

def fetch_data():
    selected_symbol = symbol_var.get()
    if not selected_symbol:
        messagebox.showwarning("Warning", "Please select a company.")
        return
    
    stock_data = get_stock_info(selected_symbol)
    if stock_data:
        for i, key in enumerate(stock_data.keys()):
            tree.item(i, values=(key, stock_data[key]))

# GUI Setup
root = tk.Tk()
root.title("S&P 500 Stock Info")
root.geometry("500x400")

tk.Label(root, text="Select a Company:").pack(pady=5)

sp500_df = get_sp500_companies()
symbols = sp500_df["Symbol"].tolist()

symbol_var = tk.StringVar()
symbol_dropdown = ttk.Combobox(root, textvariable=symbol_var, values=symbols, state="readonly")
symbol_dropdown.pack(pady=5)

fetch_button = tk.Button(root, text="Get Stock Info", command=fetch_data)
fetch_button.pack(pady=10)

columns = ("Metric", "Value")
tree = ttk.Treeview(root, columns=columns, show="headings")
tree.heading("Metric", text="Metric")
tree.heading("Value", text="Value")
tree.pack(expand=True, fill="both", pady=10)

for _ in range(6):
    tree.insert("", "end", values=("", ""))

root.mainloop()
