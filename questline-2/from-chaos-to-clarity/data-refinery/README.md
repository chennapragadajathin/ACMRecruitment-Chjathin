# Data Refinery

This project cleans a student performance dataset and saves the cleaned output as `cleaned_students_performance.csv`.

## Cleaning steps performed
1. Loaded the raw CSV into a Pandas DataFrame.
2. Checked the dataset shape and counted duplicate rows.
3. Removed duplicate records using `drop_duplicates()`.
4. Filled missing numeric values with the median of each column.
5. Filled missing text values such as `Name`, `Gender`, and `Pass_Status` using sensible defaults (`Unknown` and mode values).
6. Verified there were zero duplicate records and zero missing values remaining.
7. Saved the cleaned dataset to `cleaned_students_performance.csv`.

## Outcome
The cleaned dataset contains no duplicate rows and no unintended missing values, making it ready for further analysis.
