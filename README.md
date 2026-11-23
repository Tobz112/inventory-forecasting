# Retail inventory project: what I found

**The data:** 109,500 rows. 5 stores x 30 products x 730 days (2022-2023). No missing values, no duplicates. One row = one product in one store on one day.

**The question:** are reorders keeping up with sales, and where is stock most at risk of running short?

## What I found

**1. Reorders roughly keep up with sales.**
Orders were placed on every single row, so how *often* things get reordered doesn't tell me anything. What matters is how *much*. Units ordered came to about 99.4-99.7% of units sold in every category. So orders are just slightly under sales, but very close.

**2. Stock is thin everywhere.**
Stock covers about 1.6 days of sales on average. Winter is the thinnest (about 1.58 days vs about 1.66 in autumn). Winter sales go up about 15% but stock only goes up about 10%, so stock doesn't quite keep up. The gaps between categories and between stores are tiny (under 0.1 days), so I wouldn't read much into them.

**3. Stockout risk (the heatmap).**
I flagged a day as "at risk" when stock was lower than the average sales of the previous 7 days for that store and product. About 15.7% of days get flagged. I guessed it would be under 10%, so I was wrong there, and I haven't checked why.
- Most category/season boxes are between 14.5% and 16.5%, so they're pretty similar.
- Highest: Clothing in Winter (19.6%) and Groceries in Summer (19.4%).
- Lowest: Groceries in Spring (12.5%).
- Groceries only has 7,300 rows, so its swings could just be chance.

## What I did
- **pandas:** cleaned the data, checked it, and did the basic exploring.
- **DuckDB (SQL):** LAG, a rolling 7-day average, and a CTE to flag the risk days.
- **Seaborn:** the heatmap of risk by category and season.

## Things to keep in mind
- The "at risk" rule is my own guess. There's no delivery/lead-time data, so I can't say what a real safe level is.
- I left region off the heatmap. Every store and product shows up in all 4 regions, so region doesn't seem to be a real place.
- Days with zero sales (only 88) give an average of 0, so they never get flagged.
- I didn't work out how much money is tied up in slow-moving stock. Average stock on hand is roughly 1.2M (in whatever the price column is in), but I didn't split out the slow sellers.
- The data looks made up (generated), so the patterns might just come from how it was built.
