# 🦸‍♂️ Marvel Movie Data Pipeline — AWS Lambda + OMDB API + S3

This project is a **serverless data engineering pipeline** built with **AWS Lambda**, **Python**, and **S3**, designed to **scrape, clean, enrich, and store Marvel Cinematic Universe (MCU) movie data**.

It automatically:

- Scrapes movie metadata from Wikipedia (Marvel Cinematic Universe film pages)
- Cleans and structures the data into a usable format
- Enriches the dataset with additional metadata from the **OMDB API**
- Scrapes **recurring character and cast** information
- Stores the final CSV datasets directly into an **Amazon S3 bucket**

---

## 🧠 Features

- 🔎 **Scrape Marvel Movie Titles:** Extracts detailed MCU movie tables from Wikipedia (Phases 1–6).
- 🧹 **Clean & Format Data:** Removes unnecessary reference tags, parses dates, and standardizes columns.
- 🎥 **Enrich Movie Metadata:** Calls the OMDB API for each film to retrieve ratings, runtime, plot, etc.
- 🦸‍♂️ **Character & Cast Scraper:** Collects recurring character information from Wikipedia.
- ☁️ **AWS S3 Integration:** Uploads final datasets (`movies.csv`, `omdb.csv`, `characters.csv`) to an S3 bucket.
- ⚙️ **Serverless-Ready:** Designed to run as an **AWS Lambda function** on a schedule or event trigger.

---

## 🗂️ Project Structure

```
├── movies_function.py      # Main Lambda function script
├── README.md               # Project documentation (this file)
```

---

## 🧰 Requirements

Before deploying or running locally, make sure you have:

- 🐍 **Python 3.8+**
- 📦 Required libraries:
  - `pandas`
  - `requests`
  - `boto3`
  - `beautifulsoup4`

Install them locally:

```bash
pip install pandas requests boto3 beautifulsoup4
```

---

## 🔑 Environment Setup

### 1. OMDB API Key

Sign up for a free OMDB API key at [https://www.omdbapi.com/](https://www.omdbapi.com/).

Replace the placeholder in `movies_function.py`:

```python
OMDB_API_KEY = "your_actual_api_key_here"
```

### 2. AWS S3 Bucket

Create an S3 bucket and update the variable:

```python
S3_BUCKET_NAME = "your-s3-bucket-name"
```

Make sure your Lambda role or local AWS credentials have `s3:PutObject` permissions.

---

## 🚀 How It Works

### 1. Scrape Movie Metadata

```python
movies_df = scrape_marvel_movies()
```

Extracts MCU movie tables from Wikipedia and stores them in a Pandas DataFrame.

### 2. Clean Data

```python
movies_df_cleaned = clean_movie_data(movies_df)
```

Cleans text, removes references, and standardizes date formats.

### 3. Enrich with OMDB API

```python
omdb_data = fetch_omdb_data("Iron Man")
```

Fetches detailed metadata for each film.

### 4. Scrape Character Data

```python
characters_df = scrape_characters_data()
```

Collects recurring cast and character details.

### 5. Upload to S3

```python
upload_to_s3(movies_df_cleaned, 'movies.csv')
upload_to_s3(omdb_df, 'omdb.csv')
upload_to_s3(characters_df, 'characters.csv')
```

Saves all final datasets to S3 for downstream analysis.

---

## 🪄 AWS Lambda Deployment

1. Zip your code and dependencies:

```bash
zip -r marvel_lambda.zip movies_function.py
```

2. Upload to AWS Lambda (via console or CLI).

3. Set the handler:

```
lambda_function.lambda_handler
```

4. Add environment variables (optional) or update constants directly.

5. Trigger Lambda manually, via **EventBridge (Cron)**, or **API Gateway**.

---

## 📊 Output Files in S3

| File             | Description                                               |
| ---------------- | --------------------------------------------------------- |
| `movies.csv`     | Cleaned MCU film list with release dates, directors, etc. |
| `omdb.csv`       | OMDB-enriched metadata (ratings, plot, runtime, etc.)     |
| `characters.csv` | Recurring cast and character information                  |

---

## 📈 Example Use Cases

- 🧪 **Data Analytics:** Load CSVs into AWS Athena, Redshift, or Pandas for analysis.
- 📊 **Dashboarding:** Power BI / Tableau dashboards for Marvel movie timelines.
- 🤖 **Data Science:** Train recommendation models or sentiment analysis using enriched metadata.

---

## ⚠️ Notes

- The Wikipedia page structure may change — occasional scraping adjustments might be needed.
- OMDB API has rate limits on the free tier (1,000 requests/day).
- Ensure IAM permissions are correctly set for Lambda to access S3.

---

## 🧑‍💻 Author

**Darshan** – [GitHub Profile](https://github.com/naikdarshan45)  
Feel free to open issues or pull requests to improve this project.

---

