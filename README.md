import requests

def convert_multiple_currencies(amount, from_currency, to_currencies):
    to_currencies_str = ','.join(to_currencies)
    url = f"https://api.frankfurter.app/latest?amount={amount}&from={from_currency}&to={to_currencies_str}"

    try:
        response = requests.get(url)
        response.raise_for_status()
        data = response.json()
        print(f"\n{amount} {from_currency} is equal to:")
        for currency, converted_amount in data['rates'].items():
            print(f"  {converted_amount:.2f} {currency}")
        return data['rates']
    except requests.exceptions.RequestException as e:
        print(f"Error fetching exchange rate: {e}")
        return None

# Example usage
if __name__ == "__main__":
    amount = float(input("Enter amount: "))
    from_currency = input("From currency (e.g., USD): ").upper()
    to_currencies = input("To currencies (comma-separated, e.g., EUR,GBP,INR): ").upper().split(',')

    convert_multiple_currencies(amount, from_currency, to_currencies)
