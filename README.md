# Python-web-scraping
Amazon product price alert on mail


import requests
from bs4 import BeautifulSoup
import smtplib

URL = 'https://www.amazon.in/Sony-Full-Frame-Mirrorless-Interchangeable-Lens-F3-5-5-6/dp/B07B45D8WV/ref=sr_1_1?keywords=SONY+A7&qid=1568312764&s=gateway&sr=8-1'

headers = {"User-Agent": 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36'}


def check_price():
    page = requests.get(URL, headers=headers)

    soup = BeautifulSoup(page.content, 'html.parser')

    print(soup.prettify())

    title = soup.find(id="productTitle").get_text()
    price1 = soup.find(id="priceblock_ourprice").get_text()
    price = "".join(price1.split(","))
    converted_price = int(price[1:8])

    if (converted_price < 155000):
        send_mail()

    print(converted_price)
    print(title.strip())

    if (converted_price < 155000):
        send_mail()

def send_mail():
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.ehlo()

    server.login('ankitmarwaha8@gmail.com','eoqazzgwpxjbgwjt')

    subject = 'Price fell down'
    body =  'Check the Amazon link https://www.amazon.in/Sony-Full-Frame-Mirrorless-Interchangeable-Lens-F3-5-5-6/dp/B07B45D8WV/ref=sr_1_1?keywords=SONY+A7&qid=1568316340&s=gateway&sr=8-1'

    msg = f"Subject: {subject}\n\n{body}"

    server.sendmail(
        'ankitmarwaha8@gmail.com',
        'ankitmarwaha7@gmail.com',
        msg
    )
    print('Hey Email Has Been Sent')

    server.quit()

check_price()
