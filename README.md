# Persian Calendar Model for Metabase

A Metabase model that converts Gregorian dates to the Persian (Jalali) calendar system, providing additional calendar features like seasons, week start dates, and Persian month names.

## Why This Project Was Developed?
In the world of **product management** and data analytics, **misalignment between analytical infrastructure and business realities** can lead to poor decision-making. A common challenge in business reporting is aligning analytical systems with real-world operational calendars. Many businesses operate on the **Jalali (Shamsi/Persian) calendar system**, Many businesses operate on the **Jalali (Shamsi/Persian) calendar system**, yet most data analytics tools primarily support the **Gregorian calendar** only.

As a Product Leader working at the intersection of business strategy and data-driven decision-making, I personally faced this challenge when trying to align analytical reports with real-world business cycles. Despite searching for a solution, I found that the **Metabase community** had raised this issue but no concrete solution was available. Realizing that this was a shared challenge among product professionals, I decided to develop a comprehensive solution that everyone could benefit from.

## Overview
This model creates a persistent calendar table in Metabase that converts Gregorian dates to Persian (Jalali) calendar dates. It's designed to be used as a model that other queries can join with to get Persian date information.

## Database Support
Currently available for:
- PostgreSQL: See [PostgreSQL Model](/PostgreSQL%20Model/)
- MySQL: See [MySQL Model](/MySQL%20Model/)

## Changelog
For a detailed list of changes, new features, and improvements, see the [CHANGELOG.md](/CHANGELOG.md) file.

## Quick Start
1. Choose your database implementation from the supported databases above
2. Follow the database-specific setup guide
3. Create a new model in Metabase using the provided SQL code
4. Start using Persian dates in your queries and dashboards

## Features
- Converts Gregorian dates to Persian dates
- Handles leap years in both calendars
- Provides month names in Persian (فارسی)
- Includes season information
- Provides Persian week start date (Saturday to Friday)
- Configurable date range based on your needs

## Example Use Cases
- Convert Gregorian dates to Persian dates
- Generate monthly reports based on Persian calendar
- Analyze seasonal trends using Persian seasons
- Create weekly reports aligned with Persian weeks (Saturday to Friday)
- Filter data using Persian date components

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



## Author
Hi! I'm Navid Behrangi, a Product Leader focused on building data-driven solutions that solve real business challenges.

- 🌐 Website: [navidbehrangi.com](https://www.navidbehrangi.com/)
- 💼 LinkedIn: [linkedin.com/in/navidbehrangi](https://www.linkedin.com/in/navidbehrangi/)

Feel free to reach out if you have questions about the project or want to collaborate!

## Support
If you find this project useful, please consider:
- Giving it a ⭐️star on GitHub
- Contributing improvements
- Sharing with others who might benefit

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
