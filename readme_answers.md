How many users do we have?

SELECT
COUNT(DISTINCT user_id)
FROM DEV_DB.DBT_JAHIDBENHAGOUGCARTIERCOM.STG_USERS

--> 130 users



On average, how many orders do we receive per hour?

SELECT
    avg(order_count)
FROM (
    SELECT
        date_trunc('HOUR',created_at), 
        count(order_id) AS order_count
    FROM DEV_DB.DBT_JAHIDBENHAGOUGCARTIERCOM.STG_ORDERS
    GROUP BY 1
)

--> 7.520833 orders



On average, how long does an order take from being placed to being delivered?

SELECT
        floor(avg(datediff(minute,created_at,delivered_at))/60/24) AS days,
        floor((avg(datediff(minute,created_at,delivered_at))-(floor(avg(datediff(minute,created_at,delivered_at))/60/24)*24*60))/60) AS hours,
        floor(avg(datediff(minute,created_at,delivered_at))-(floor(avg(datediff(minute,created_at,delivered_at))/60/24)*24*60)-(floor((avg(datediff(minute,created_at,delivered_at))-(floor(avg(datediff(minute,created_at,delivered_at))/60/24)*24*60))/60))*60) AS minutes
    FROM DEV_DB.DBT_JAHIDBENHAGOUGCARTIERCOM.STG_ORDERS
    
--> 3 days, 21 hours and 24 minutes



How many users have only made one purchase? Two purchases? Three+ purchases?

WITH temp_table AS (

SELECT
    user_id, 
    count(order_id) AS order_count
FROM DEV_DB.DBT_JAHIDBENHAGOUGCARTIERCOM.STG_ORDERS
GROUP BY 1

)

SELECT 
    count(user_id)
FROM temp_table
WHERE order_count=1 (2, >2)

--> 25, 28, 71

Note: you should consider a purchase to be a single order. In other words, if a user places one order for 3 products, they are considered to have made 1 purchase.

On average, how many unique sessions do we have per hour?

SELECT
    avg(session_count)
FROM (
    SELECT
        date_trunc('HOUR',created_at), 
        count(session_id) AS session_count
    FROM DEV_DB.DBT_JAHIDBENHAGOUGCARTIERCOM.STG_EVENTS
    GROUP BY 1
)

--> 61 sessions

