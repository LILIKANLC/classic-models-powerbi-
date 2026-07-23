# Classic Models - Sales & Customer Analysis Dashboard

**Tools:** Power BI (Power Query, DAX, Data Modeling)

**Dataset:** Classic Models - sample database of a scale model cars retailer

**Period:** 2003-2005

## Project Overview

The analysis is built around three core business questions:

1. **Which products should be ordered more or less?**
Inventory analysis combining stock risk and product performance to identify restocking priorities.

2. **How can marketing strategies be adapted to customer behavior?**
Customer segmentation into VIP and low-engagement groups to guide communication strategy.

3. **How much can we spend on acquiring new customers?**
Customer Lifetime Value (LTV) calculation to define a data-driven acquisition budget.

## Data Model

The report uses a Star Schema with 8 tables and a custom Date Table.

| Table | Records | Description |
|-------|--------:|-------------|
| **orders** | 326 | Customer orders with order dates and statuses |
| **orderdetails** | 2,996 | Order line items with products, quantities, and prices |
| **customers** | 122 | Customer profiles from 28 countries |
| **products** | 110 | Products with prices and inventory levels |
| **productlines** | 7 | Product categories |
| **payments** | 273 | Customer payment records |
| **employees** | 23 | Employees, sales representatives, and managers |
| **offices** | 7 | Company offices located in 7 cities |

Relationships: orders → customers, orderdetails → products → productlines, customers → employees → offices, orders → DateTable (active on orderDate)

## Data Preparation (Power Query)

- Removed `comments` column from orders (over 80% empty)

- Removed `addressLine2` from customers (sparse, no analytical value)

- Removed `textDescription` from productlines (free text, not used in analysis)

- Replaced NULL values in `state` with "N/A" for non-US customers

- Set correct data types: dates → Date, prices → Decimal Number

- Created calculated column `Revenue` in orderdetails: `quantityOrdered × priceEach`

- Created calculated column `Profit` in orderdetails: `quantityOrdered × (priceEach − buyPrice)`

- Created calculated column `ShippingDays` in orders: `shippedDate − orderDate`

## DAX Measures

| Measure | Formula / Purpose |

|---|---|

| Total Revenue | SUMX on orderdetails Revenue column |

| Total Profit | SUMX on orderdetails Profit column |

| Total Orders | DISTINCTCOUNT of orderNumber |

| Total Customers | DISTINCTCOUNT of customerNumber |

| Avg Order Value | Total Revenue / Total Orders |

| Profit Margin % | Total Profit / Total Revenue |

| YTD Revenue | TOTALYTD of Total Revenue |

| MTD Revenue | TOTALMTD of Total Revenue |

| QTD Revenue | TOTALQTD of Total Revenue |

| Revenue LY | CALCULATE with SAMEPERIODLASTYEAR |

| Revenue Growth % | (Total Revenue − Revenue LY) / Revenue LY |

| Customer LTV | Total Revenue / Total Customers |

| Avg Profit per Customer | Total Profit / Total Customers |

| Avg Shipping Days | AVERAGE of ShippingDays column |

| Low Stock Ratio | SUM(quantityOrdered) / MAX(quantityInStock) |

| Low Stock Items | Count of products where Low Stock Ratio > 1 |

| New Customers | Customers placing their first order in the period |

| Top Country | Country with highest Total Revenue |


## Dashboard Structure

The dashboard consists of 6 pages and 1 summary page.

**Page 1 - Sales Overview**

KPI cards: Total Revenue, Total Profit, Total Orders, Avg Order Value.

Monthly Revenue Trend (line chart by year), Revenue by Product Line (bar chart).

Slicers: Select Year, Select Product Line.

**Page 2 - Geography & Growth**

Revenue by Country (filled map), Year-over-Year Revenue Growth (column chart),

Revenue by Quarter (line chart).

Note: 2005 data covers January–October only - full-year comparison is not applicable.

**Page 3 - Product Performance**

Top 10 Products by Performance (bar chart),

Revenue Performance vs Stock Availability (scatter chart - products with high demand

and low stock are highlighted as critical restocking priorities).

**Page 4 - Inventory Analysis**

Profit Margin % by Product Line (bar chart),

Current Inventory by Product Line (bar chart - count of products),

Bottom 10 Products by Performance (candidates for inventory reduction).

**Page 5 - Customer Overview**

KPI cards: Total Customers, Customer LTV, Avg Profit per Customer, Top Country.

New Customers by Month (line chart - acquisition trend by year),

Top 10 VIP Customers by Profit, Bottom 5 Customers by Profit.

**Page 6 - Customer Segments**

Top 10 Countries by Revenue (bar chart),

Customer Value Analysis (scatter chart - LTV vs Total Profit per customer,

VIP segment clearly separated from regular customers).

**Page 7 - Key Insights & Recommendations**

Summary of key findings and actionable business recommendations.


## Key Findings

- **Classic Cars generate 40% of total revenue** - the most critical product line by far

- **11 products are at critical stock risk** - Vintage Cars and Motorcycles are top priority for restocking

- **Motorcycles have the highest profit margin (41.8%)** despite lower revenue than Classic Cars

- **November is the peak month** across all three years - critical period for inventory and staffing

- **Revenue grew 36% from 2003 to 2004** but declined 61% in 2005 (partial year, Jan–Oct only)

- **Euro+ Shopping Channel and Mini Gifts Distributors** are dominant VIP clients - classic Pareto effect where two customers generate a disproportionate share of profit

- **No new customers were acquired after September 2004** - urgent need for marketing investment

- **Customer LTV = $39K** - this defines the maximum reasonable budget per new customer acquisition

- **USA dominates revenue** ($3.3M vs $1.1M for Spain in 2nd place)

- **Average delivery time is 4 days** - consistent operational performance


## Business Recommendations

1. **Prioritize restocking Vintage Cars and Motorcycles** - high demand combined with low stock creates risk of lost sales

2. **Launch a VIP loyalty program** targeting Euro+ Shopping Channel and Mini Gifts Distributors to protect the most profitable relationships

3. **Run a re-engagement campaign** for bottom-tier customers - even modest improvement in low-engagement accounts would meaningfully impact total profit

4. **Invest in new customer acquisition** - LTV of $39K per customer justifies significant marketing spend; focus on Spain, France, and Australia as high-potential markets

5. **Plan inventory and staffing increases for October–November** to handle peak season demand without stockouts

6. **Review Trains product line** - lowest profit margin (34.7%) and smallest inventory (3 products); evaluate whether continued investment is justified
