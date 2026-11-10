Step 1: File Description & Initial Diagnosis
File Overview:

Rows: 97 raw observations

Columns (6): date (object), item (object), city (object), price (object), unit (object), data_label (object)

Metadata columns: unit ("PKR per kg (illustrative)") and data_label ("ILLUSTRATIVE TEACHING DATA - NOT ACTUAL PBS OBSERVATIONS") are 100% uniform across all 97 rows.

Diagnostic Findings (Revealing Python Code & Findings):
Exact Duplicate Row (Row 96):

Code: df.duplicated()

Finding: Row 96 is an exact duplicate of Row 8 (2026-01-08, Staple A, City 1, 99.5).

Mislabeled Item / Typo (Row 40):

Code: df['item'].value_counts()

Finding: Row 40 is listed as oil b (lowercase) for City 1 on 2026-02-05 with a price of 108.5. Cross-referencing reveals Oil B for City 1 on 2026-02-05 already exists at Row 42 (318.0). Meanwhile, Staple A for City 1 on 2026-02-05 is completely missing, and its expected price (matching City 2 price of 114.5 minus the standard 6.0 PKR city spread) is exactly 108.5.

Missing Value / Non-Reporting (Row 5):

Code: df.isnull().sum()

Finding: Pulse C for City 2 on 2026-01-01 has a NaN price.

Formatting Issue (Comma Thousands Separator - Row 17):

Code: df['price'].apply(lambda x: is instance(x, str) and ',' in str(x))

Finding: Row 17 (Staple A, City 2, 2026-01-15) has price stored as "1,250", forcing the price column to load as text (object type).

Extreme Data Outliers / Data Entry Errors:

Code: pd.to_numeric(df['price'].str.replace(',',''), errors='coerce') > 1000

Row 17: 1,250 for Staple A (normal baseline ~103–107 PKR). Likely an extra zero typo for 125.0 or a misplaced comma/decimal.

Row 30: 9999 for Sweetener D on 2026-01-22 (normal range ~150–180 PKR). Classic placeholder/sentinel for missing data.


Step 2: Planned Decision Log 
Row 96 (Duplicate): Exclude. Dropping this exact duplicate restores the correct panel structure of eight observations per date.

Row 40 (Mislabeled Item): Correct. Relabel oil b to Staple A to fix the missing record without fabricating numbers.

Row 5 (Missing Price): Retain and Flag. Keep the row to preserve date coverage, but flag it as missing to prevent skewing calculations.

Row 17 (String Format and Outlier): Investigate and Flag. Strip the comma to convert the data type to numeric float, and flag the value 1250.0 as an unverified extreme outlier.

Row 30 (Sentinel Value 9999): Flag and Convert. Convert 9999 to NaN and flag it to protect downstream price indices from corruption.

Step 3. Trial of the AI's Proposed Cleaning Code
Line One (drop_duplicates()): Keep. Successfully removes duplicate row ninety-six without losing any valid unique observations.

Line Two (to_numeric() with comma replacement): Modify. Essential for converting object types to floats, but needs careful tracking alongside missing value flags.

Line Three (dropna(subset=['price'])): Reject. Silently deletes row five (Pulse C), breaking the panel balance for January first. Missing values should be retained as NaN and explicitly flagged instead of being silently dropped.

Line Four (df[df['price'] < 1000]): Reject. Silently deletes row seventeen (1250.0) and row thirty (9999). Silently deleting rows destroys audit transparency; outliers should be flagged or converted to NaN rather than hidden.

Line Five (.str.title()): Reject as standalone. Running this converted oil b to Oil B, creating duplicate entries for Oil B on February fifth while leaving Staple A missing. Contextual price auditing must precede string formatting.

Step 4. Summary of Results and Item Frequencies
Following our audited cleaning process rather than the naive AI approach, the final dataset contains exactly ninety-six rows (twelve weekly dates multiplied by eight city-item observations per date). Every item now appears the expected number of times:
Staple A: Twenty-four observations.

Oil B: Twenty-four observations.

Pulse C: Twenty-four observations.

Sweetener D: Twenty-four observations.

The final clean dataset retains two missing price entries (NaN) corresponding to the original missing value in row five and the converted sentinel value in row thirty, ensuring complete honesty in the ledger without hidden deletions.

