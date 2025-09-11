# Puppeteer Worker 1

This is the **first worker (of three)** that scrapes URLs fed from a backend service (`chunkgeneratorforaimodel`) based on searched keywords.  
The workers run in parallel to share the scraping load, increasing the speed and efficiency of the process.

---

## 📦 About This Package

- Functions as **1/3 of the scraper workload**.
- Consumes jobs (URLs + keywords) from **Kafka**.
- Uses **Puppeteer** to scrape the content of web pages.
- Filters and extracts text that contains the searched keywords.
- Sends results back to Kafka for further processing by the backend (`chunkgeneratorforaimodel`).
- Designed to run together with two other workers for parallel scraping.

---

## ⚙️ How It Works

1. **Kafka Consumer**  
   - Listens for messages on the `topuppeteerworker` topic.  
   - Each message contains ~1/3 of the total URLs to be scraped.

2. **Scraping Process** (`runScraper` in `puppeteerWorker.js`)  
   - Launches Puppeteer in headless mode.
   - Visits each URL.
   - Extracts page text.
   - Checks for presence of keywords from `topicsArray`.
   - If found, pushes a result in the format:

   {
     "text": "page content",
     "metadata": {
       "url": "https://example.com",
       "date": "2025-09-11T10:00:00.000Z",
       "sourcekb": "external",
       "searched": "your_query"
     }
   }

💻 Platform Requirements

At least 12 GB RAM

At least 4 CPU cores (Intel i5 or higher recommended)

Docker installed

🚀 How to Run

Download the Docker image

docker pull ghcr.io/hendram/puppeteerworker1


Start the container

docker run -it -d --network=host ghcr.io/hendram/puppeteerworker1 bash


Find your container name

docker ps

Example: pedantic_payne (your name will differ)


Enter the container

docker exec -it pedantic_payne /bin/bash


Run the service

cd /home/browserconnworker1
node index.js



