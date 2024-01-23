# SQL.Data_Cleaning_Queries
This repository contains a set of SQL queries designed for cleaning and transforming data within a Microsoft SQL Server database. 
The queries address various data cleaning tasks such as standardizing date formats, handling missing values, breaking down address fields, and more.

Queries Overview:
Standardize Date Format:

# Converts the 'SaleDate' column to a standardized date format.
-- Standardize Date Format
Update dbo.housingData
SET SaleDate = CONVERT(Date, SaleDate)
# Handle Missing Property Addresses:

Fills missing 'PropertyAddress' values by joining with another table based on 'ParcelID' and 'UniqueID'.
-- Property Address Data
Update a
SET PropertyAddress = ISNULL(a.PropertyAddress, b.PropertyAddress)
From dbo.housingData a
JOIN dbo.housingData b ON a.ParcelID = b.ParcelID AND a.[UniqueID] <> b.[UniqueID]
Where a.PropertyAddress is null

# Break Out Address Into Individual Columns:

Splits 'PropertyAddress' into separate columns for address, city, and state.
-- Breaking Out Address into Individual Columns(Address, City, State)
ALTER TABLE dbo.housingData
Add PropertySplitAddress Nvarchar(255);

Update dbo.housingData
SET PropertySplitAddress = SUBSTRING(propertyAddress, 1, CHARINDEX(',', propertyAddress) - 1)

# Handle Missing Owner Addresses:

Parses 'OwnerAddress' and splits it into separate columns for address, city, and state.

-- Handle Missing Owner Addresses
ALTER TABLE dbo.housingData
Add OwnerSplitAddress Nvarchar(255);

Update dbo.housingData
SET OwnerSplitAddress = PARSENAME(replace(OwnerAddress, ',', '.'), 3)

# Change Y and N to YES and NO:

Updates values in the 'SoldAsVacant' column to 'YES' or 'NO'.

# Remove Duplicate Values:

Identifies and displays duplicate values across all columns in the database.

# Identify and Display Duplicate Rows:

Uses a CTE to identify and display rows with duplicate values based on certain columns.

# Delete Unused Date Column:

Deletes the 'SaleDate' column to remove unnecessary data.


# Important Notes:
Caution:

Exercise caution when using these queries, especially if applying them to a production database. It's advisable to test in a safe environment first.
Customization:

Review and customize the queries based on your specific database schema and data requirements.
Feedback and Contributions:

Feel free to provide feedback or contribute improvements to these queries.

