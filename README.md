## Assisted with Chatgpt

# Employee Data Engineering and Exploratory Data Analysis

## Project Overview

This project demonstrates an end-to-end data engineering and exploratory data analysis workflow using synthetic IT employee data. It generates fictional employee records with Python and Faker, introduces realistic data-quality problems, cleans and transforms the data, normalizes it into a Third Normal Form (3NF) database design, stores the cleaned records in Neon PostgreSQL, and produces salary visualizations with Pandas, Matplotlib, and Seaborn.

The records are entirely synthetic and do not represent real employees or real salary patterns.

## Objectives

The project demonstrates how to:

- Generate at least 50 synthetic employee records with Python and Faker.
- Identify missing, duplicated, inconsistent, and invalid values.
- Clean and validate employee and workplace information.
- Transform dates and engineer useful analytical features.
- Apply min-max scaling to salary.
- Normalize repeated workplace information into 3NF tables.
- Connect Python to a Neon PostgreSQL database using `psycopg2`.
- Insert and retrieve employee records from the cloud database.
- Create standard and advanced salary visualizations.
- Explain the workflow and findings using notebook Markdown cells.

## Project Structure

```text
data-collection/
├── data/
│   ├── faulty_employees.csv
│   ├── clean_employees.csv
│   └── departments.csv
├── notebook/
│   └── Employee_Data_Engineering_EDA_Complete.ipynb
├── README.md
└── requirements.txt
```

The exact folder names are organizational conventions. If the notebook is stored somewhere else, update the relative paths used in the notebook.

## Dataset Columns

The initial flat dataset contains the following columns:

| Column            | Description                                    |
| ----------------- | ---------------------------------------------- |
| `employee_id`     | Unique employee identifier                     |
| `name`            | Synthetic employee name                        |
| `position`        | IT-related job title                           |
| `start_date`      | Employment start date between 2015 and 2024    |
| `salary`          | Annual salary between $60,000 and $200,000     |
| `department_id`   | Identifier linking an employee to a department |
| `department_name` | Name of the department                         |
| `work_location`   | City where the department operates             |
| `annual_budget`   | Department's annual budget                     |

## Intentional Data-Quality Problems

The notebook deliberately introduces errors so that the cleaning process can be demonstrated. These include:

- A duplicated employee ID
- Missing employee name, salary, and work location
- Extra spaces in text values
- Inconsistent capitalization and punctuation in job titles
- Inconsistent department and location names
- An invalid date value
- A start date outside the required period
- Salaries below and above the permitted range
- A negative department budget
- A budget stored as formatted text

## Data Cleaning Workflow

The notebook performs the following cleaning operations:

1. Loads the faulty CSV into a Pandas DataFrame.
2. Inspects data types, missing values, duplicates, and descriptive statistics.
3. Converts identifiers and numeric columns using `errors="coerce"` where appropriate.
4. Removes the duplicated primary-key record.
5. Trims unnecessary spaces from text.
6. Standardizes inconsistent job titles.
7. Fills the missing synthetic employee name.
8. Fills the missing salary with the median valid salary.
9. Caps salaries to the required $60,000–$200,000 range.
10. Converts dates and repairs invalid or out-of-range synthetic dates.
11. Rebuilds department name, location, and budget from the authoritative `department_id` mapping.
12. Uses assertions to verify the final cleaned dataset contains 50 valid employees.

## 3NF Database Design

The cleaned data is separated into two related tables.

### Employees

| Column          | Database role                         |
| --------------- | ------------------------------------- |
| `employee_id`   | Primary key                           |
| `name`          | Employee attribute                    |
| `position`      | Employee attribute                    |
| `start_date`    | Employee attribute                    |
| `salary`        | Employee attribute                    |
| `department_id` | Foreign key referencing `departments` |

### Departments

| Column            | Database role        |
| ----------------- | -------------------- |
| `department_id`   | Primary key          |
| `department_name` | Department attribute |
| `work_location`   | Department attribute |
| `annual_budget`   | Department attribute |

This structure is in 3NF because department name, location, and budget are stored once in the `departments` table instead of being repeated for every employee.

## Neon PostgreSQL Setup

Create a free PostgreSQL project in Neon. The notebook contains manual SQL instructions for creating or checking the required tables. It does not contain database deletion, truncation, or drop commands.

The database workflow is:

1. Create or confirm the `departments` and `employees` tables.
2. Insert departments before employees because employees reference department IDs.
3. Connect from Python using `psycopg2`.
4. Insert the cleaned DataFrames.
5. Query the joined tables and load the results into Pandas.

The notebook uses a direct placeholder:

```python
DATABASE_URL = "PASTE_YOUR_NEON_CONNECTION_STRING_HERE"
```

Replace the placeholder only while running the notebook. Never commit or upload the real connection string because it contains database credentials. Restore the placeholder before submitting or publishing the project.

## Installation

Create and activate a virtual environment if desired, then install the dependencies:

```bash
pip install -r requirements.txt
```

The main packages are:

- Pandas and NumPy for data processing
- Faker for synthetic record generation
- psycopg2-binary for PostgreSQL connectivity
- scikit-learn for min-max scaling
- Matplotlib and Seaborn for visualization
- Jupyter and ipykernel for running the notebook

## Running the Project

1. Open the project folder in Visual Studio Code.
2. Select the correct Python/Jupyter kernel.
3. Open `notebook/Employee_Data_Engineering_EDA_Complete.ipynb`.
4. Run the notebook cells from top to bottom.
5. Follow the manual Neon instructions when the database section is reached.
6. Confirm the query returns the expected employee records.
7. Save the notebook with its outputs and charts visible.
8. Remove the database URL before committing or submitting the project.

## Exploratory Data Analysis

The notebook reports:

- Data types and non-null counts with `.info()`
- Descriptive statistics with `.describe()`
- Missing values with `.isnull().sum()`
- Employee counts by job title
- Salary range and distribution
- Department and position combinations

## Transformation and Feature Engineering

The notebook creates:

- `start_year`, extracted from `start_date`
- `years_of_service`, calculated using a fixed analysis date
- `salary_scaled`, created through min-max scaling

The original salary remains available so that charts and conclusions can be expressed in dollars.

## Visualizations

### Average Salary by Position and Start Year

A grouped bar chart compares the average salary across IT positions and employee start years.

### Average Salary by Department and Position

An advanced heatmap uses the joined employee and department tables to compare average salary across two categorical dimensions.

Because the dataset is synthetic and some groups contain few employees, the visual results demonstrate the analytical process rather than real employment trends.

## Expected Outputs

After the notebook is completed successfully, it should show:

- 50 cleaned employee records
- 5 normalized department records
- Successful Neon connection and database query
- No missing required values
- Valid salary and start-date ranges
- Scaled salary values between 0 and 1
- A grouped bar chart
- A department-position salary heatmap
- Written insights and conclusions

## Security Note

Do not upload database passwords, active connection strings, or other credentials to GitHub or the learning management system. Before submission, confirm that the notebook contains only the connection-string placeholder.

## Conclusion

This project demonstrates how raw synthetic data can move through generation, ingestion, inspection, cleaning, transformation, normalization, cloud-database storage, retrieval, analysis, and visualization. It also shows why primary keys, foreign keys, validation rules, and consistent data types are important in a data engineering workflow.
