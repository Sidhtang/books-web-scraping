# books-web-scraping
this set of code will teach you how to scrape data from a website
import pandas
import requests
from bs4 import BeautifulSoup

response = requests.get('https://books.toscrape.com/')
data = BeautifulSoup(response.text, 'html.parser')
b1 = data.find(class_='product_pod')

base_url = 'http://books.toscrape.com/'
b1_url = base_url + b1.h3.a['href']

response = requests.get(b1_url)
data = BeautifulSoup(response.text, 'html.parser')
title = data.h1.string
price = data.find(class_='price_color').string
qty = data.find(class_='instock availability').text.strip()

print(title)
print(price)
print(qty)
print(b1_url)

book_detail = []
book_detail.append([title, b1_url, price, qty])

df = pandas.DataFrame(book_detail, columns=['Title', 'Link', 'Price', 'Quantity in Stock'])
print(df)
df.to_csv('books.csv'  )
