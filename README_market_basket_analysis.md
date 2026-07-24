# Market Basket Analysis of E-commerce Orders

## Overview
This Jupyter notebook performs a Market Basket Analysis (MBA) on small business, e-commerce order data to identify frequently co-occurring items and derive association rules. The analysis utilizes the Apriori algorithm to uncover product relationships, which can inform business strategies related to product placement, promotional bundling, and recommendation systems.

## Data Source
The analysis uses a CSV file named `cleaned_orders1.csv`, which contain processed e-commerce order data.

## Notebook Structure

1.  **Data Loading and Initial Inspection**: Loads the `cleaned_orders1.csv` file into a Pandas DataFrame and displays the first few rows.
2.  **More Data Cleaning**: 
    *   Renames the 'Name' column to 'OrderID'.
    *   Fills missing customer names and order totals by grouping on 'OrderID'.
    *   Converts date columns ('Created at', 'Fulfilled at', 'Cancelled at', 'Paid at') to datetime objects.
    *   Renames 'Created at' to 'order_date' and 'Billing Name' to 'billing_name'.
    *   Removes '#' prefix from 'OrderID'.
    *   Removes 'NEW - ', 'RETIRED - ', and 'PREORDER - ' prefixes from 'Lineitem name'.
    *   Filters data to include only orders created in 2024 or later.
3.  **Market Basket Analysis**: 
    *   **Transaction Grouping**: Groups `Lineitem name` by `OrderID` to create a list of items for each transaction.
    *   **One-Hot Encoding**: Uses `mlxtend.preprocessing.TransactionEncoder` to transform the transaction lists into a one-hot encoded DataFrame, suitable for the Apriori algorithm.
    *   **Applying Apriori**: Implements the `apriori` algorithm from `mlxtend.frequent_patterns` to find frequent itemsets based on a specified `min_support` threshold (0.002).
    *   **Generating Association Rules**: Generates association rules from the frequent itemsets using `association_rules` with `metric="lift"` and `min_threshold=1`, then sorts them by lift in descending order.
    *   **Interpreting and Filtering Rules**: Filters the generated association rules based on `confidence` (> 0.1) and `lift` (> 1.0) to focus on strong, meaningful relationships.
4.  **Listing Specific Item Relationships for Business Action**: Iterates through the top filtered rules to print actionable insights, detailing which items are likely to be purchased together, along with their confidence, lift, and support values.
5.  **Summary of Market Basket Analysis**: Provides a brief conclusion on the utility of MBA for product placement, promotional bundles, recommendation systems, and inventory management, highlighting key findings from the filtered rules.

## Key Libraries Used
*   `pandas` for data manipulation and analysis.
*   `numpy` for numerical operations.
*   `mlxtend.preprocessing.TransactionEncoder` for one-hot encoding transactions.
*   `mlxtend.frequent_patterns.apriori` for finding frequent itemsets.
*   `mlxtend.frequent_patterns.association_rules` for generating association rules.


## Key Findings (Example from the notebook)

*   **Strong Complementary Products**: If a customer buys 'April Dishcloth Set of 3 by Geometry', they are highly likely to also buy 'April Kitchen Tea Towel by Geometry' (Confidence: 0.64, Lift: 223.66).
*   **Cross-Selling Opportunities**: Items like 'Power Mist Frosted Mint Hand Sanitizer' and 'Power Mist Pure Lavender Hand Sanitizer' show a strong co-occurrence (Confidence: 0.42, Lift: 69.58).

These insights can be leveraged for various business strategies such as cross-selling, bundling, and targeted promotions to enhance customer experience and increase sales.
