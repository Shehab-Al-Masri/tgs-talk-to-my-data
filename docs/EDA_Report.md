EDA Report -- Olist Dataset

Project: TGS Talk-to-My-Data AI Chat Application

1. EDA Objective

The objective of this Exploratory Data Analysis (EDA) phase is to
evaluate the structure, quality, and readiness of the Olist dataset
before building the Azure Data Factory pipeline and the AI-powered
Talk-to-My-Data application.

The analysis focused on: - Dataset profiling - Schema and data type
validation - Missing value analysis - Duplicate detection - Data
integrity checks - Numeric field analysis - Data visualization to
support cleaning and transformation decisions

2. Dataset Overview

The dataset contains multiple relational CSV files representing
customers, orders, products, payments, reviews, sellers, and geographic
information.

Dataset                                    Rows

olist_geolocation_dataset             1,000,163
olist_order_items_dataset               112,650
olist_order_payments_dataset            103,886
olist_orders_dataset                     99,441
olist_customers_dataset                  99,441
olist_order_reviews_dataset              99,224
olist_products_dataset                   32,951
olist_sellers_dataset                     3,095
product_category_name_translation            71

3. Dataset Size Analysis

Visualization

dataset_size_overview.png

Findings

The geolocation dataset is significantly larger than the remaining
datasets, containing more than one million records.

The transaction-related tables have approximately 100k records, which
makes them suitable for analytical processing.

Pipeline Impact

During Azure Data Factory implementation: - Large tables should be
handled carefully during ingestion. - Storage and transformation
strategies should consider table size. - Partitioning or optimized
loading may be required for large datasets.

4. Schema and Data Type Profiling

Generated outputs:

schema_profile.csv

datatype_report.csv

The profiling identified:

Primary identifiers:

order_id

customer_id

product_id

seller_id

Date fields:

purchase timestamps

delivery timestamps

approval timestamps

Measures:

price

freight value

payment value

review score

5. Missing Values Analysis

Visualization

missing_values_plot.png

Findings

The highest missing values were found in review text fields:

review_comment_title

review_comment_message

This is expected because customers can provide a numerical rating
without writing a comment.

Transformation Decision

Keep review_score because it is complete and valuable for customer
satisfaction analysis.

Treat review text columns separately.

Avoid removing records because missing comments represent normal
user behavior.

Additional missing values exist in:

delivery date fields

product dimension attributes

These fields require business rules during transformation.

6. Duplicate Analysis

Generated output:

duplicates_report.csv

Finding

Most datasets contain no significant duplication.

The geolocation dataset contains a high number of duplicate records
because location information is repeated for the same geographic
identifiers.

Transformation Decision

Remove exact duplicates from the geolocation dataset before loading into
the clean layer.

7. Data Integrity Validation

Relationship checks were performed between key entities:

Examples: - Orders and customers - Order items and products - Order
items and sellers

The dataset relationships are suitable for building an analytical layer
and supporting natural language queries.

8. Translation Table Assessment

product_category_name_translation

This table contains only 71 rows and has a different purpose compared
with the transactional datasets.

It is a reference mapping table used only to translate product category
names.

Recommendation

Do not treat this table as a main analytical dataset.

Recommended approach:

Keep it as an optional reference lookup table.

Use it only during transformation if English category names are
required.

Do not include it in the main fact/dimension modeling as a
standalone business entity.

This keeps the analytical model cleaner and avoids unnecessary
complexity.

9. Order Status Distribution

Visualization

order_status_distribution.png

Findings

The majority of orders are in the delivered status.

Other statuses include: - shipped - canceled - unavailable - invoiced -
processing - created - approved

Project Impact

For the chatbot, order status should be standardized into business
categories.

Example:

Status       Business Meaning

delivered    Completed
shipped      In Progress
processing   In Progress
canceled     Cancelled

This improves natural language understanding.

10. Review Score Distribution

Visualization

review_score_distribution.png

Findings

The majority of reviews have high scores.

Review score distribution:

5 stars: 57,328

4 stars: 19,142

3 stars: 8,179

2 stars: 3,151

1 star: 11,424

Project Impact

Review score is an important field for chatbot questions related to: -
customer satisfaction - average rating - review analysis

11. Payment Value Distribution

Visualization

payment_value_distribution.png

Findings

Payment values show a right-skewed distribution.

Statistics:

Records: 103,886

Average payment value: 154.10

Median payment value: 100.00

Maximum value: 13,664.08

Transformation Considerations

Investigate extreme values before analytical usage.

Validate zero payment values.

Keep payment_value as an important financial measure.

12. EDA Conclusion

The Olist dataset is suitable for building the TGS Talk-to-My-Data
solution.

The dataset provides:

Relational structures

Transactional information

Customer feedback

Financial measures

Required fields for business questions

Main preparation requirements:

Standardize data types

Handle missing values

Remove duplicates

Create business-friendly status mappings

Prepare metadata for AI retrieval

Next steps:

Build Azure Data Factory ingestion pipelines

Create raw and clean data layers

Prepare chatbot metadata

Evaluate the system using benchmark questions