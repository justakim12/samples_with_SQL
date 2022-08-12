## Changing Timestamp with CAST and TIMESTAMP Clause
- Count number of reviews created starting from 2019-03-01

```sql
SELECT count(*)
FROM reviews
WHERE TIMESTAMP(CAST(created.`$date` AS BIGINT), 'Asia/Seoul') > '2019-03-01 00:00:00'
```

## COUNT DISTINCT Clause
- Count distinct number of users who wrote a review

```sql
SELECT COUNT(DISTINCT reviews.names)
FROM reviews_v1 AS reviews
WHERE TIMESTAMP(CAST(created.`$date` AS BIGINT), 'Asia/Seoul') > '2019-03-01 00:00:00'
```



## GROUP BY and ORDER BY Clause
- Find how many reviews each user created starting from 2020-08-01

```sql
SELECT COUNT(reviews.id) as review_count,
       reviews.name
FROM reviews
WHERE TIMESTAMP(CAST(created.`$date` AS BIGINT), 'Asia/Seoul') > '2020-08-01 00:00:00'
GROUP BY reviews.name
ORDER BY review_count DESC
```

## HAVING Clause
- Find users who created more than 10 reviews

```sql
SELECT COUNT(reviews.id) AS review_count,
       reviews.fulldocument.name
FROM reviews
GROUP BY reviews.fulldocument.name
HAVING review_count >= 10
```

## LEFT JOIN and WHERE Clause
- Find the average rating of users who have a current grade of green 

```sql
SELECT userstats.id,
       userstats.avgRating,
       users.currentGrade
FROM userstats_v1 AS userstats
       LEFT JOIN users_v1 AS users
                 ON userstats.id = users.name
WHERE users.grade LIKE '%green%'
```

## LENGTH, SIZE and OR Clause
- Find reviews that have a body longer than 1 or media array larger than 1. 

```sql
SELECT id,
       LENGTH(body),
       SIZE(array_media)
FROM reviews
WHERE (LENGTH(body) >= 1)
OR (SIZE(array_media) >= 1)
```

## CONCAT and ARRAY_CONTAINS Clause
- Find where array contains the string 'cool' and create full URL by combining strings

```sql
SELECT CONCAT('https://website_name/', reviews.id) AS url, 
       reviews.name
FROM reviews
WHERE ARRAY_CONTAINS(reviews.class, 'cool)
       
```  

## WITH AS Clause
- Count reviews which have a rating higher than 4

```sql
WITH temp AS (
    SELECT reviews.id
    FROM reviews
    WHERE reviews.rating >= 4)

SELECT COUNT(*) 
FROM temp
```
