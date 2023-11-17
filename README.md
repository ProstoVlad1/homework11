import requests
from bs4 import BeautifulSoup

class CurrencyConverter:
    def __init__(self, exchange_rate):
        self.exchange_rate = exchange_rate

    def convert_to_usd(self, amount):
        return amount / self.exchange_rate

def get_usd_exchange_rate():
    url = "https://www.bankofcountry.com/exchangerates](https://bank.gov.ua/ua/markets/currency-market"
    response = requests.get(url)

    if response.status_code == 200:
        soup = BeautifulSoup(response.text, 'html.parser')
        usd_rate_element = soup.find("span", class_="usd-rate")
        
        if usd_rate_element:
            usd_rate = float(usd_rate_element.text.strip())
            return usd_rate
        else:
            print("Failed to find USD exchange rate on the page.")
    else:
        print(f"Failed to retrieve data from {url}. Status code: {response.status_code}")

    return None

if __name__ == "__main__":
    usd_exchange_rate = get_usd_exchange_rate()

    if usd_exchange_rate is not None:
        converter = CurrencyConverter(usd_exchange_rate)
        
        try:
            amount = float(input("Enter the amount in your currency: "))
            converted_amount = converter.convert_to_usd(amount)
            print(f"{amount} in your currency is approximately {converted_amount:.2f} USD.")
        except ValueError:
            print("Invalid input. Please enter a valid numeric value.")
