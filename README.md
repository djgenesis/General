# Huginn Automated Daily Digest & Price Monitor

This Huginn Scenario automates data collection from multiple sources (RSS feeds, Web Scraping, and Webhooks), aggregates the events, processes them via the Google Gemini API to generate a daily summary in Greek, and dispatches the final results directly to Discord.

The entire workflow executes sequentially every morning between **06:00 AM and 09:00 AM**.

---

## 📋 Workflow Architecture

The scenario is structured into 4 distinct, time-scheduled steps:

### 🔄 Step 1: Data Collection & Scraping (06:00 AM)
Several agents run in parallel to gather data from the web:
* **RSS Scraper (Twitter/X Feeds):** Monitors specific X accounts utilizing RSS.app feeds.
* **RSS Scraper (GitHub Releases):** Tracks new releases for a curated list of repositories (including Huginn, Immich, AzuraCast, NuvioTV, etc.).
* **Web Scraper (Spitishop.gr & Anemos.gr):** Monitors availability and pricing changes for specific product categories (King Size bedsheets).
* **Receive Webhook:** An exposed endpoint configured to accept and ingest incoming external payloads instantly.

### 🗙 Step 2: Data Aggregation (07:00 AM)
* **Data Aggregator (Combine Multi-Source Events):** A Digest Agent that collects all events generated during Step 1 and groups them into a single comprehensive data payload.

### 🤖 Step 3: AI Summary Generation (08:00 AM)
* **Gemini Summary Generator:** Forwards the aggregated text payload to the **Gemini 2.5 Flash API**. The system prompt instructs the AI to act as a professional news presenter and draft a concise narrative summary in Greek (under 2,000 words), sorting X/GitHub updates first, followed by retail price movements.

### 📢 Step 4: Discord Dispatching (09:00 AM)
* **Discord Dispatcher (Text):** Formats a clean Markdown summary displaying the top 5 Tweets/Alerts and top 5 retail products, including a fallback URL link to view any additional events.
* **Discord Dispatcher (Gemini Summary):** Posts the structured, AI-generated Greek text summary to your designated Discord channel.
* *(Optional / Disabled)* **Discord Dispatcher (Audio):** Pre-configured logic to handle and dispatch an audio daily digest (Base64 encoded MP3).

---

## 🛠️ Prerequisites & Credentials

To get this scenario running seamlessly, you need to set up the following **Credentials** within your Huginn instance:

1. `gemini_api_key`: Your Google Gemini API Key used to authenticate requests to the `generativelanguage.googleapis.com` endpoint.
2. `discord_webhook_url`: The Webhook URL provided by your Discord server channel where notifications should be published.

---

## 🚀 Installation (Importing to Huginn)

1. Download or copy the JSON scenario file from this repository.
2. Log in to your **Huginn** dashboard.
3. Navigate to **Scenarios**
4. Click on **Import Scenario**, upload the JSON file, and confirm.
5. Insert required environment credentials in the relevant huginn tab (`gemini_api_key` and `discord_webhook_url`).
