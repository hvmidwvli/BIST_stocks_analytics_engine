# BIST Stocks Analytics & Risk Engine

## Project Overview

This project is a full-stack equity analysis of 30 publicly traded stocks listed on Borsa Istanbul (BIST), designed to evaluate market behavior beyond simple price tracking. Using historical daily data from 2010 to 2025 (depending on sector availability). The goal was to model risk, sector trends, and capital flow beyond simple price tracking.

This project is built with a portable data engine. The Excel workbook uses a dynamic relative path to automatically detect the local directory, meaning you won’t have to manually fix file paths for the 30+ CSV files.

#### How to Run the Project:
1. Download/Clone this repository to your machine.

2. Open `BIST_Analysis.xlsx`:

    (Optional) Unhide and go to the `Data_Dictionary` sheet.

    Click **Data > Refresh All**. The engine will auto-detect your folder path and update the ETL pipelines.

3. Open `BIST_Analysis.pbix`:

    If the dashboard shows a "Data Source Error," go to **File > Options and settings > Data source settings**.

    Click **Change Source** and select the `BIST_Analysis.xlsx` file in your local folder.

    Click Apply Changes to see the live visualizations.

## Data Scope & Standardization

To ensure strict temporal consistency, the analysis utilizes a September 30 (Q3) cut-off. Since Q4 2025 data was unavailable at the time of modeling, I standardized all historical windows to strictly align with this cycle (Q3–Q3). This ensures that every backtested period represents an exact, comparable annual interval without seasonal skew.

Because some stocks are newer than others, the data depth varies by industry:

* **Banking:** 15 Years (2010–2025)
* **Industrials:** 12 Years (2013–2025)
* **Airlines & Transportation:** 10 Years (2015–2025)
* **Consumer & Retail:** 10 Years (2015–2025)
* **Technology:** 7 Years (2018–2025)

The goal was to build a system that acts like an institutional research tool which ingests raw daily data, cleans it through a custom pipeline, and outputs actionable risk/reward insights.

## Technical Architecture

This project was built using a "Fact & Dimension" approach to ensure the data model was scalable and accurate. A directory-based ETL pipeline (`Folder.Files`) that dynamically scans 5 sector folders to ingest 30 individual stock CSVs was implemented.

### Data Engineering (Excel & Power Query)

The backend relies on a custom ETL pipeline built in Power Query (M Language). I consolidated six independent sector datasets into a unified model. Instead of manual merging, I created reusable functions to standardize date formats and handle currency localization across ~80,000 rows.

Crucially, this is where I engineered the relational model. I linked the granular `Master_StockData` (Fact Table) to the `Industry_Map` (Dimension Table) directly within the Excel architecture. This pre-built relationship ensures that all downstream analysis handles sector filtering automatically without needing complex lookups later on.

### Statistical Modeling (Excel)

Before visualization, I used Excel Pivot Tables to run the core statistical tests. I constructed a 30x30 Correlation Matrix to test if diversification strategies were actually valid, or if assets were just moving in lockstep. I also modeled average daily returns, standard deviations, and volume liquidity to quantify the "structural risk" of each sector.

* **Test A:** Sector Performance & Annual Momentum
* **Test B:** Volatility Clustering & Risk Assessment
* **Test C:** Liquidity & Market Participation
* **Test D:** Correlation Matrix (Diversification Check)

To make the data more interactive, I added an industry slicer and a timeline to the Excel interface. These allow instant filtering across different market cycles and sectors and consequently transform the static tables into a dynamic engine for exploration.

### Business Intelligence (Power BI)

The final layer is the interactive dashboard. Since the data model was already structured in Excel, I focused here on high-level analytics. I wrote custom DAX measures to detect specific market signals and visualized them using "quadrant" scatter plots for regime analysis.

* **Test E:** The Golden Cross 
* **Test F:** Maximum Drawdown 
* **Test G:** Risk-Adjusted Returns

## 📂 Repository Structure

* **`BIST_Analysis.xlsx`**: The backend engine. Contains the embedded Power Query (M) logic that iterates through the `data/` directories to ingest and normalize files.
* **`BIST_Analysis.pbix`**: The visual dashboard.
* **`BIST_30_Analytics_Sample_Report.pdf`**: The executive findings. 
* **`assets/`**: Project screenshots.
* **`data/`**: The raw data lake. Organized into 5 sector-specific sub-directories containing the 30 individual stock CSVs.

---
*Designed & Engineered by Hamed Wali Fayez*