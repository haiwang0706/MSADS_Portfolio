--Question # 0:
SELECT * FROM marketing_data Limit 2;​

--Question # 1:
SELECT geo, sum(clicks)
From marketing_data
Group by geo;

--Question # 2:
SELECT store_location, sum(revenue)
FROM store_revenue 
Group by store_location;

--Question # 3:
SELECT m.date,  m.geo, m.impressions, m.clicks, s.revenue
FROM marketing_data m  FULL OUTER JOIN store_revenue s  ON m.date = s.date and 'United States-' || m.geo = s.store_location;

--Question # 4:
WITH temp AS(
    Select *, Round(m.clicks*100/m.impressions,2) as clickrate
    FROM marketing_data m FULL OUTER JOIN store_revenue s ON m.date = s.date and 'United States-' || m.geo = s.store_location
    GROUP BY m.date,m.geo
)

SELECT date,geo,brand_id,revenue,click_rate
FROM temp
Order by click_rate DESC
-- I consider the clickrate which represent 'number of click/impression' and order by Descending, it can show top brand in different states of US.The higher the click rate the more efficient store which is CA brand_id 1. Also, we can set the limited to ignore the low clickrate

--Question # 5:
WITH temp AS(
    Select store_location, sum(s.revenue) as sum_revenue
    FROM store_revenue s 
    Group by store_location
)
Select sum_revenue
FROM temp
Order by sum_revenue DESC
Limit 10;
