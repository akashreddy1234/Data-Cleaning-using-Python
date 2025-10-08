````markdown
# Customer Call List Data Cleaning

## Project Overview
This project demonstrates **data cleaning using Python and Pandas**. The dataset contains customer call details with columns like customer information, phone numbers, addresses, and contact preferences. The goal is to clean and preprocess the dataset so that it is ready for analysis or further processing.

## Dataset
- Original dataset: `Customer Call List.xlsx`
- Cleaned dataset: `Customer_Call_List_Cleaned.xlsx`

**Columns in original dataset:**
- `CustomerID`
- `First_Name`
- `Last_Name`
- `Phone_Number`
- `Address`
- `Paying Customer`
- `Do_Not_Contact`
- `Not_Useful_Column` (removed during cleaning)

## Cleaning Steps
1. **Remove duplicates**  
   Ensures that each customer record is unique.

2. **Remove unnecessary columns**  
   Dropped columns that are not useful for analysis (e.g., `Not_Useful_Column`).

3. **Clean last names**  
   Removed unwanted characters from `Last_Name` using `str.strip()`.

4. **Standardize phone numbers**  
   - Removed any non-alphanumeric characters.
   - Reformatted numbers as `xxx-xxx-xxxx`.
   - Removed invalid or missing phone numbers.

5. **Split address into separate columns**  
   - `Street_Address`, `State`, `Zip_code` (split by commas).
   - Dropped `Zip_code` if empty after cleaning.

6. **Standardize categorical values**  
   - `Paying Customer`: converted `Y/N` to `Yes/No`.
   - `Do_Not_Contact`: converted `Y/N` to `Yes/No`.

7. **Remove unwanted rows**  
   - Deleted customers who opted for `Do_Not_Contact`.
   - Deleted customers without a valid phone number.

8. **Handle missing values**  
   Replaced `NaN` or `N/a` with empty strings.

9. **Reset index**  
   After dropping rows, reset index to maintain proper sequence.

## Python Code

```python
import pandas as pd

# Load dataset
df = pd.read_excel("Customer Call List.xlsx")

# Remove duplicates
df = df.drop_duplicates()

# Drop unuseful columns
df = df.drop(columns="Not_Useful_Column")

# Clean last names
df["Last_Name"] = df["Last_Name"].str.strip("123./_")

# Clean and format phone numbers
df['Phone_Number'] = df['Phone_Number'].str.replace('[^a-zA-Z0-9]', '', regex=True)
df["Phone_Number"] = df["Phone_Number"].apply(lambda x: str(x))
df["Phone_Number"] = df["Phone_Number"].apply(lambda x: x[0:3]+'-'+x[3:6]+'-'+x[6:10])
df["Phone_Number"] = df["Phone_Number"].str.replace('nan--','')
df["Phone_Number"] = df["Phone_Number"].str.replace('Na--','')

# Split address into columns
df[["Street_Address","State","Zip_code"]] = df["Address"].str.split(',', n=2, expand=True)

# Standardize categorical columns
df['Paying Customer'] = df['Paying Customer'].replace({'Y':'Yes','N':'No'})
df['Do_Not_Contact'] = df['Do_Not_Contact'].replace({'Y':'Yes','N':'No'})

# Handle missing values
df = df.replace('N/a', '').fillna('')

# Remove unwanted records
df = df[df['Do_Not_Contact'] != 'Yes']
df = df[df['Phone_Number'] != '']

# Drop empty Zip_code column
df = df.drop(columns='Zip_code')

# Reset index
df = df.reset_index(drop=True)

# Save cleaned dataset
df.to_excel("Customer_Call_List_Cleaned.xlsx", index=False)

print(df)
````

## Outcome

* The cleaned dataset is saved as `Customer_Call_List_Cleaned.xlsx`.
* It is ready for further analysis or visualization.
* Missing, incorrect, or unformatted values have been standardized.

---

**Skills Demonstrated:**

* Python & Pandas
* Data cleaning and preprocessing
* Handling missing values and duplicates
* String manipulation
* Exporting cleaned data to Excel

```
