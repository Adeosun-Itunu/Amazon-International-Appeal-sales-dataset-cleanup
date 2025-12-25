# Amazon-International-Apparel-sales-dataset-cleanup

## Project Overview 

This project focused on cleaning a messy international sales dataset of 37,000+ rows using Excel Power Query. The raw data contained repeated headers, subtotal rows, blank and hidden-value cells, mixed data types, and inconsistencies in key columns like SKU, Size, and PCS. The SKU column was particularly problematic, with hidden spaces, non-breaking characters, and blanks that initially prevented accurate filtering, merging, and analysis. Through systematic cleaning, removing non-data rows, standardizing text columns, normalizing SKUs, converting data types, and revealing true nulls, the dataset was reduced to 18,169 clean transactional records, fully structured and analysis-ready, supporting reliable SKU-level reporting and business insights.

## Tool
Excel (Power Query)

## After loading the raw CSV into Power Query:
* Dataset contained:
1. Header rows mixed with data
2. Totals and subtotal rows
3. Repeated headers inside the table
   
* Misaligned values across columns

1. Multiple columns had wrong data types:
2. Dates inside text columns
3. Text values inside numeric columns
   
* Key identifier columns:
1. Index
2. Date
3. SKU
4. Style
5. Size
6. PCS
7. Customer

## Immediate conclusion

The dataset was not analysis-ready.
Row-level integrity was broken, so row cleanup had to happen before column cleanup.

## Row Structure Cleanup (Reducing 37,000+ → ~18,000 rows)

Step 1: Remove non-data rows
Using filters and row inspection:
	Removed:
	- Blank rows
	- Repeated header rows
	- Totals / summary rows
	- Rows where Row Type ≠ actual sales record

This step alone cut the dataset by more than half.
Result: 18,169 clean transactional rows

## Column Standardization (Structural Cleanup)

* Columns addressed early
- SKU
- Customer
- Style 
- Size
- PCS

* Actions applied
- Trim
- Clean
- Uppercase where appropriate
- Removed leading/trailing spaces
- Removed non-breaking spaces

## Index Column Issues

* Finding
- Index started at 0
- Index order was inconsistent after row deletions
*Resolution
 - Dataset was first sorted properly (by Date, then original order)
 - Index column was removed
 - A new Index column was added:
 - Starting from 1
 - Reflecting clean row order

* This fixed ordering issues permanently.
  
## Date & Month Issues

* Findings
- Dates appeared in:
- Date column
- Style column
- Other text columns
- Month column did not exist initially

* Resolution
- Dates removed from non-date columns
- Proper Date column enforced
- Month column created after cleaning, not before

  Size Column Problems

* Findings
- Size column contained:
- Actual sizes (S, M, L, XL)
- Text like PCS, FREE
- Style codes
- Junk values

*  Resolution
- Size standardized as Text
- Non-size values filtered and corrected
- Final Size column kept only valid size logic

Old Size columns were removed once the cleaned version was confirmed.

## PCS Column Errors

* Findings
- Conversion to number failed
- Error:
[DataFormat.Error] We couldn’t convert to Number

  * Cause
	- PCS column contained text values
* Resolution
- Non-numeric rows identified
- Invalid rows removed or corrected
- PCS converted to Whole Number only after cleanup

## SKU Cleanup (Main Pain Point)

* Initial Findings
- SKU column contained:
- True nulls
- Empty strings
- Spaces
- Non-breaking spaces
- Many rows looked blank but were not technically null

the below formula generated an accurate output 

let
    t0 = try Text.From([SKU]) otherwise "",
    t1 = Text.Replace(t0, Character.FromNumber(160), " "),
    t2 = Text.Trim(Text.Clean(t1))
in
    if t2 = "" then null else t2
    
  * Result
- Fake blanks → real NULLs
- Missing SKUs finally visible
- ~427 missing SKU rows identified

This was the turning point of the cleanup.

## Business Decision on Missing SKUs

Because:
- Analysis revolved around SKU
- No SKU master file was available
- Guessing SKUs would corrupt results

Final decision
✅ missing SKU were replace by Unknown

## Final Dataset Status

SKU
- No blank
- No nulls
- No hidden spaces
- Reliable identifier

* Structure
- Clean rows
- Correct data types
- Proper indexing
- Analysis-ready

## Final Analyst Conclusion

The dataset started as a highly corrupted 37,000+ row file with broken row structure and inconsistent identifiers.
Through systematic row filtering, column standardization, and identifier validation, it was reduced to 18169 clean transactional records suitable for SKU-based analysis.
All remaining records now meet the business requirement of having a valid SKU.




 

