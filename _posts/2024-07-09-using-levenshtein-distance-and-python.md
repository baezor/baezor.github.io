---
layout: post
title: Using Levenshtein Distance and Python to Find Similar Names and Avoid Duplicates
date: 2024-07-09 00:00:00 -0500
description: A Python script to find similar names using the Levenshtein distance algorithm.
categories: python algorithms prepain
author: Angel Baez
---

The other day, a colleague from our [Prepa IN](https://prepain.mx/) team needed help with a tough task involving a hefty report. He had to spot rows with similar names, some of which were misspelled but actually referred to the same person. Like, there would be _"Angel Baez"_ in one row and _"Angel Báez"_ in another. Identifying these differences was hard, especially with 3000 names to go through - definitely not a one-person job.

To tackle this, I whipped up a Python script that uses the Levenshtein distance to find similar rows by measuring the likeness between two elements. The script uses the [Levenshtein](https://pypi.org/project/python-Levenshtein/), [Pandas](https://pandas.pydata.org), and [FuzzyWuzzy](https://pypi.org/project/fuzzywuzzy/) libraries.

Here's what I did:

```python
import pandas as pd
from fuzzywuzzy import fuzz

# Load the CSV file into a DataFrame
df = pd.read_csv('db.csv')

# Ensure all values are treated as strings and handle NaN values
df['Apellido Paterno'] = df['Apellido Paterno'].fillna('').astype(str)
df['Apellido Materno'] = df['Apellido Materno'].fillna('').astype(str)
df['Nombre(s)'] = df['Nombre(s)'].fillna('').astype(str)

# Combine the columns to create a full name
df['full_name'] = df['Apellido Paterno'] + ' ' + \
    df['Apellido Materno'] + ' ' + df['Nombre(s)']

def find_similar_rows(dataframe, column_name, threshold=80):
    similar_pairs = []
    checked = set()

    for i in range(len(dataframe)):
        for j in range(i + 1, len(dataframe)):
            if (i, j) not in checked:
                name1 = dataframe.iloc[i][column_name]
                name2 = dataframe.iloc[j][column_name]
                if name1 and name2:  # Ensure neither name is empty
                    ratio = fuzz.ratio(name1, name2)
                    if ratio > threshold:
                        similar_pairs.append((name1, name2, ratio))
                checked.add((i, j))
    return similar_pairs


# Find similar rows with a similarity threshold of 80%
similar_rows = find_similar_rows(df, 'full_name')

# Display the results
for row in similar_rows:
    print(f"Similar pair: {row[0]} and {
          row[1]} with similarity ratio: {row[2]}")

# Optionally, save the similar pairs to a new CSV file
similar_df = pd.DataFrame(similar_rows, columns=[
                          'Name1', 'Name2', 'Similarity'])
similar_df.to_csv('similar_rows.csv', index=False)

```

I hope this script helps you as much as it did for us. It's a great tool to have in your arsenal when dealing with data cleaning tasks. If you have any questions or suggestions, feel free to reach out to me. I'm always happy to help!
