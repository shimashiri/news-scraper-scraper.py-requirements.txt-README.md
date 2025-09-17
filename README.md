# news-scraper-scraper.py-requirements.txt-README.md
import requests
from bs4 import BeautifulSoup
import csv
from datetime import datetime

URL = "https://news.ycombinator.com/"  # Hacker News
FILE_NAME = "news.csv"

def scrape_news():
    response = requests.get(URL)
    soup = BeautifulSoup(response.text, "html.parser")

    articles = soup.select(".storylink")
    results = []

    for i, article in enumerate(articles[:10], start=1):
        title = article.get_text()
        link = article["href"]
        results.append({"rank": i, "title": title, "link": link})

    return results

def save_to_csv(data):
    with open(FILE_NAME, "w", newline="", encoding="utf-8") as csvfile:
        writer = csv.DictWriter(csvfile, fieldnames=["rank", "title", "link"])
        writer.writeheader()
        writer.writerows(data)

if __name__ == "__main__":
    print("🔎 Scraping latest news...")
    news = scrape_news()
    save_to_csv(news)
    print(f"✅ Saved {len(news)} news articles to {FILE_NAME} at {datetime.now()}")
