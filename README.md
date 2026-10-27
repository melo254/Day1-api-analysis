# Pesa Pulse Customer Data Analysis

**API Data Analysis Using Python Structures**  


## 1. Project Overview

Pesa Pulse is a fictional Nairobi fintech start-up that receives customer data from a partner through an API. Before planning regional marketing campaigns, the business needs to understand the structure of the data, the number of customers, their locations, and the distribution of selected customer attributes.

This project retrieves sample customer records from the JSONPlaceholder API, explores nested JSON structures, extracts relevant attributes, performs aggregations using plain Python, validates the results using pandas, exports CSV files, and generates charts.

**Data source:** [JSONPlaceholder Users API](https://jsonplaceholder.typicode.com/users)

**Important:** JSONPlaceholder provides mock data for development and learning. The results in this project do not represent actual Pesa Pulse customers.

## 2. Project Objectives

- Retrieve JSON data from an API using the `requests` library.
- Handle unsuccessful HTTP responses, network failures, and invalid JSON.
- Explore nested dictionaries and lists recursively.
- Extract customer attributes into a flat structure.
- Identify unique cities using Python sets.
- Count customers by city using dictionaries and loops.
- Count customers by email top-level domain using a dictionary comprehension.
- Convert the cleaned records into a pandas DataFrame.
- Validate Python aggregation results against pandas.
- Export the cleaned data and city summaries to CSV.
- Visualise customer distributions using Matplotlib.

## 3. Tools and Technologies

- **Python:** Data retrieval, transformation, exploration, and aggregation.
- **Requests:** Making HTTP requests to the API.
- **JSON:** Understanding and working with structured API responses.
- **Dictionaries and lists:** Navigating and transforming nested records.
- **Sets:** Identifying distinct cities and email top-level domains.
- **Dictionary comprehensions:** Aggregating email-domain counts.
- **pandas:** Creating DataFrames, validating results, and exporting CSV files.
- **Matplotlib:** Creating and saving charts.
- **Jupyter Notebook:** Executing the analysis step by step.

## 4. Approach and Methodology

### Step 1: Retrieve customer data

I used the `requests` library to send a GET request to the JSONPlaceholder Users API.

A timeout was configured to prevent the request from waiting indefinitely. The response status was checked using `raise_for_status()`, and the JSON response was converted into Python objects.

Network and response errors were handled with `try/except` so that a failed request would produce a clear message instead of an unhandled traceback.

### Step 2: Explore the nested structure

I implemented a recursive `explore()` function to print every key, its Python type, and its nesting level.

The API response contains dictionaries nested inside other dictionaries. The main nested attributes are:

- `address`: Contains street, suite, city, and zipcode.
- `address.geo`: Contains latitude and longitude.
- `company`: Contains the company name, catchphrase, and business description.

The deepest required attributes are latitude and longitude, accessed through:

- `customer["address"]["geo"]["lat"]`
- `customer["address"]["geo"]["lng"]`

This exploration helped identify the paths required to extract the relevant fields.

### Step 3: Extract and flatten customer attributes

I implemented `extract_attributes()` to transform each nested customer record into a flat dictionary.

The extracted attributes are:

`id`, `name`, `username`, `email`, `email_domain`, `phone`, `city`, `zipcode`, `latitude`, `longitude`, `company`, and `website`.

The email domain is derived from the email address. Latitude and longitude are converted from strings to floating-point numbers so they can be used for numerical analysis and future geographic visualisations.

Missing nested dictionaries are handled using `.get()` and fallback empty dictionaries where appropriate.

### Step 4: Identify unique cities

I used a set comprehension to identify distinct customer cities.

Sets automatically remove duplicate values, making them appropriate for measuring the number of unique cities represented in the dataset.

I compared the number of unique cities with the total number of customer records to determine whether multiple records shared the same city.

### Step 5: Count customers using plain Python

I implemented two aggregation functions:

- `city_counts()`: Uses a dictionary and loop to count customer records per city.
- `email_domain_counts()`: Uses a set comprehension to identify email top-level domains and a dictionary comprehension to count records in each group.

The counts are sorted from most common to least common, with alphabetical ordering used to break ties.

These calculations were completed without using `collections.Counter` or pandas.

### Step 6: Create and validate the DataFrame

I converted the flattened dictionaries into a pandas DataFrame using `pd.DataFrame()`.

I inspected the DataFrame's shape and column data types, then used `value_counts()` to independently calculate customer counts by city.

The pandas results were compared with the plain Python results using assertions. This provided a consistency check between the two approaches.

### Step 7: Export the results

I created an `output/` directory and exported the results using `to_csv(index=False)`.

The index was excluded to avoid adding an unnecessary index column to the output files.

### Step 8: Visualise the distributions

I used Matplotlib to create labelled bar charts showing the distribution of customer records.

The city chart helps communicate how the sample is distributed across the represented cities. An additional chart groups records by email top-level domain.

Because the dataset is small and may contain one customer per city, the city chart can have limited comparative value. The email-domain chart may provide more variation if some domains occur repeatedly.

Neither chart should be treated as evidence of real market demand.

## 5. Findings

The following findings are expected when the API returns its standard 10-record dataset. They should be confirmed against the output of the notebook execution.

| Finding | Expected result |
|---|---|
| Total customer records | 10 |
| Unique cities | 10 |
| DataFrame shape | 10 rows × 13 columns |
| Customer records per city | 1 each |
| Latitude and longitude | Floating-point values |
| Python and pandas city counts | Matching results |

### Interpretation for marketing

The sample contains one record for each represented city. Therefore, there are no repeated cities in the standard dataset, and the city counts alone do not reveal a leading customer market.

The dataset demonstrates how to calculate customer distribution, but it is too small and synthetic to support real campaign targeting or regional budget allocation.

Email top-level domain counts provide an additional way to explore the data. However, email domains are not reliable indicators of customer location or purchasing behaviour.

## 6. Assumptions

- Each record returned by the API represents one customer for the purposes of this exercise.
- The API returns a list of customer dictionaries with the expected structure.
- The `address.city` field represents the customer's recorded city.
- Latitude and longitude are supplied as numeric strings that can be converted to floats.
- Email top-level domains are extracted from valid email addresses.
- The standard API response contains 10 records, although API availability and responses can change.
- The data is synthetic and is used only to demonstrate the analysis workflow.

## 7. Limitations

- The dataset contains mock rather than real customer information.
- Ten records are insufficient for reliable regional marketing decisions.
- A customer's recorded city may not represent their current residence or preferred service location.
- Email domains do not establish geographic location, nationality, or customer value.
- Missing or malformed fields require additional validation in a production system.
- API failures may prevent the analysis from running.
- The charts describe the available sample and cannot establish broader customer trends.

## 8. Project Outputs

After running the notebook, the `output/` directory contains:

| File | Description |
|---|---|
| `customers.csv` | Flattened customer records with 13 columns |
| `city_counts.csv` | Customer counts grouped by city |
| `customer_distribution.png` | Bar chart of customer counts by city |
| `email_domain_distribution.png` | Bar chart of customer counts by email top-level domain |

## 9. How to Run the Project

### Prerequisites

Install Python and Jupyter Notebook or use the Jupyter support in Visual Studio Code.

Install the required libraries:

```bash
python -m pip install requests pandas matplotlib
```

### Execution steps

1. Clone or download the project repository.
2. Open the project folder in VS Code.
3. Open the `.ipynb` notebook.
4. Run the cells in order, beginning with the imports and API request.
5. Inspect the customer records, nested structure, and aggregation results.
6. Confirm that the pandas validation passes.
7. Check the `output/` directory for the generated CSV files and charts.

An active internet connection is required to retrieve the API data.

## 10. Conclusion

This project demonstrates a complete introductory data analysis workflow, from retrieving JSON through an API to exploring nested structures, extracting attributes, performing aggregations, validating results, exporting CSV files, and generating visualisations.

It reinforces the importance of understanding raw data before analysing it and of validating independently calculated results.

For a real fintech application, the next step would be to analyse a larger, authorised customer dataset with validated geographic information, stronger data-quality checks, and appropriate privacy controls before making marketing recommendations.
