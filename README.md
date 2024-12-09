# 📊 Electoral Bond Data Analysis Project

## 🚀 Project Overview
In this project, we analyze the Electoral Bond data following the Supreme Court's landmark judgment mandating transparency in political party funding. The Election Commission of India (ECI) released comprehensive data, which is supplemented by additional data from the Enforcement Directorate. This analysis focuses on understanding the flow of money from donors to recipients, identifying sources of monetary donations, and assessing the impact of political funding transparency.

## 🗂️ Data Tables
We have three main data tables:
1. **Donor Data**: Details about companies, organizations, and individuals who bought electoral bonds.
2. **Receiver Data**: Details about political parties and the bonds they received.
3. **Bank Data**: Details about bank branches where bonds can be issued or encashed.

## 📜 Data Dictionary
### Donor Data Table
| Column Name    | Description                                            |
|----------------|--------------------------------------------------------|
| SNo            | Serial number.                                         |
| Urn            | Unique reference number.                               |
| JournalDate    | Date of journal entry.                                 |
| PurchaseDate   | Date of bond purchase.                                 |
| ExpiryDate     | Expiry date of the bond.                               |
| Purchaser      | Name of the purchaser.                                 |
| Prefix         | Prefix code associated with the bond.                  |
| BondNumber     | Number followed by prefix code.                        |
| Denominations  | Value of the bond.                                     |
| PayBranchCode  | Payment branch code (IFSC).                            |
| PayTeller      | Teller ID who issued the bond.                         |

### Receiver Data Table
| Column Name    | Description                                            |
|----------------|--------------------------------------------------------|
| SNo            | Serial number.                                         |
| DateEncashment | Date of bond encashment.                               |
| PartyName      | Name of the political party (recipient).               |
| AccountNum     | Account number of the party.                           |
| Prefix         | Prefix code associated with the bond.                  |
| BondNumber     | Number followed by prefix code.                        |
| Denominations  | Value of the bond.                                     |
| PayBranchCode  | Payment branch code (IFSC).                            |
| PayTeller      | Teller ID who encashed the bond.                       |

### Bank Branch Data Table
| Column Name               | Description                                                |
|---------------------------|------------------------------------------------------------|
| Sl.No.                    | Serial number.                                             |
| State/UT                  | State/UT where the bank is located.                        |
| Name of the Branch & Address | Complete address of the bank.                             |
| City                      | City where the bank branch is located.                     |
| Branch Code No            | IFSC code.                                                 |

## 📈 SQL Queries

### 1. Volume Growth by Quarter
```sql
SELECT
    YEAR(website_sessions.created_at) AS yr,
    QUARTER(website_sessions.created_at) AS qtr,
    COUNT(DISTINCT website_sessions.website_session_id) AS total_sessions,
    COUNT(DISTINCT orders.order_id) AS total_orders
FROM website_sessions
LEFT JOIN orders
    ON website_sessions.website_session_id = orders.website_session_id
GROUP BY 1,2
ORDER BY 1,2;

### 2. Efficiency Improvements by Quarter
```sql
SELECT
    YEAR(website_sessions.created_at) AS yr,
    QUARTER(website_sessions.created_at) AS qtr,
    COUNT(DISTINCT orders.order_id) / COUNT(DISTINCT website_sessions.website_session_id) AS conv_rate,
    SUM(price_usd) / COUNT(DISTINCT orders.order_id) AS rev_per_orders,
    SUM(price_usd) / COUNT(DISTINCT website_sessions.website_session_id) AS rev_per_session
FROM website_sessions
LEFT JOIN orders
    ON website_sessions.website_session_id = orders.website_session_id
GROUP BY 1,2
ORDER BY 1,2;

