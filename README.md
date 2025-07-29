# Automated Tennis Player Performance Reporting ELT Pipeline

## Project Overview

This project simulates a real-world analytics workflow by building an end-to-end ELT pipeline. The system automatically extracts tennis match data from a local database, generates a performance summary for a specific player (Ben Shelton), and uploads the final report to a Google Sheet for easy access by stakeholders.

This project demonstrates the integration of key data technologies (Python, SQL, Cloud APIs) to create a fully automated and reproducible reporting system.

## Technologies Used

*   **Python:** The core language for data processing and pipeline orchestration.
*   **Pandas:** For data manipulation and analysis.
*   **SQLite:** As a local, file-based relational database for structured data storage.
*   **SQL:** For efficient data extraction and transformation within the database.
*   **Google Cloud Platform (GCP):**
    *   Google Drive API
    *   Google Sheets API
*   **gspread & oauth2client:** Python libraries for interacting with the Google Sheets API.
*   **JupyterLab:** As the primary IDE for development and analysis.

## The Pipeline Explained

The project is structured as a modular ELT (Extract, Load, Transform) pipeline, broken down into separate notebooks for clarity and maintainability:

1.  **`01_create_database.ipynb`**: The **Load** stage. This notebook reads the raw data from a `.csv` file into a pandas DataFrame and loads it into a local SQLite database table (`tennis_data.db`). This simulates a real-world scenario where data resides in a structured database.

2.  **`02_shelton_analysis.ipynb`**: The **Extract** and **Transform** stages. This notebook connects to the SQLite database, uses a SQL query to extract only the relevant data (all matches played by Ben Shelton), and then transforms it using pandas to calculate key performance indicators (KPIs) such as win rates on different surfaces.

3.  **`03_report_uploader.ipynb`**: The **Share** stage. This final notebook takes the summarized data from the analysis stage and automatically uploads it to a specified Google Sheet using the `gspread` library and a Google Cloud Service Account for authentication.

## Getting Started

To run this pipeline locally, follow these steps:

### 1. Prerequisites
*   Python 3.x (preferably via Anaconda)
*   A Google Cloud Platform project with **Google Drive API** and **Google Sheets API** enabled.

### 2. Installation
1.  Clone the repository:
    ```bash
    git clone https://github.com/toothless66622242525/python-sql-spreadsheet-pipeline.git
    cd python-sql-spreadsheet-pipeline
    ```
2.  Install the required Python libraries:
    ```bash
    pip install pandas gspread google-auth-oauthlib
    ```

### 3. Configuration
1.  **Google Credentials:** Follow the [Google Cloud documentation](https://cloud.google.com/iam/docs/service-accounts-create) to create a **Service Account** and download its credentials as a `.json` key file. Place this file in the root of the project directory. **Do not commit this file to Git!**
2.  **Google Sheet:** Create a new, empty Google Sheet. Open your `.json` key file, copy the `client_email` address, and share the Google Sheet with this email, giving it "Editor" permissions.
3.  **Update Code:** In the `03_report_uploader.ipynb` notebook, make sure the name of your Google Sheet in the `gc.open('Your Sheet Name')` line matches the one you created.

### 4. Running the Pipeline

Now just execute the Jupyter notebooks in numerical order: `01_`, then `02_`, then `03_`.

### 5. Keeping the Data Fresh

The `atp_tennis.csv` file included in this repository is current as of **July 29, 2025**.

This pipeline is automated from the CSV file onwards. To get the very latest match results, you will need to manually download the updated dataset from [its source](https://www.kaggle.com/datasets/dissfya/atp-tennis-2000-2023daily-pull) on Kaggle and replace the existing `atp_tennis.csv` file before running the pipeline.

## Final Output

The pipeline generates a clean, easy-to-understand summary of Ben Shelton's performance, automatically delivered to a Google Sheet:

| Surface | Total Matches | Wins | Win Rate (%) |
|---------|---------------|------|------------------|
| Clay    | 32            | 16   | 50.00            |
| Grass   | 23            | 12   | 52.1739130434783 |
| Hard    | 99            | 60   | 61.1650485436893 |

*(Note: Data is based on the provided dataset and reflects values at the time of analysis.)*
