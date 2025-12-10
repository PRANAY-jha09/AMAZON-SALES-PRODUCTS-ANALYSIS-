he Amazon Canada Products 2023 dataset contains product-level information like price, rating, and category, but lacks transactional sales data (order dates, quantities sold, customer IDs, and regions/provinces).

To address the lack of sales/customer data and still create a compelling dashboard, the project needs to shift from a Sales Performance Dashboard to a Product and Pricing Analytics Dashboard.

Here is a detailed, step-by-step guide with specific questions tailored to the available columns:

The dataset columns are:

asin [(Product ID)]

title (Product Name)

imgUrl (Image URL)

productURL (Product URL)

stars (Product Rating)

reviews (Number of Reviews)

price (Current Price in CAD)

listPrice (Original/List Price in CAD)

categoryName (Product Category)

isBestSeller (Boolean)

Part 1: Business Questions & KPI Definition 🎯
The core business goal is to understand product assortment performance and pricing strategy on Amazon.ca.

KPI Category	Key Performance Indicators (KPIs)	Business Questions to Answer
Product Performance	Top Categories, Bestseller Ratio, Average Rating/Review Count	1. Which product categories have the highest number of listings?
Discount Rate ( 
listPrice
listPrice−price
​
 )	2. Which categories show the highest and lowest average customer ratings (stars)?
Pricing & Margin	Average Price, Average Discount Percentage	3. How does the average price vary across the top 10 categories?
Bestseller Price/Discount Profile	4. What is the average discount percentage offered across all products and by category?
Customer Insight	Rating/Review Correlation	5. Is there a correlation between the number of reviews and the average rating (stars)?

Export to Sheets
Part 2: Data Preparation & Analysis Steps
Step 1: Data Cleaning and Preparation (Python/Excel)
Use Python (Pandas) for efficiency due to the dataset's size (2.1M rows).

Action	Tool	Purpose & Steps
A1: Load & Inspect	Python (Pandas)	Load the amz_ca_total_products_data_processed.csv file. Print .info() and .head().
A2: Handle Nulls/Zeros	Python (Pandas)	Price/ListPrice: The search results indicate 0 is used for unavailable prices. Filter out or replace rows where price is 0 or less, as these can skew financial analysis. Reviews/Stars: Keep them as 0, but be mindful when calculating averages (e.g., filter out products with 0 reviews when calculating average rated product price).
A3: Calculate Key Metrics	Python (Pandas)	1. Create Discount Metrics: Calculate Discount_Amount (listPrice - price) and Discount_Percent ( 
listPrice
listPrice−price
​
 ). 2. Clean Category Name: Handle any inconsistencies or typos in the categoryName column.
A4: Outlier Check	Python (Pandas)	Check for extreme outliers in price (e.g., prices over $10,000) and decide whether to cap/remove them for cleaner visualizations.
A5: Export for SQL/BI	Python (Pandas)	Save the cleaned and enriched data (including the new discount columns) as a new CSV file (e.g., amazon_ca_cleaned.csv).

Export to Sheets
Step 2: Querying and Pre-Analysis (SQL/Python)
For a portfolio project, showing advanced SQL is valuable. Use a local database (SQLite, PostgreSQL, etc.) or a Python environment like Kaggle/Jupyter Notebook to write your "queries" with Pandas/Python.

Query/Analysis Goal	Tool/Method	Specific Question & Code Concept
Q1: Category Dominance	SQL: COUNT(), GROUP BY, ORDER BY	What are the Top 10 product categories by number of listings? SELECT categoryName, COUNT(asin) AS ProductCount FROM Table GROUP BY 1 ORDER BY 2 DESC LIMIT 10;
Q2: Average Pricing	SQL: AVG(), GROUP BY	What is the average price for the Top 10 categories? SELECT categoryName, AVG(price) AS AvgPrice_CAD FROM Table WHERE price > 0 GROUP BY 1 ORDER BY 2 DESC;
Q3: Bestseller Profile	SQL: AVG(), WHERE, GROUP BY	Do Bestsellers have a higher average price or discount rate? SELECT isBestSeller, AVG(price), AVG(Discount_Percent) FROM Table WHERE price > 0 GROUP BY 1;
Q4: Rating Analysis	SQL: AVG(), WHERE	What is the average star rating for products with at least 50 reviews? (To filter out unreliably rated products) SELECT categoryName, AVG(stars) AS AvgStarRating FROM Table WHERE reviews >= 50 GROUP BY 1 ORDER BY 2 DESC;
Q5: Discount Analysis	SQL: AVG(), WHERE	Which category has the highest average Discount Amount? SELECT categoryName, AVG(Discount_Amount) AS AvgDiscount_CAD FROM Table WHERE Discount_Amount > 0 GROUP BY 1 ORDER BY 2 DESC LIMIT 5;

Export to Sheets
Step 3: Dashboard Visualization (Power BI/Tableau)
Connect your cleaned CSV file to your BI tool to build the visualizations that answer the business questions.

Visualization	KPI/Question Answered	Insights/Business Recommendation Example
Key Performance Indicators (KPI) Cards	Total Products, Avg. Price, Avg. Discount %, Avg. Rating	Showcase overall health and pricing strategy.
Bar Chart (Top N)	Q1: Top 10 Categories by Product Count	Which categories are currently dominating the Amazon.ca listings?
Bar Chart (Ratings/Pricing)	Q2/Q4: Avg. Price & Avg. Rating by Category	Plot Avg. Price vs. Avg. Rating. Insight: The most expensive categories often have lower ratings.
Scatter Plot	Q5: Rating/Review Correlation	Plot reviews (X-axis) vs. stars (Y-axis). Insight: For products with over 1000 reviews, the rating variance is very small, suggesting rating reliability increases with volume.
Histogram/Bar Chart	A3/Q3: Distribution of Discount % vs. Bestseller Status	Plot the average discount percentage, segmented by isBestSeller. Insight: Non-Bestsellers require an average of 15% deeper discount to compete.
Treemap/Donut Chart	Q1: Product Count by Category	Visual representation of product market share by category.

Export to Sheets
Part 4: Storytelling & Business Recommendation 💡
The final step for a successful project is translating your data insights into an actionable recommendation.

Sample Insight Generation and Recommendation
Insight from Q4: "Category 'Electronics' has a high product count but the lowest average star rating (3.5/5.0) among the top 10 categories (with over 50 reviews)."

Business Recommendation (Product Quality Angle):

"Electronics" products show concerningly low average ratings. Recommendation: Perform a deep-dive analysis on the top 5 lowest-rated electronics sub-categories (e.g., specific brands or product types) to identify quality control issues or misleading product descriptions. Focus on improving listing details to manage customer expectations and boost ratings.







