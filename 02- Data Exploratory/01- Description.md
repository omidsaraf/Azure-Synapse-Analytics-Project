
## Phase 1 - Discovery

- **Review existing files**: 
  - Check file format, data type, and whether they are partitioned or not.
- **Find the most efficient and cost-effective way to fetch the data**.
- **Input files are usually in CSV format**.

### Determine Appropriate Compute and Storage Solutions:
- **Identify total number of records in source data**.
- **Identify record counts per day, week, or month** to project data growth.

### Check Data Quality:
- **Duplicates**: Identify and remove them.
- **Missing values**: Detect and handle appropriately.
- **Invalid information**: 
  - Approach the data supplier to fix issues.
  - Look for alternative data sources.
  - Apply manual transformations if necessary.

### Join Datasets from Multiple Sources:
- Ensure appropriate keys exist for joining datasets.

### Ensure Data Provides Business Value:
- **Right data items (columns)**: Verify the presence of necessary columns.
- **Perform necessary transformations**.
- **Aggregations**: Apply as needed.
- **Additional requirements**: Address any other specific business needs.

### Practical Steps:
- **Check for duplicates in key columns**.
- **Identify missing data values**.
- **Look for unexpected values in columns**.
- **Ensure sufficient information is available to join datasets**.
- **Write SQL to get summary information for business decisions**.
- **Perform transformations as needed**.

---

Feel free to adjust or add any specific details that are unique to your project. Is there anything else you'd like to include?
