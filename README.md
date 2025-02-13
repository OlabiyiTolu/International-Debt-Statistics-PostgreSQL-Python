# The World Bank's International Debt Data Analysis

![GitHub](https://img.shields.io/badge/Language-SQL-blue) 
![GitHub](https://img.shields.io/badge/Database-PostgreSQL-green) 
![GitHub](https://img.shields.io/badge/Analysis-Data%20Visualization-orange)

This repository contains an analysis of international debt data collected by The World Bank. The dataset includes information about the amount of debt (in USD) owed by developing countries across various debt indicators. The analysis aims to answer key questions such as:

- What is the total amount of debt owed by the countries listed in the dataset?
- Which country has the highest amount of debt, and what is that amount?
- What is the average amount of debt owed by countries across different debt indicators?

## Table of Contents

1. [Introduction](#introduction)
2. [Connecting to the Database](#connecting-to-the-database)
3. [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
   - [First Look at the Data](#first-look-at-the-data)
   - [Number of Distinct Countries](#number-of-distinct-countries)
   - [Distinct Debt Indicators](#distinct-debt-indicators)
4. [Total Debt Analysis](#total-debt-analysis)
   - [Total Amount of Debt Owed](#total-amount-of-debt-owed)
   - [Country with the Highest Debt](#country-with-the-highest-debt)
5. [Average Debt Analysis](#average-debt-analysis)
   - [Average Debt Across Indicators](#average-debt-across-indicators)
   - [Highest Amount of Principal Repayments](#highest-amount-of-principal-repayments)
6. [Most Common Debt Indicator](#most-common-debt-indicator)
7. [Conclusion](#conclusion)
8. [Installation](#installation)
9. [Usage](#usage)
10. [Contributing](#contributing)
11. [License](#license)

## Introduction

Debt is a critical factor in understanding a country's economic health. This project analyzes international debt data provided by The World Bank to uncover insights into the debt profiles of developing countries. The analysis is conducted using SQL queries on a PostgreSQL database.

## Connecting to the Database

The first step in the analysis is connecting to the `international_debt` database and querying the `international_debt` table.

```sql
%%sql
postgresql:///international_debt
    
SELECT * 
FROM international_debt
LIMIT 10;
```

## Exploratory Data Analysis (EDA)

### First Look at the Data

The dataset includes columns such as `country_name`, `country_code`, `indicator_name`, `indicator_code`, and `debt`. The first ten rows provide a snapshot of the data.

```sql
%%sql
SELECT * 
FROM international_debt
LIMIT 10;
```

### Number of Distinct Countries

To understand the scope of the dataset, we count the number of unique countries.

```sql
%%sql
SELECT COUNT(DISTINCT(country_name)) AS total_distinct_countries
FROM international_debt;
```

### Distinct Debt Indicators

The dataset includes various debt indicators. We identify the unique indicators to understand the categories of debt.

```sql
%%sql
SELECT DISTINCT(indicator_code) AS distinct_debt_indicators
FROM international_debt
ORDER BY 1;
```

## Total Debt Analysis

### Total Amount of Debt Owed

We calculate the total amount of debt owed by all countries in the dataset.

```sql
%%sql
SELECT 
    ROUND(SUM(debt)/1000000, 2) AS total_debt
FROM international_debt;
```

### Country with the Highest Debt

We identify the country with the highest total debt and the corresponding amount.

```sql
%%sql
SELECT 
    country_name, 
    SUM(debt) AS total_debt
FROM international_debt
GROUP BY country_name
ORDER BY total_debt DESC
LIMIT 1;
```

## Average Debt Analysis

### Average Debt Across Indicators

We calculate the average amount of debt owed by countries across different debt indicators.

```sql
%%sql
SELECT 
    indicator_code  AS debt_indicator,
    indicator_name,
    AVG(debt) AS average_debt
FROM international_debt
GROUP BY debt_indicator, indicator_name
ORDER BY average_debt DESC
LIMIT 10;
```

### Highest Amount of Principal Repayments

We identify the country with the highest amount of debt in the long-term debt category.

```sql
%%sql
SELECT 
    country_name, 
    indicator_name
FROM international_debt
WHERE debt = (SELECT 
                 MAX(debt)
             FROM international_debt
             WHERE indicator_code LIKE 'DT.AMT.DLXF.CD');
```

## Most Common Debt Indicator

We determine the most common debt indicator among the countries.

```sql
%%sql
SELECT 
    indicator_code, 
    COUNT(indicator_code) AS indicator_count
FROM international_debt
GROUP BY indicator_code
ORDER BY indicator_count DESC, indicator_code DESC
LIMIT 20;
```

## Conclusion

This analysis provides a comprehensive overview of international debt data, highlighting key statistics and trends. The findings offer valuable insights into the economic conditions of developing countries and their debt profiles.

## Installation

To run this analysis locally, you need to have PostgreSQL installed and set up the `international_debt` database with the provided dataset.

## Usage

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/International_Debt_Analysis.git
   ```
2. Navigate to the project directory:
   ```bash
   cd International_Debt_Analysis
   ```
3. Set up the PostgreSQL database and import the dataset.
4. Run the SQL queries in your preferred SQL client or Jupyter notebook with SQL magic commands.

## Contributing

Contributions are welcome! Please fork the repository and create a pull request with your changes.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

For any questions or feedback, please open an issue or contact the repository owner.
