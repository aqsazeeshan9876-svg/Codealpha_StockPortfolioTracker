# Codealpha_StockPortfolioTracker

import csv

def calculate_portfolio_value(stock_prices, portfolio):
    """Calculate and return total investment value."""
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
    """Save portfolio to.txt file as per requirement."""
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
            file.write("=" * 40 + "\n")
            file.write(f"Total Investment Value: ${total_value:.2f}\n")
        print(f"Portfolio saved to {filename}")
    except Exception as e:
        print(f"Error saving TXT file: {e}")

def save_to_csv(portfolio, stock_prices, total_value):
    """Optional: Save portfolio to.csv file."""
    filename = "portfolio.csv"
    try:
        with open(filename, 'w', newline='') as file:
            writer = csv.writer(file)
            writer.writerow(["Stock", "Quantity", "Price", "Value"])
            for stock, qty in portfolio.items():
                if stock in stock_prices:
                    price = stock_prices[stock]
                    value = price * qty
                    writer.writerow([stock, qty, price, value])
            writer.writerow(["TOTAL", "", "", total_value])
        print(f"Portfolio saved to {filename}")
    except Exception as e:
        print(f"Error saving CSV file: {e}")

def main():
    # 1. Hardcoded dictionary - REQUIRED
    stock_prices = {
        "AAPL": 180.00,
        "TSLA": 250.00,
        "GOOGL": 140.00,
        "MSFT": 330.00,
        "AMZN": 130.00
    }

    portfolio = {}

    print("====== CODEALPHA STOCK PORTFOLIO TRACKER ======")
    print("Available stocks:", ", ".join(stock_prices.keys()))

    while True:
        print("\n1. Add Stock 2. View Portfolio 3. Save to TXT 4. Save to CSV 5. Exit")
        choice = input("Enter choice: ").strip()

        if choice == "1":
            # 2. User inputs stock names and quantity - REQUIRED
            stock = input("Enter stock symbol: ").upper().strip()
            if stock not in stock_prices:
                print("Error: Stock not available. Choose from list above.")
                continue
            try:
                quantity = int(input(f"Enter quantity for {stock}: "))
                if quantity <= 0:
                    print("Error: Quantity must be > 0")
                    continue
                portfolio[stock] = portfolio.get(stock, 0) + quantity
                print(f"Added {quantity} shares of {stock}")
            except ValueError:
                print("Error: Enter a valid number")

        elif choice == "2":
            if not portfolio:
                print("Portfolio is empty. Add stocks first.")
            else:
                # 3. Display total investment value - REQUIRED
                calculate_portfolio_value(stock_prices, portfolio)

        elif choice == "3":
            if not portfolio:
                print("Portfolio is empty. Nothing to save.")
            else:
                total = calculate_portfolio_value(stock_prices, portfolio)
                # 4. Save to.txt file - REQUIRED
                save_to_txt(portfolio, stock_prices, total)

        elif choice == "4":
            if not portfolio:
                print("Portfolio is empty. Nothing to save.")
            else:
                total = calculate_portfolio_value(stock_prices, portfolio)
                # Optional: Save to.csv file
                save_to_csv(portfolio, stock_prices, total)

        elif choice == "5":
            print("Program terminated.")
            break
        else:
            print("Invalid choice. Enter 1-5.")

if __name__ == "__main__":
    main()
