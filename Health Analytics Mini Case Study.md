## Health Analytics Mini Case Study

Have been asked to debug SQL sript outputs to answer the following business questions:

    1. How many unique users exist in the logs dataset?
    2. How many total measurements do we have per user on average?
    3. What about the median number of measurements per user?
    4. How many users have 3 or more measurements?
    5. How many users have 1,000 or more measurements?

In the log data, what is the number and percent of active users who:

    6. Have logged blood glucose measurements?
    7. Have at least 2 types of measurements?
    8. Have all 3 measures - blood glucose, weight and blood pressure?

For users that have blood pressure measurements:
    9. What is the median systolic/diastolic blood pressure values?

## Debug SQL Script
```
-- 1. How many unique users exist in the logs dataset?
--Bug
SELECT
  COUNT DISTINCT user_id
FROM health.user_logs;

--answer
SELECT 
  COUNT (DISTINCT id)
FROM health.user_logs;
```
```

-- for questions 2-8 we created a temporary table
--bug
DROP TABLE IF EXISTS user_measure_count;
CREATE TEMP TABLE user_measure_cout
SELECT
    id,
    COUNT(*) AS measure_count,
    COUNT(DISTINCT measure) as unique_measures
  FROM health.user_logs
  GROUP BY 1; 

--answer
DROP TABLE IF EXISTS user_measure_count;
CREATE TEMPORARY TABLE user_measure_count AS(
  SELECT 
    id,
    COUNT(*) AS measure_count,
    COUNT(DISTINCT measure) as unique_measures
  FROM health.user_logs
  GROUP BY 1); 
```

```

-- 2. How many total measurements do we have per user on average?
--bug
SELECT
  ROUND(MEAN(measure_count))
FROM user_measure_count;

--answer
SELECT
  ROUND(AVG(measure_count))
FROM user_measure_count;

```
```
-- 3. What about the median number of measurements per user?
--bug
SELECT
  PERCENTILE_CONTINUOUS(0.5) WITHIN GROUP (ORDER BY id) AS median_value
FROM user_measure_count;

--answer
SELECT
  PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY measure_count) AS median_value
FROM user_measure_count;
```

```
-- 4. How many users have 3 or more measurements?
--bug
SELECT
  COUNT(*)
FROM user_measure_count
HAVING measure >= 3;

--answer
SELECT
  COUNT(distinct id)
FROM user_measure_count
WHERE measure
```

```
-- 5. How many users have 1,000 or more measurements?
--bug
SELECT
  SUM(id)
FROM user_measure_count
WHERE measure_count >= 1000;

--answer
SELECT
  SUM(id)
FROM user_measure_count
WHERE measure_count >= 1000;
```