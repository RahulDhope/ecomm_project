# ecomm_project

IDMC (Informatica / Intelligent Data Management Cloud) mapping — high level

Steps to create mapping from Oracle → Snowflake:

Source connection: Configure Oracle connection (wallet/tnsnames/jdbc).

Target connection: Configure Snowflake connection (account, user, role, warehouse, database, schema).

Create mappings:

orders → dim_orders (map columns directly; consider SCD Type 2 if orders attributes change)

products → dim_products

aisles → dim_aisles

departments → dim_departments

order_products + lookups to products,orders → fact_order_products (populate department_id, aisle_id from product lookup)

Transformations:

Cast datatypes appropriately (Oracle NUMBER → Snowflake NUMBER/INTEGER, VARCHAR2 → STRING).

Convert reordered numeric to boolean.

Compute order_number string if needed.

Key handling:

Preserve natural keys (e.g., order_id, product_id, user_id).

Optionally generate surrogate keys in Snowflake if your analytics require them.

Performance:

Use bulk load (STAGE + COPY INTO) where possible for large tables.

Partition incremental loads by last_modified or use CDC if available.

Job scheduling:

Create a daily schedule in IDMC (once per day). Add incremental filter WHERE <op_ts> >= :last_run_time for incremental loads.

Monitoring & Alerts:

Configure success/failure email alerts and logging to Snowflake or cloud logging (CloudWatch/Stackdriver).
