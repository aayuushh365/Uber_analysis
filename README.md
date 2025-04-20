Overview

This repository contains an end-to-end Extract, Transform, Load (ETL) pipeline and exploratory data analysis for New York City Yellow Taxi trip records. The goal is to ingest raw taxi trip data, clean and enrich it, and load it into a dimensional data model for downstream analytics and reporting.

Key components:

Data Dictionary (provided): Definitions for each field in the raw dataset. citeturn0file0

Data Model (uber_data_model.png): Star schema design with fact and dimension tables.

ETL Notebook (uber_pipeline.ipynb): Jupyter notebook implementing the pipeline in Python.

Raw Data (uber_data.csv): Sample slice of Yellow Taxi trip records.

Repository Structure

├── README.md                 # This file
├── uber_data_model.png       # Dimensional model diagram
├── data_dictionary_trip_records_yellow.pdf  # Official field definitions
├── uber_data.csv             # Sample raw trip records
└── uber_pipeline.ipynb       # ETL and analysis pipeline

Data Dictionary

The available fields for each trip record are described in detail in the PDF file:

VendorID: TPEP provider code (1 = Creative Mobile Technologies; 2 = VeriFone)

tpep_pickup_datetime / tpep_dropoff_datetime: Trip start/end timestamps

Passenger_count: Number of passengers (driver-entered)

Trip_distance: Distance (miles) per taximeter

PULocationID / DOLocationID: Pickup/dropoff taxi zone IDs

RateCodeID: Fare rate applied (1 = standard, 2 = JFK, etc.)

store_and_fwd_flag: Whether record was buffered in-vehicle

Payment_type: Payment method (1 = credit card, 2 = cash, etc.)

Fare_amount, Extra, MTA_tax, Improvement_surcharge, Tip_amount, Tolls_amount, Total_amount, Congestion_Surcharge, Airport_fee

For full definitions, see data_dictionary_trip_records_yellow.pdf citeturn0file0.

Dimensional Data Model

The star schema consists of:

Fact_table: one row per trip, measures include fare, tip, tolls, total, etc.

Dimension tables:

datetime_dim: pickup/dropoff timestamps dissected into date, hour, day, month, year, weekday

passenger_count_dim: number of passengers

trip_distance_dim: distance in miles

pickup_location_dim / drop_location_dim: latitude & longitude

ratecode_dim: rate code lookup

payment_type_dim: payment method lookup

Embedded above is the diagram uber_data_model.png illustrating foreign key relationships.

ETL Pipeline

All ETL steps are implemented in uber_pipeline.ipynb:

Extract: Read uber_data.csv into a Pandas DataFrame.

Transform:

Parse and normalize datetime columns.

Generate surrogate keys for dimension tables.

Handle missing or outlier values.

Split out dimension DataFrames and the fact DataFrame.

Load: Write each dimension and fact table to disk or database in CSV format.

Running the Notebook

Install dependencies:

pip install -r requirements.txt  # if provided

Launch Jupyter and open the notebook:

jupyter notebook uber_pipeline.ipynb

Execute each cell sequentially to reproduce the ETL process.

Usage

Use the generated dimension and fact CSVs for analytics in SQL engines (e.g., PostgreSQL, BigQuery).

Extend the pipeline to process full monthly datasets by modifying the input path in the notebook.

Dependencies

Python 3.7+

pandas

numpy

sqlalchemy (optional, if loading into a database)

Jupyter Notebook
