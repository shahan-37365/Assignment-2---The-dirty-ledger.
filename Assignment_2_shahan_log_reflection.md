Final Evaluation AI vs. My Decisions
Handling of Row 40 (oil b):

AI Decision: Converted string to title case (Oil B), resulting in duplicate Oil B entries for 2026-02-05 and a missing Staple A entry.

Student Decision: Identified that 108.5 matched the Staple A price trend for City 1, correctly relabeling it to Staple A.

Better Supported: Student Decision. It preserves panel completeness and reflects the true underlying price structure.

Handling of Outliers & Missing Values (Rows 5, 17, 30):

AI Decision: Silently deleted all 4 problematic rows (dropna + < 1000 filter).

Student Decision: Retained the row structure, flagged outliers (1250 and 9999), converted 9999 to NaN, and documented all changes in a dedicated cleaning log.

Better Supported: Student Decision. As Ms. Noor noted, a "clean-looking" table that silently deletes observations is worse than an honest messy table. Transparent flagging protects data integrity for downstream analysis.

Our  decisions proved significantly better supported than the AI's automated code. The AI code relied on blind filtering (dropna and price thresholds), which silently destroyed four valid date slots and created duplicate categories through unverified string manipulation.

By contrast, our audit preserved panel completeness, corrected the mislabeled Staple A record using logical price alignment, and maintained an honest audit trail via explicit flagging. As Ms. Noor noted, a clean-looking table with undocumented changes is worse than a messy honest one.
