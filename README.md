# looker_dashboard_ecommerce
End-to-end E-commerce BI solution featuring a Looker Studio dashboard and a realistic 120k+ row synthetic dataset (2023-2025). Analyzes full order lifecycle—from marketing attribution and sales performance to complex logistics, shipping costs, and returns.

📌 Project Overview
This dataset represents the end-to-end operations of a fictional Indian e-commerce brand, "ShopEasy," covering a 3-year period (Jan 1, 2023 – Dec 31, 2025). It includes 120,000+ orders and granular details on sales, customers, inventory, and logistics.

This data is designed to power the Marketing & Sales Dashboard in Looker Studio, enabling analysis of:

Sales Performance: Revenue, order volume, and trends over time.

Customer Loyalty: Repeat purchase behavior and membership impact.

Logistics Efficiency: Delivery success rates, shipping costs, and partner performance (Delhivery, DTDC, etc.).

Product Metrics: Margins, stock levels, and price elasticity.

🔗 Dashboard Link: View Looker Studio Report

📂 File Descriptions & Schema
1. orders.csv (Fact Table)
Description: The central transaction table containing individual line items for every order.

order ID: Unique identifier.

Single Item Order: OD202301010001

Multi-Item Order: OD202301010001/1, OD202301010001/2 (Allows Market Basket Analysis).

order date: Date order was placed (YYYY-MM-DD).

dispatch date: Date handed to courier (or NA if rejected/cancelled).

customer name: Links to customers.csv.

Product SKU: Links to products.csv.

marketing channel: Source of the sale (e.g., Google Ads, Organic, Email).

qty ordered vs qty shipped: Differences indicate partial fulfillment issues.

order amount: Final transaction value after discounts.

return reason: Reason for return (e.g., "Defective", "Size Issue"); NA for successful orders.

2. logistics.csv (Fact Table)
Description: Tracks the shipping journey for every parent order. Link to orders.csv using the base order id (before the /).

order id: Parent ID (e.g., OD202301010001). Matches the base ID in the orders table.

logistic name: Courier partner (Delhivery, DTDC, Xpressbees, Nimbus, SelfShip).

status: Current status (delivered, rejected, return to origin, partially delivered).

total packages: Number of boxes in the shipment.

weight: Total shipment weight in kg (calculated as ~0.5kg - 5kg per package).

shipping charges: Cost incurred for shipping. Formula: Base Fee + (Weight * Provider Rate).

Note: Charges are 0 if status is rejected.

3. products.csv (Dimension Table)
Description: Master catalog of all items sold.

Product SKU: Primary Key (e.g., SKU-10001).

margin: Profit margin percentage (whole number, e.g., 15, 20).

unit price: Standard selling price.

price update: The previous price before the latest update (allows price change analysis).

packages count: How many physical boxes this item requires (affects logistics).

4. customers.csv (Dimension Table)
Description: Database of registered users.

customer name: Primary Key.

membership %: Discount tier (5%, 10%, 15%, or NA).

membership date: When they joined the loyalty program.

pincode: 6-digit Indian pincode (100000-999999).

5. product_history.csv (Dimension Table)
Description: Historical log of price and stock changes for trending analysis.

price update timestamp: When the price changed (contains NA for unchanged items).

stock update timestamp: When inventory was last audited.

📊 How to Join These Tables (Data Model)
For your Looker Studio dashboard to work correctly, create the following relationships:

Sales Analysis:

Join orders.Product SKU ↔ products.Product SKU (Many-to-One).

Join orders.customer name ↔ customers.customer name (Many-to-One).

Logistics Analysis:

Create a calculated field in orders called Parent Order ID by removing the /1, /2 suffix.

Join orders.Parent Order ID ↔ logistics.order id (Many-to-One).

💡 Key Metrics You Can Calculate
Average Order Value (AOV): Sum(order amount) / Count Distinct(order id)

Return Rate: Count(order id where return reason is not NA) / Total Orders

Shipping Cost %: Sum(shipping charges) / Sum(order amount)

On-Time Delivery Rate: Count(orders where status = 'delivered') / Total Orders

Customer Lifetime Value (CLV): Sum(order amount) grouped by customer name.

⚠️ Notes for Analysis
"NA" Values: Standard placeholder for nulls in this dataset (e.g., non-members have NA in membership fields). Ensure your dashboard treats these as "No Value" or filters them out.

Partial Deliveries: If qty shipped < qty ordered, it indicates a stockout or logistics issue.

Rejection Costs: Rejected orders have shipping charges = 0 in this dataset, simulating a model where the vendor absorbs the cost elsewhere or isn't charged for failed pickups.

📋 Dataset Summary Table
Metric	Details
Date Range	Jan 1, 2023 – Dec 31, 2025 (3 Years)
Total Order Lines	~120,560 (Projected)
Total Customers	8,000 Unique Customers
Product Catalog	1,200 Unique SKUs
Avg. Daily Orders	~110 Orders/Day (Min 100 guaranteed)
Logistics Providers	5 (Delhivery, DTDC, Xpressbees, Nimbus, SelfShip)
Order Types	50% Single Item / 50% Multi-Item Baskets
Key Features	Market Basket Analysis (/1 suffixes), Partial Deliveries, Returns Logic, Membership Tiers
Total Revenue	~ ₹ 250 Cr+ (Estimated based on Unit Price & Volume)
📂 File-by-File Statistics
File Name	Rows (Approx)	Columns	Key Usage
orders.csv	~180k - 200k	14	Sales KPIs: Revenue, AOV, Product Performance, Customer Trends.
logistics.csv	~120k	9	Ops KPIs: Shipping Costs, Courier Performance, Delivery Status.
products.csv	1,200	8	Inventory: Margins, Stock Levels, Category Analysis.
customers.csv	8,000	7	CRM: Loyalty Tiers, Regional Distribution, Wallet Balance.
product_history.csv	~500	7	Trends: Price Fluctuation & Stock Movement History.

