STRING PROCESSING FOR DATA CLEANING

A hands-on Python and Pandas data cleaning project focused on text processing, string normalization, category extraction, and data feature preparation on real-world datasets.

PROJECT OVERVIEW
Unstructured and raw datasets often contain formatting inconsistencies, hidden whitespace, messy casings, and embedded special characters. This project demonstrates essential string manipulation techniques to clean, transform, and structure raw text data into analytical-ready datasets.

OBJECTIVES AND DELIVERABLES

Core String Operations: Demonstrate practical usage of strip(), lower(), upper(), replace(), and split().

Reusable Cleaning Function: Develop a modular Python function to handle string inputs, fix spacing issues, and standardize casing.

Feature Normalization:

Clean user names and product titles by removing excess spaces and applying Title Case.

Normalize email addresses into standardized lowercase text.

Split multi-level categorical strings like Electronics|Computers|Laptops to extract primary top-level categories.

Clean formatted currency strings by stripping symbols and commas, then converting them into numeric float data types.

Dataset Export: Export the cleaned DataFrame to a structured CSV file.

TOOLS AND LIBRARIES

Language: Python 3.x

Data Processing: Pandas

Text Operations: Regular Expressions (re)

Environment: Jupyter Notebook

KEY CODE IMPLEMENTATION

Reusable Text Cleaning Function:

import pandas as pd
import re

def clean_text_field(text, case='lower'):
if pd.isna(text) or not isinstance(text, str):
return text

cleaned = text.strip()
cleaned = re.sub(r'\s+', ' ', cleaned)

if case == 'lower':
    cleaned = cleaned.lower()
elif case == 'upper':
    cleaned = cleaned.upper()
elif case == 'title':
    cleaned = cleaned.title()
    
return cleaned
Cleaning Pipeline Applied on Dataset:

df = pd.read_csv('amazon.csv')

df['clean_user_name'] = df['user_name'].apply(lambda x: clean_text_field(str(x).split(',')[0], case='title'))
df['clean_product_name'] = df['product_name'].apply(lambda x: clean_text_field(str(x), case='title'))

df['category_list'] = df['category'].apply(lambda x: [cat.strip().lower() for cat in str(x).split('|')])
df['primary_category'] = df['category_list'].apply(lambda x: x[0] if len(x) > 0 else '')

df['clean_discounted_price'] = df['discounted_price'].str.replace('₹', '').str.replace(',', '').str.strip().astype(float)
df['clean_actual_price'] = df['actual_price'].str.replace('₹', '').str.replace(',', '').str.strip().astype(float)

df.to_csv('cleaned_amazon_dataset.csv', index=False)

HOW TO RUN

Clone the repository to your local machine.

Navigate to the project directory.

Place amazon.csv in the root folder.

Launch Jupyter Notebook and execute the code cells sequentially.
