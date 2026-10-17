/. Malformed rows are logged instead of failing the table.

Output:

bronze/sales/Address/year=2026/month=10/day=16/Address.parquet   ← real column names + load_date, source_file
Part 6 – Bronze → Azure SQL
Step 6.1 – ForEachsqlTable

Another ForEach, connected ForEachTable → On success → it (use On completion with the optional try-catch below).

Setting	Value
Sequential	✘ Unticked
Batch count	2 (gentler on the free-tier database)
Items	@activity('Lookup_metadata').output.value
Step 6.2 – Truncate_table_Script (inside ForEachsqlTable)

General → Script

Setting	Value
General → Retry	2, interval 60 s (handles auto-pause wake-up)
Linked service	LS_AzureSQL
DBName	@item().sqlserverdatabase
Script type	NonQuery
Script (Add dynamic content)
