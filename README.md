# NoSQL Challenge: UK Food Standards Database

An ETL pipeline extracting data from json file format to MongoDB NoSQL in Python for the purpose of modeling data for an Eat Safe, Love, food magazine. The aim is to evaluate some ratings data in UK from the UK Food Standards Agency to help their journalists and critics decide where to focus future articles.

## Overview

This project involves setting up and working with a MongoDB NoSQL database to analyze food hygiene ratings in the UK. The project is divided into three parts:

1. **Database and Jupyter Notebook Set Up**
2. **Update the Database**
3. **Exploratory Analysis**

## Prerequisites

- MongoDB installed and running on your local machine.
- Python environment with necessary packages (`pymongo`, `pandas`, `pprint`).
- Jupyter Notebook for running the provided starter files.

# The Task
## Getting Started

1. **Clone the Repository**:
    ```sh
    git clone https://github.com/yourusername/nosql-challenge.git
    cd nosql-challenge
    ```

2. **Download the Resources**:
    - Place `establishments.json` in the `Resources` folder.

3. **Import Data into MongoDB**:
    - Use the following command to import the data:
    ```sh
    mongoimport --db uk_food --collection establishments --drop --file Resources/establishments.json --jsonArray
    ```

4. **Open Jupyter Notebook**:
    - Start Jupyter Notebook and open `NoSQL_setup_starter.ipynb` and `NoSQL_analysis_starter.ipynb`.

## Part 1: Database and Jupyter Notebook Set Up

### Steps

1. **Import Libraries**:
    ```python
    from pymongo import MongoClient
    from pprint import pprint
    ```

2. **Create Mongo Client and Verify Database**:
    ```python
    client = MongoClient('mongodb://localhost:27017/')
    db = client['uk_food']
    collection = db['establishments']
    ```

3. **Check Database and Collection**:
    ```python
    print(client.list_database_names())
    print(db.list_collection_names())
    pprint(collection.find_one())
    ```

## Part 2: Update the Database

### Steps

1. **Add New Restaurant**:
    ```python
    new_restaurant = {
        "BusinessName": "Penang Flavours",
        "BusinessType": "Restaurant/Cafe/Canteen",
        "BusinessTypeID": "",
        "AddressLine1": "Penang Flavours",
        "AddressLine2": "146A Plumstead Rd",
        "AddressLine3": "London",
        "AddressLine4": "",
        "PostCode": "SE18 7DY",
        "Phone": "",
        "LocalAuthorityCode": "511",
        "LocalAuthorityName": "Greenwich",
        "LocalAuthorityWebSite": "http://www.royalgreenwich.gov.uk",
        "LocalAuthorityEmailAddress": "health@royalgreenwich.gov.uk",
        "scores": {
            "Hygiene": "",
            "Structural": "",
            "ConfidenceInManagement": ""
        },
        "SchemeType": "FHRS",
        "geocode": {
            "longitude": "0.08384000",
            "latitude": "51.49014200"
        },
        "RightToReply": "",
        "Distance": 4623.9723280747176,
        "NewRatingPending": True
    }
    collection.insert_one(new_restaurant)
    ```

2. **Update BusinessTypeID**:
    ```python
    result = collection.find_one({"BusinessType": "Restaurant/Cafe/Canteen"}, {"BusinessTypeID": 1, "BusinessType": 1})
    business_type_id = result['BusinessTypeID']
    collection.update_one(
        {"BusinessName": "Penang Flavours"},
        {"$set": {"BusinessTypeID": business_type_id}}
    )
    ```

3. **Remove Dover Establishments**:
    ```python
    dover_count = collection.count_documents({"LocalAuthorityName": "Dover"})
    print(dover_count)
    collection.delete_many({"LocalAuthorityName": "Dover"})
    new_dover_count = collection.count_documents({"LocalAuthorityName": "Dover"})
    print(new_dover_count)
    ```

4. **Convert Data Types**:
    ```python
    collection.update_many({}, [{'$set': {'geocode.latitude': {'$toDouble': '$geocode.latitude'}, 'geocode.longitude': {'$toDouble': '$geocode.longitude'}}}])
    collection.update_many({}, [{'$set': {'RatingValue': {'$toInt': '$RatingValue'}}}], upsert=False)
    ```

## Part 3: Exploratory Analysis

### Steps

1. **Hygiene Score Equal to 20**:
    ```python
    query = {"scores.Hygiene": 20}
    count = collection.count_documents(query)
    print(count)
    result = collection.find(query)
    df = pd.DataFrame(result)
    print(df.head(10))
    ```

2. **RatingValue ≥ 4 in London**:
    ```python
    query = {"LocalAuthorityName": {"$regex": "London"}, "RatingValue": {"$gte": 4}}
    count = collection.count_documents(query)
    print(count)
    result = collection.find(query)
    df = pd.DataFrame(result)
    print(df.head(10))
    ```

3. **Top 5 Establishments Near "Penang Flavours"**:
    ```python
    latitude = 51.49014200
    longitude = 0.08384000
    query = {
        "geocode.latitude": {"$gte": latitude - 0.01, "$lte": latitude + 0.01},
        "geocode.longitude": {"$gte": longitude - 0.01, "$lte": longitude + 0.01},
        "RatingValue": 5
    }
    result = collection.find(query).sort("scores.Hygiene", 1).limit(5)
    df = pd.DataFrame(result)
    print(df.head(10))
    ```

4. **Hygiene Score of 0 by Local Authority**:
    ```python
    pipeline = [
        {"$match": {"scores.Hygiene": 0}},
        {"$group": {"_id": "$LocalAuthorityName", "count": {"$sum": 1}}},
        {"$sort": {"count": -1}}
    ]
    result = collection.aggregate(pipeline)
    df = pd.DataFrame(list(result))
    print(df.head(10))
    ```

# References
UK Food Standards AgencyLinks to an external site. (2022). UK food hygiene rating data API. https://ratings.food.gov.uk/open-data/en-GBLinks to an external site.. Contains public sector information licensed under the Open Government Licence v3.0Links to an external site.
Accessed Sept 9, 2022 and Sept 12, 2022 with the establishment settings as follows: longitude=51.5072, latitude=-0.1276, maxdistancelimit=4567, pagesize=10000, sortoptionkey=distance, pagenumber=(1,2,3,4,5,6,7,8)
