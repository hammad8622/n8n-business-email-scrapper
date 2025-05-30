# 📧 Email Scraper - Google Maps Restaurant Emails via n8n

This project automates the extraction of restaurant email addresses from Google Maps using [n8n](https://n8n.io), an open-source workflow automation platform. It consists of a parent-child workflow system: one handles batch queries, the other scrapes, cleans, and stores email addresses into a Google Sheet.

---

## 🔍 Project Overview

The **Email Scraper** project is designed as a modular data pipeline that processes batches of Google Maps search queries. For each query, it:

* Extracts restaurant URLs from the Google Maps HTML.
* Visits each URL to extract email addresses.
* Cleans and filters the emails.
* Saves them to a Google Sheet.

This system uses two workflows:

* **`main_scrapper`**: Manages queries and launches scrapes.
* **`scrapper`**: Executes the actual scraping and processing logic.

---

## 📈 Research & Concept Development

Key automation principles implemented:

* **Batch Processing**: Handles multiple queries efficiently.
* **Workflow Chaining**: Modular design via Execute Workflow node.
* **Web Scraping**: Extracts raw HTML from Google Maps pages.
* **Regex Parsing**: Pulls email addresses from web content.
* **Data Cleansing**: Deduplication and relevance filtering.
* **Cloud Integration**: Final data is saved in Google Sheets.

This is a classic **Data Processing Pipeline**, transforming unstructured web content into clean, structured email data.

---

## 🧰 Workflow Hierarchy

### 1. `main_scrapper` (Parent)

* Triggered manually.
* Iterates through search queries.
* Triggers the `scrapper` workflow.
* Introduces delay to prevent request overload.

### 2. `scrapper` (Child)

* Accepts a single search query.
* Scrapes Google Maps result page.
* Extracts and processes restaurant URLs.
* Fetches and parses emails from each page.
* Cleans, filters, and stores results.

---

## 🔢 Node-by-Node Breakdown

### `main_scrapper` Nodes:

1. **Run Workflow** (`manualTrigger`): User-initiated trigger.
2. **Loop over queries** (`splitInBatches`): Iterates through hardcoded search queries.
3. **Execute Workflow** (`executeWorkflow`): Runs `scrapper` workflow per query.
4. **Wait between executions** (`wait`): 3-second delay between scrapes.

### `scrapper` Nodes:

1. **Starts scraper workflow** (`workflowTrigger`): Accepts query parameter.
2. **Search Google Maps with query** (`httpRequest`): Fetches HTML content from Google Maps.
3. **Extract URLs from HTML** (`code`): Uses regex to extract URLs from HTML.
4. **Remove Duplicate URLs** (`removeDuplicates`): Ensures uniqueness of links.
5. **Loop over pages** (`splitInBatches`): Processes each URL.
6. **Scrape Emails from Page** (`code`): Makes HTTP requests and extracts emails via regex.
7. **Aggregate arrays of emails** (`merge`): Combines all found emails into one list.
8. **Split out into default data structure** (`code`): Converts list to standard n8n JSON format.
9. **Remove duplicate emails** (`removeDuplicates`): Deduplicates emails.
10. **Filter irrelevant emails** (`if`): Removes emails with domains like `example.com`.
11. **Save emails to Google Sheet** (`googleSheets`): Appends final results to a Google Sheet.

---

## 🔄 Data Flow Summary

### `main_scrapper`:

1. Manual trigger.
2. Loop through a list of queries.
3. Execute `scrapper` for each query.
4. Delay between each execution.

### `scrapper`:

1. Triggered with a single query.
2. Google Maps results page scraped.
3. Restaurant/place URLs extracted.
4. Duplicate links removed.
5. Loop through URLs.
6. Emails extracted from HTML.
7. All emails aggregated.
8. Standardized data structure applied.
9. Duplicate emails removed.
10. Irrelevant emails filtered.
11. Saved to Google Sheets.

---

## 🛠️ Tools & Versions

### n8n Core Nodes:

* `manualTrigger` (v1)
* `splitInBatches` (v3)
* `executeWorkflow` (v2)
* `wait` (v3)
* `workflowTrigger` (v1)
* `httpRequest` (v4.2)
* `code` (v2)
* `removeDuplicates` (v1.1)
* `merge` (v3)
* `if` (v1)
* `googleSheets` (v3.3)

### APIs/Services:

* Google Maps (via raw HTTP requests)
* Google Sheets (via n8n Google Sheets node)

---

## 🔮 Conclusion

This modular scraping architecture shows how n8n can automate sophisticated web data pipelines. It combines web scraping, regex-based email extraction, and cloud-based storage into a seamless workflow. The separation of control (parent) and execution (child) workflows makes it maintainable, extensible, and production-ready for data harvesting at scale.

---

## 📝 License

MIT License — use, modify, and share freely.

---

## 🤝 Contributing

Contributions are welcome! If you find a bug or want to improve functionality, feel free to fork and submit a PR.

---

## 🔗 Related Links

* [n8n Docs](https://docs.n8n.io)
* [Google Sheets API](https://developers.google.com/sheets/api)
* [Google Maps](https://www.google.com/maps)
