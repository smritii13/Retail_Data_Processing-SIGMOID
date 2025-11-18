# Retail_Data_Processing-SIGMOID

1. Vision and Approach
Vision: Our vision is to transform raw, high-volume retail sales data into an actionable customer segmentation tool. The ultimate goal is to identify and segment high-value customers, enabling the business to run effective, personalized promotions.
Approach: We will implement a robust, automated data pipeline that handles the complete data lifecycle. Our approach is twofold:
Data Foundation: First, we establish a "single source of truth" by ingesting all data from CSVs, enforcing strict data quality rules , and segregating all bad data for review.
Customer Intelligence: Second, we build analytics on top of this clean data to accurately calculate loyalty points and then apply RFM (Recency, Frequency, Monetary) analysis to segment customers into "High-Spenders" and "At-Risk" groups.

2. Technology Used
We selected a modern, flexible, and open-source-friendly tech stack as recommended by the hackathon guidelines:
Data Generation: A popular GenAI LLM (like Gemini or ChatGPT) to create realistic, interconnected datasets for sales, customers, and products.
Processing & ETL: Python, using libraries like Pandas for data manipulation and data quality checks, and SQLAlchemy to connect to our database.
Database: PostgreSQL (or MySQL) as our primary database to store all clean relational data and the segregated bad data.
Diagramming: Mermaid (or PowerPoint) to create the required solution architecture and ERD diagrams.
Version Control: Git, with all code and design artifacts stored in a central repository for evaluation.

3. Components
Our solution is broken down into several key components:
Data Generator: A set of prompts for the LLM to create 7 days of simulated sales data, customer profiles, and product catalogs.
Ingestion & DQ Engine (Python Script): This is the core of our solution. It connects to the CSVs, validates each row against our defined business rules (e.g., valid foreign keys, positive amounts), and routes data.
Core Database (PostgreSQL):
Clean Data Tables: The 7 tables defined in the document (e.g., customer_details, store_sales_header, etc.) .
Rejects Table: A custom table (rejected_records) that stores any data that fails validation, along with a reason for the rejection.
Analytics Engine (Python/SQL Script): A script that runs after ingestion. It queries the clean data to calculate loyalty points, update customer balances, compute RFM scores, and finally update the segment_id in the customer_details table.
Design Artifacts:
Solution_Architecture.png: A diagram showing the data flow.
ERD.png: A diagram showing the data model.

4. How Do We Use It
The solution runs in a simple, two-step process:
Run the Ingestion Pipeline: The user executes the main Python script (e.g., python ingest.py). This script automatically reads all source CSVs, performs all data quality checks, loads the clean data into the PostgreSQL database, and populates the rejected_records table.
Run the Analytics Pipeline: The user executes the analytics script (e.g., python segment.py). This script connects to the clean database, calculates all RFM and loyalty metrics, and updates the customer_details table with the new total_loyalty_points and segment_id for each customer.
The business can then query the customer_details table to get a list of all "High-Spenders" or "At-Risk" customers for their marketing campaigns.

5. Intended Design / Pipelines
(Here, you would show your Solution Architecture Diagram.)
Our pipeline is designed as a linear data flow:
Source: Raw CSV files (simulated by the LLM) representing 7 days of sales.
Processing (Python): A single Python script orchestrates the entire process.
Ingest: Reads CSVs into memory.
Validate: Applies data quality rules.
Load:
Clean data is loaded into the target PostgreSQL tables.
Bad data is loaded into the rejected_records table.
Storage (PostgreSQL): The database holds the clean, structured data.
Analysis (SQL/Python): A second script runs on the database to perform calculations (RFM, Loyalty).
Output: The customer_details table is updated with the final loyalty and segment information, making it ready for use.

6. How Data Model Will Look Like
(Here, you would show your Entity-Relationship Diagram (ERD).)
Our data model is a classic retail schema based exactly on the 7 entities provided in the document . The key relationships are:
A customer_details record can have many store_sales_header records.
A stores record can have many store_sales_header records.
A store_sales_header (a single transaction) can have many store_sales_line_items (the individual products).
A products record can be on many store_sales_line_items.
A promotion_details record can be applied to many store_sales_line_items.
In addition to these, we have created one new table as required:
rejected_records: This table stands alone and is not related to the others. It stores the raw data that failed validation and a text description of the error.

