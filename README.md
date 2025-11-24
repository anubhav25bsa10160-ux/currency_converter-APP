# currency_converter-APP
#The currency converter Python code I provided uses a free, open-access API to get live exchange rates for almost all world currencies.



#code of this program
import requests

BASE_URL = "https://open.er-api.com/v6/latest/"  

def get_rates(base_code):
    """
    Download latest exchange rates for the given base currency code.
    Returns a dict: { 'USD': rate, 'INR': rate, ... }
    """
    url = BASE_URL + base_code
    response = requests.get(url, timeout=10)

    if response.status_code != 200:
        print("HTTP error:", response.status_code)
        return None

    data = response.json()
    if data.get("result") != "success":
        print("API error:", data.get("error-type", "Unknown error"))
        return None

    return data.get("rates", {})

def convert_currency(amount, from_code, to_code):
    """
    Convert 'amount' from from_code to to_code using live rates.
    """
    from_code = from_code.upper()
    to_code = to_code.upper()

    rates = get_rates(from_code)
    if rates is None:
        return None

    if to_code not in rates:
        print(f"Target currency {to_code} not supported.")
        return None

    rate = rates[to_code]
    converted = amount * rate
    return converted, rate
def main():
    print("=== Simple Currency Converter (All World Currencies) ===")
    print("Example codes: INR, USD, EUR, GBP, JPY, AUD, CAD, CNY, etc.")
    print("Type 'exit' anytime to quit.\n")

    while True:
        from_code = input("From currency code: ").strip()
        if from_code.lower() == "exit":
            break

        to_code = input("To currency code: ").strip()
        if to_code.lower() == "exit":
            break

        amount_str = input("Amount: ").strip()
        if amount_str.lower() == "exit":
            break

        try:
            amount = float(amount_str)
        except ValueError:
            print("Invalid amount. Please enter a number.\n")
            continue

        result = convert_currency(amount, from_code, to_code)
        if result is None:
            print("Conversion failed. Try again.\n")
            continue

        converted, rate = result
        print(f"\nRate: 1 {from_code.upper()} = {rate:.4f} {to_code.upper()}")
        print(f"{amount} {from_code.upper()} = {converted:.4f} {to_code.upper()}\n")

if _name_ == "_main_":
    main()
