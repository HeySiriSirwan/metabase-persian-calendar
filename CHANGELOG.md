# Changelog
All notable changes to Persian Calendar Model for Metabase will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.2.0] - 2025-02-19

This release delivers on the multi-database vision introduced in version **1.1.0** by adding full **MySQL support**, refining the **PostgreSQL implementation**, and restructuring the project for better maintainability. The Persian Calendar Model is now more accessible across different database systems while ensuring consistency and accuracy.

### Added
- **MySQL Support**: Introduced a MySQL implementation, making this the first step toward broader database compatibility.
- **Database-Specific Documentation**: Added separate `README.md` files for both PostgreSQL and MySQL, providing clear setup and usage instructions.
- **New Project Structure**: Organized the repository into dedicated directories for each supported database system, improving clarity and maintainability.

### Changed
- **Refined PostgreSQL Date Handling**: Updated the implementation to return `date` instead of `timestamp`, ensuring consistency across database engines.
- **Updated Documentation**: Revised all documentation to reflect the new multi-database architecture and ensure clear guidance for users.

### Fixed
- **Resolved PostgreSQL Timestamp Display Issue**: Ensured correct formatting and handling of dates to avoid inconsistencies.

### Summary
This update marks a significant milestone in the evolution of the Persian Calendar Model, expanding its reach beyond PostgreSQL by introducing **full MySQL support**. With a structured project layout, refined date handling, and improved documentation, this release enhances usability and ensures a seamless experience across different **Metabase** installations.

## [1.1.0] - 2025-01-31

### Added
- Restructured to support multiple databases, starting with PostgreSQL
- Persian week start date (Saturday) calculation for accurate weekly reporting

### Changed  
- Season calculation method from day-of-year based to Persian month-based for simpler and more intuitive calculations

### Removed
- Deprecated `persian_week_number` in favor of `persian_week_start_date`

### Fixed
- Corrected Persian week boundaries to properly start from Saturday

This release focuses on improved accuracy in season calculations and better weekly reporting with explicit Persian calendar week boundaries, while laying the groundwork for supporting additional database systems.

## [1.0.0] - 2024-12-26

### Added
- Persian to Gregorian date conversion
- Persian month names (فارسی)
- Season calculation based on day of year
- Week number calculation
- Configurable date range with defaults:
 - Start: March 21, 2022 (1401/01/01)
 - End: Current date + 7 days
- Support for leap years in both calendars
- Example queries for common use cases:
 - Basic date conversion
 - Monthly reports
 - Seasonal analysis
 - Weekly trends
 - Date filtering
- Detailed documentation and usage guide
