# MySQL Persian Calendar for Metabase

A Metabase model that converts Gregorian dates to the Persian (Jalali) calendar system using MySQL, providing additional calendar features like seasons, week start dates, and Persian month names.

## Overview
This model creates a persistent calendar table in Metabase that converts Gregorian dates to Persian (Jalali) calendar dates. It's designed to be used as a model that other queries can join with to get Persian date information.

## Setup in Metabase

1. Create New Model:
   - Go to New > Model in Metabase
   - Select "Native query" as the model type
   - Give your model a descriptive name (e.g., "Persian Calendar")

2. Add the SQL Code:
   - Copy the entire SQL code provided in `mysql-persian-calendar.sql`
   - Paste it into the query editor in Metabase

3. Save:
   - Click "Save" to create your model
   - Note the model ID from the URL (you'll need this for queries)

4. Test Your Model:
   - Go to New > SQL query
   - Run a simple query to verify the installation
   - Use the test query provided in the Testing section

## Features
- Converts Gregorian dates to Persian dates
- Handles leap years in both calendars
- Provides month names in Persian (فارسی)
- Includes season information
- Provides Persian week start date (Saturday to Friday)
- Configurable date range based on your needs

## MySQL-Specific Considerations

### Recursive CTE Limitations
MySQL imposes a limit on recursive CTE iterations through the `cte_max_recursion_depth` setting (default is often 1000). To work around this:

1. **Default Configuration**:
   The provided code uses `2023-03-21` as the start date to avoid hitting limits with default MySQL settings.

2. **For Longer Date Ranges**:
   
   **Option 1**: Increase CTE recursion depth (requires admin privileges)
   ```sql
   SET SESSION cte_max_recursion_depth = 3650; -- For ~10 years range
   ```
   
   **Option 2**: Create a physical table instead of on-the-fly generation
   ```sql
   CREATE TABLE persian_calendar AS
   WITH RECURSIVE /* [rest of the query] */
   ```

## Usage Examples

> Note: The following examples use placeholder table names (`your_table`, `transactions`, `events`). 
> Replace these with your actual table names in Metabase.

### Basic Date Conversion
```sql
SELECT 
    t.created_at,
    CONCAT(pc.persian_year, '/', 
           LPAD(pc.persian_month, 2, '0'), '/', 
           LPAD(pc.persian_day, 2, '0')) as persian_date
FROM your_table t
JOIN {{#YOUR_MODEL_ID}} pc ON DATE(t.created_at) = pc.day_start;
```

### Monthly Reports
```sql
-- Count records by Persian month
SELECT 
    pc.persian_year,
    pc.persian_month,
    pc.persian_month_name,
    COUNT(*) as record_count
FROM your_table t
JOIN {{#YOUR_MODEL_ID}} pc ON DATE(t.created_at) = pc.day_start
GROUP BY 
    pc.persian_year,
    pc.persian_month,
    pc.persian_month_name
ORDER BY 
    pc.persian_year,
    pc.persian_month;
```

### Seasonal Analysis
```sql
-- Analyze data by season
SELECT 
    pc.persian_year,
    pc.persian_season,
    pc.persian_season_number,
    AVG(t.amount) as avg_amount,
    COUNT(*) as transaction_count
FROM transactions t
JOIN {{#YOUR_MODEL_ID}} pc ON DATE(t.created_at) = pc.day_start
GROUP BY 
    pc.persian_year,
    pc.persian_season,
    pc.persian_season_number
ORDER BY 
    pc.persian_year,
    pc.persian_season_number;
```

### Weekly Trends
```sql
-- Weekly data analysis
SELECT 
    pc.persian_year,
    pc.persian_month,
    pc.persian_week_start_date,
    COUNT(*) as weekly_count
FROM events t
JOIN {{#YOUR_MODEL_ID}} pc ON DATE(t.event_date) = pc.day_start
GROUP BY 
    pc.persian_year,
    pc.persian_month,
    pc.persian_week_start_date
ORDER BY 
    pc.persian_year,
    pc.persian_month,
    pc.persian_week_start_date;
```

### Date Filtering
```sql
-- Filter data for a specific Persian month
SELECT *
FROM your_table t
JOIN {{#YOUR_MODEL_ID}} pc ON DATE(t.created_at) = pc.day_start
WHERE 
    pc.persian_year = 1402 
    AND pc.persian_month = 6  -- Shahrivar
    AND pc.persian_day BETWEEN 1 AND 15;  -- First half of month
```

## Output Columns
| Column Name | Type | Description |
|------------|------|-------------|
| date | date | Original Gregorian date |
| day_start | timestamp | Start of the day (truncated timestamp) |
| persian_year | integer | Year in Persian calendar |
| persian_month | integer | Month number in Persian calendar (1-12) |
| persian_day | integer | Day of month in Persian calendar |
| persian_month_name | text | Persian month name in Persian script |
| persian_season | text | Season name in Persian script |
| persian_season_number | integer | Season number (1-4) |
| persian_week_start_date | date | Start date of the Persian week (Saturday) |

## Limitations
- Default date range:
  - Starts from March 21, 2023 (1402/01/01)
  - Ends 7 days after the current date
  - You can modify these in the `date_range` CTE:
    ```sql
    WITH RECURSIVE date_range AS (
      SELECT CAST('2023-03-21' AS DATE) as date  -- Change this to your desired start date
      UNION ALL
      SELECT DATE_ADD(date, INTERVAL 1 DAY)
      FROM date_range
      WHERE date < DATE_ADD(CURDATE(), INTERVAL 7 DAY)  -- Modify based on your needs
    )
    ```
- MySQL recursive CTE depth limitation (see MySQL-Specific Considerations section)
- Persian text requires proper UTF-8 encoding support

## Finding Your Model ID
After creating the model in Metabase:
1. Open the model
2. Look at the URL in your browser
3. The number after `/model/` is your model ID
4. Use this ID in your queries like `{{#YOUR_MODEL_ID}}`

## Testing
You can test the model with the following query:
```sql
WITH test_dates AS (
    SELECT
      test_date,
      expected_year,
      expected_month,
      expected_day,
      description
    FROM (
      SELECT 
        CAST('2024-03-19' AS DATE) AS test_date, 1402 AS expected_year, 12 AS expected_month, 29 AS expected_day, 'End of year 1402' AS description
      UNION ALL
      SELECT 
        CAST('2024-03-20' AS DATE) AS test_date, 1403 AS expected_year, 1 AS expected_month, 1 AS expected_day, 'Start of year 1403' AS description
      UNION ALL
      SELECT
        CAST('2024-12-24' AS DATE) AS test_date, 1403 AS expected_year, 10 AS expected_month, 4 AS expected_day, 'Sample winter day' AS description
    ) AS test_data
)
SELECT
    t.test_date,
    t.description,
    t.expected_year, 
    t.expected_month,
    t.expected_day,
    pc.persian_year AS calculated_year,
    pc.persian_month AS calculated_month,
    pc.persian_day AS calculated_day
FROM test_dates t
JOIN {{#YOUR_MODEL_ID}} pc ON t.test_date = pc.date;
```

## Troubleshooting

### "Recursive query aborted after X iterations"
This error occurs when your date range exceeds MySQL's recursive CTE limit. Solutions:
1. Use a more recent start date
2. Increase `cte_max_recursion_depth` setting (requires admin privileges)
3. Create a physical table instead

## Credits

- **SQL Development**: Navid Behrangi
- **Project Repository**: [https://github.com/navidb/metabase-persian-calendar](https://github.com/navidb/metabase-persian-calendar)
- **License**: MIT

**If you find this project useful, please consider giving it a star on GitHub.**

For issues, suggestions, or contributions, please visit the GitHub repository.
