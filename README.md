import requests
from bs4 import BeautifulSoup
import lxml
import csv
if response.status_code==200:
    print("conneted to the website")
    html_content=response.text
else:
    print(f"Connection failed! Status code: {response.status_code}")
    Soup=BeautifulSoup(html_content,'lxml')
    hotel_divs=Soup.find_all('div',role="listitem")
    with open('hotel_data.csv', 'w', encoding='utf-8', newline='') as file_csv:
    writer = csv.writer(file_csv)
    
    # Correct header row
    writer.writerow(['hotel_name', 'locality', 'price', 'rating', 'score', 'review', 'link'])

    for hotel in hotel_divs:
        hotel_name = hotel.find('div', class_="f6431b446c a15b38c233")
        hotel_name = hotel_name.text.strip() if hotel_name else "NA"

        location = hotel.find('span', class_="aee5343fdb def9bc142a")
        location = location.text.strip() if location else "NA"

        price = hotel.find('span', class_="f6431b446c fbfd7c1165 e84eb96b1f")
        price = price.text.strip().replace('₹', '') if price else "NA"

        rating = hotel.find('div', class_="a3b8729ab1 e6208ee469 cb2cbb3ccb")
        rating = rating.text.strip() if rating else "NA"

        score = hotel.find('div', class_="a3b8729ab1 d86cee9b25")
        score = score.text.strip().split(' ')[-1] if score else "NA"

        review = hotel.find('div', class_="abf093bdfe f45d8e4c32 d935416c47")
        review = review.text.strip() if review else "NA"

        link = hotel.find('a', href=True)
        link = link['href'] if link else "NA"

        writer.writerow([hotel_name, location, price, rating, score, review, link])
    
