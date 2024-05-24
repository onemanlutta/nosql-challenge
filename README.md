# NoSQL Challenge: UK Food Standards Database

# Eat Safe, Love Project

## Overview

This ETL pipeline project involves setting up and working with a MongoDB NoSQL database to analyze food hygiene ratings in the UK. The project is divided into three parts:

1. **Database and Jupyter Notebook Set Up**
2. **Update the Database**
3. **Exploratory Analysis**

## Prerequisites

- MongoDB installed and running on your local machine.
- Python environment with necessary packages (`pymongo`, `pandas`, `pprint`).
- Jupyter Notebook.

## 1. Project Summary

The Eat Safe, Love project focuses on managing and analyzing restaurant inspection data from the UK using MongoDB and Python. The project involves importing raw data, performing data manipulation and analysis tasks, and presenting insights into restaurant hygiene and ratings.

## 2. Objectives of Each Task/Steps

### Data Import and Setup
- **Import Data**: Import restaurant inspection data from `establishments.json` into MongoDB.
  
### Database Operations
- **Add New Restaurant**: Introduce "Penang Flavours" as a new restaurant in the database.
- **Update BusinessTypeID**: Identify and update the BusinessTypeID for "Restaurant/Cafe/Canteen".

### Data Analysis
- **Query Tasks**: Conduct specific queries to find establishments with hygiene scores of 20, high-rated establishments in London, and top-rated establishments near "Penang Flavours".
- **Data Conversion**: Convert data types and ensure numerical accuracy for coordinates and rating values.
- **Data Presentation**: Utilize Pandas DataFrames to visualize and explore findings.

## 3. Project Files

### Structure

- **Resources/**
  - **establishments.json**: JSON file containing raw data of restaurant inspection records.
- **NoSQL_analysis_starter.ipynb**: Jupyter notebook for performing data manipulation, querying, and analysis.
- **NoSQL_setup_starter.ipynb**: Jupyter notebook for setting up the database, importing data, and performing initial exploration and setup tasks.
- **README.md**: Markdown file containing project overview, objectives, tasks, findings, and disclaimer.


## 4. Methods Used

- **MongoDB**: Database management for storing and querying restaurant inspection data.
- **Python (pymongo)**: Scripting language for data manipulation, querying MongoDB, and data analysis.
- **Pandas**: Data manipulation and analysis library for presenting insights.

## 5. Overview of Findings

The project reveals insights into restaurant hygiene and ratings across different regions in the UK. Key findings include:
- Identification of establishments with specific hygiene scores.
- Analysis of highly-rated establishments in London.
- Top-rated establishments near "Penang Flavours" based on proximity and hygiene scores.

## 6. Disclaimer

The data used in this project is sourced from public records and may be subject to updates or inaccuracies. The findings and analysis are based on the available dataset and should be interpreted with consideration for any limitations or changes in data over time.

# References
UK Food Standards AgencyLinks to an external site. (2022). UK food hygiene rating data API. https://ratings.food.gov.uk/open-data/en-GBLinks to an external site.. Contains public sector information licensed under the Open Government Licence v3.0Links to an external site.
Accessed Sept 9, 2022 and Sept 12, 2022 with the establishment settings as follows: longitude=51.5072, latitude=-0.1276, maxdistancelimit=4567, pagesize=10000, sortoptionkey=distance, pagenumber=(1,2,3,4,5,6,7,8)