### 3. Quarterly Orders by Channel
```sql
SELECT
    YEAR(website_sessions.created_at) AS yr,
    QUARTER(website_sessions.created_at) AS qtr,
    COUNT(DISTINCT CASE WHEN utm_source = 'gsearch' AND utm_campaign='nonbrand' THEN orders.order_id ELSE NULL END) AS Gsearch_nonbrand_orders,
    COUNT(DISTINCT CASE WHEN utm_source = 'bsearch' AND utm_campaign='nonbrand' THEN orders.order_id ELSE NULL END) AS Bsearch_nonbrand_orders,
    COUNT(DISTINCT CASE WHEN utm_campaign='brand' THEN orders.order_id ELSE NULL END) AS Brand_overall_orders,
    COUNT(DISTINCT CASE WHEN utm_source IS NULL AND http_referer IS NULL THEN orders.order_id ELSE NULL END) AS direct_type_in_orders,
    COUNT(DISTINCT CASE WHEN utm_source IS NULL AND http_referer IN('https://www.gsearch.com', 'https://www.bsearch.com') THEN orders.order_id ELSE NULL END) AS Organic_search_orders
FROM website_sessions
LEFT JOIN orders
    ON website_sessions.website_session_id = orders.website_session_id
GROUP BY 1,2
ORDER BY 1,2;

### 4. Session-to-Order Conversion Rate by Channel
```sql
SELECT
    YEAR(website_sessions.created_at) AS yr,
    QUARTER(website_sessions.created_at) AS qtr,
    COUNT(DISTINCT CASE WHEN utm_source = 'gsearch' AND utm_campaign='nonbrand' THEN orders.order_id ELSE NULL END)
        / COUNT(DISTINCT CASE WHEN utm_source = 'gsearch' AND utm_campaign='nonbrand' THEN website_sessions.website_session_id ELSE NULL END) AS Gsearch_nonbrand_conv,
    COUNT(DISTINCT CASE WHEN utm_source = 'bsearch' AND utm_campaign='nonbrand' THEN orders.order_id ELSE NULL END)
        / COUNT(DISTINCT CASE WHEN utm_source = 'bsearch' AND utm_campaign='nonbrand' THEN website_sessions.website_session_id ELSE NULL END) AS Bsearch_nonbrand_conv,
    COUNT(DISTINCT CASE WHEN utm_campaign='brand' THEN orders.order_id ELSE NULL END)
        / COUNT(DISTINCT CASE WHEN utm_campaign='brand' THEN website_sessions.website_session_id ELSE NULL END) AS Brand_overall_conv,
    COUNT(DISTINCT CASE WHEN utm_source IS NULL AND http_referer IS NULL THEN orders.order_id ELSE NULL END)
        / COUNT(DISTINCT CASE WHEN utm_source IS NULL AND http_referer IS NULL THEN website_sessions.website_session_id ELSE NULL END) AS direct_type_in_conv,
    COUNT(DISTINCT CASE WHEN utm_source IS NULL AND http_referer IN('https://www.gsearch.com', 'https://www.bsearch.com') THEN orders.order_id ELSE NULL END)
        / COUNT(DISTINCT CASE WHEN utm_source IS NULL AND http_referer IN('https://www.gsearch.com', 'https://www.bsearch.com') THEN website_sessions.website_session_id ELSE NULL END) AS Organic_search_conv
FROM website_sessions
LEFT JOIN orders
    ON website_sessions.website_session_id = orders.website_session_id
GROUP BY 1,2
ORDER BY 1,2;

### 5. Monthly Trending for Revenue and Margin by Product
```sql
 
    YEAR(created_at) AS yr,
    MONTH(created_at) AS mon,
    SUM(CASE WHEN product_id=1 THEN price_usd ELSE NULL END) AS mrfuzzy_rev,
    SUM(CASE WHEN product_id=1 THEN price_usd-cogs_usd ELSE NULL END) AS mrfuzzy_marg,
    SUM(CASE WHEN product_id=2 THEN price_usd ELSE NULL END) AS lovebear_rev,
    SUM(CASE WHEN product_id=2 THEN price_usd-cogs_usd ELSE NULL END) AS lovebear_marg,
    SUM(CASE WHEN product_id=3 THEN price_usd ELSE NULL END) AS birthdaybear_rev,
    SUM(CASE WHEN product_id=3 THEN price_usd-cogs_usd ELSE NULL END) AS birthdaybear_marg,
    SUM(CASE WHEN product_id=4 THEN price_usd ELSE NULL END) AS minibear_rev,
    SUM(CASE WHEN product_id=4 THEN price_usd-cogs_usd ELSE NULL END) AS minibear_marg,
    SUM(price_usd) AS total_revenue,
    SUM(price_usd-cogs_usd) AS total_margin
FROM order_items
GROUP BY 1,2
ORDER BY 1,2;
### 6.Monthly Sessions to the /products Page
```sql
CREATE TEMPORARY TABLE product_pageviews
SELECT 
    website_session_id,
    website_pageview_id,
    created_at AS product_page_seen_at
FROM website_pageviews
WHERE pageview_url='/products';

SELECT 
    YEAR(product_page_seen_at) AS yr,
    MONTH(product_page_seen_at) AS mon,
    COUNT(DISTINCT product_pageviews.website_session_id) AS sessions_to_product_page,
    COUNT(DISTINCT website_pageviews.website_session_id) AS clicked_to_next_page,
    COUNT(DISTINCT website_pageviews.website_session_id)/COUNT(DISTINCT product_pageviews.website_session_id) AS clickthru_rt,
    COUNT(DISTINCT orders.order_id) AS orders,
    COUNT(DISTINCT orders.order_id)/COUNT(DISTINCT product_pageviews.website_session_id) AS product_to_order_rt
FROM product_pageviews
LEFT JOIN website_pageviews
    ON website_pageviews.website_session_id = product_pageviews.website_session_id
    AND website_pageviews.website_pageview_id > product_pageviews.website_pageview_id
