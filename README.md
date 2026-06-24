# Codealpha_StockPortfolioTracker

import csv

def calculate_portfolio_value(stock_prices, portfolio):
    total_value = 0.0
    print("\n======= PORTFOLIO SUMMARY =======")
    print(f"{'Stock':<8} {'Qty':<6} {'Price':<10} {'Value':<12}")
    print("-" * 38)

    for stock, quantity in portfolio.items():
        if stock in stock_prices:
            price = stock_prices[stock]
            value = price * quantity
            total_value += value
            print(f"{stock:<8} {quantity:<6} ${price:<9.2f} ${value:<11.2f}")
        else:
            print(f"{stock:<8} {quantity:<6} {'N/A':<10} {'N/A':<12}")

    print("-" * 38)
    print(f"TOTAL INVESTMENT VALUE: ${total_value:.2f}")
    return total_value

def save_to_txt(portfolio, stock_prices, total_value):
    filename = "portfolio.txt"
    try:
        with open(filename, 'w') as file:
            file.write("STOCK PORTFOLIO TRACKER - CodeAlpha\n")
            file.write("=" * 40 + "\n")
            for stock, qty in portfolio.items():
                if stock in stock_prices:
                    price = stock_prices[stock]
                    value = price * qty
                    file.write(f"{stock}: {qty} shares @ ${price:.2f} = ${value:.2f}\n")
