# SQL 50

My solutions to the Top 50 SQL questions from LeetCode

[Crack SQL Interview in 50 Qs](https://leetcode.com/studyplan/top-sql-50/)

## Select

### [1757. Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y' AND recyclable = 'Y';
```

### [584. Find Customer Referee](https://leetcode.com/problems/find-customer-referee/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT name
FROM Customer
WHERE referee_id != 2 OR referee_id IS NULL;
```

> In SQL, `NULL` represents an unknown or missing value. When you compare any value with NULL, the result of the comparison is also `NULL`, which is interpreted as `FALSE` in the context of WHERE clauses.

### [595. Big Countries](https://leetcode.com/problems/big-countries/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT name, population, area
FROM World
WHERE area >= 3000000 or population >= 25000000;
```

### [1148. Article Views I](https://leetcode.com/problems/article-views-i/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT DISTINCT author_id as id
FROM Views
WHERE author_id = viewer_id
ORDER BY id;
```

### [1683. Invalid Tweets](https://leetcode.com/problems/invalid-tweets/?envType=study-plan-v2&envId=top-sql-50)
    
```sql
SELECT tweet_id
FROM Tweets
WHERE length(content) > 15
```
------------------------------------------------------------

## Basic Joins

### [1378. Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT unique_id, name
FROM Employees
LEFT JOIN EmployeeUNI
ON EmployeeUNI.id = Employees.id
```

### [1068. Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT product_name, year, price
FROM Sales s
LEFT JOIN Product p
ON s.product_id = p.product_id
```

### [1581. Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT V.customer_id, COUNT(V.customer_id) AS count_no_trans
FROM Visits V
LEFT JOIN Transactions T
ON V.visit_id = T.visit_id
Where transaction_id IS NULL
GROUP BY V.customer_id
```

### [197. Rising Temperature](https://leetcode.com/problems/rising-temperature/description/?envType=study-plan-v2&envId=top-sql-50)
    
```sql
SELECT id
FROM Weather T1
WHERE temperature >
    (
        SELECT temperature
        FROM Weather T2
        WHERE T2.recordDate = DATE_SUB(T1.recordDate, INTERVAL 1 DAY)
        --WHERE T2.recordDate = DATE_ADD(T1.recordDate, INTERVAL -1 DAY)
        --WHERE T2.recordDate = SUBDATE(T1.recordDate, 1)
        --WHERE T2.recordDate = ADDDATED(T1.recordDate, -1)
    )
```

### [1661. Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT T1.machine_id, ROUND(AVG(T2.timestamp - T1.timestamp), 3) AS processing_time
FROM Activity T1
Join Activity T2
ON T1.machine_id = T2.machine_id AND
   T1.process_id = T2.process_id AND
   T1.activity_type = 'start' AND
   T2.activity_type = 'end'
GROUP BY machine_id
```

### [577. Employee Bonus](https://leetcode.com/problems/employee-bonus/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT name, bonus
FROM Employee e
LEFT JOIN Bonus b
ON e.empId = b.empId
WHERE bonus < 1000 OR bonus IS NULL
```

### [1280. Students and Examinations](https://leetcode.com/problems/students-and-examinations/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT s.student_id, student_name, sub.subject_name,
       COUNT(e.student_id) AS attended_exams
FROM Students s
CROSS JOIN Subjects sub
LEFT JOIN Examinations e
ON s.student_id = e.student_id AND sub.subject_name = e.subject_name
GROUP BY s.student_id, sub.subject_name
ORDER BY s.student_id, sub.subject_name;
```

### [570. Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT name
FROM Employee
WHERE id IN
    (
        SELECT managerId
        FROM Employee
        GROUP BY managerId
        HAVING COUNT(managerId) >= 5
    )
```

### [1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT s.user_id,
       ROUND(AVG(IF(action='confirmed', 1, 0)), 2) AS confirmation_rate
FROM Signups s
LEFT JOIN Confirmations c
ON s.user_id = c.user_id
GROUP BY s.user_id
```

------------------------------------------------------------

## Basic Aggregate Functions

### [620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT *
FROM Cinema
WHERE (id % 2) = 1 AND description <> "boring"
ORDER BY rating DESC;
```

### [1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT p.product_id, IFNULL(ROUND(SUM(units * price) / SUM(units), 2), 0) AS average_price
FROM Prices p
LEFT JOIN UnitsSold u
ON (p.product_id = u.product_id) AND (purchase_date BETWEEN start_date AND end_date)
GROUP BY product_id
```

### [1075. Project Employees I](https://leetcode.com/problems/project-employees-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT project_id, ROUND(AVG(experience_years), 2) AS average_years
FROM Project p
LEFT JOIN Employee e
ON p.employee_id = e.employee_id
GROUP BY project_id

--OR
-- SELECT project_id, ROUND(SUM(experience_years)/ COUNT(p.employee_id), 2) AS average_years
-- FROM Project p
-- LEFT JOIN Employee e
-- ON p.employee_id = e.employee_id
-- GROUP BY project_id
```

### [1633. Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT contest_id, ROUND(COUNT(r.user_id) / (SELECT COUNT(user_id) FROM Users) * 100, 2) AS percentage
FROM Register r
GROUP BY contest_id
ORDER BY percentage DESC, contest_id ASC;
```

### [1211. Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT query_name,
    ROUND(SUM(rating / position) / COUNT(query_name), 2) AS quality,
    ROUND(SUM(IF(rating < 3, 1, 0)) / COUNT(query_name) * 100, 2) AS poor_query_percentage
FROM Queries
WHERE query_name IS NOT NULL
GROUP BY query_name
```

### [1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT LEFT(trans_date, 7) AS month, country,
    COUNT(id) AS trans_count,
    SUM(state = 'approved') AS approved_count,
    SUM(amount) AS trans_total_amount ,
    SUM(amount * (state = 'approved')) AS approved_total_amount
FROM Transactions
GROUP BY month, country
```

### [1174. Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT ROUND(AVG(order_date = customer_pref_delivery_date) * 100, 2) AS immediate_percentage
FROM Delivery
WHERE (customer_id, order_date) IN (
    SELECT customer_id, MIN(order_date)
    FROM Delivery
    GROUP BY customer_id
)
```

### [550. Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT ROUND( COUNT(player_id) / (SELECT COUNT(DISTINCT player_id) FROM Activity), 2) AS fraction
FROM Activity
WHERE
    (player_id, DATE_SUB(event_date, INTERVAL 1 DAY))
    IN(
            SELECT player_id, MIN(event_date) AS first_login
            FROM Activity
            GROUP BY player_id
    )
```

---------------------------------------------------------------

## Sorting and Grouping

### [2356. Number of Unique Subjects Taught by Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT teacher_id, COUNT(DISTINCT subject_id) AS cnt
FROM Teacher
GROUP BY teacher_id
```

### [1141. User Activity for the Past 30 Days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT activity_date AS day, COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date BETWEEN '2019-06-28' AND '2019-07-27'
GROUP BY activity_date
```

### [1070. Product Sales Analysis III](https://leetcode.com/problems/product-sales-analysis-iii/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT product_id, year AS first_year, quantity, price
FROM Sales
WHERE (product_id, year) IN (
    SELECT product_id, MIN(year)
    FROM Sales
    GROUP BY product_id
)
```

### [596. Classes More Than 5 Students](https://leetcode.com/problems/classes-more-than-5-students/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5
```

### [1729. Find Followers Count](https://leetcode.com/problems/find-followers-count/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT user_id, COUNT(follower_id) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id
```

### [619. Biggest Single Number](https://leetcode.com/problems/biggest-single-number/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT IF(COUNT(num) = 1, num, NULL) AS num
FROM MyNumbers
GROUP BY num
ORDER BY num DESC
LIMIT 1

--OR
-- SELECT MAX(num) AS num
-- FROM (
--     SELECT num
--     FROM MyNumbers
--     GROUP BY num
--     HAVING COUNT(num) = 1
--     ) myTable;
```

### [1045. Customers Who Bought All Products](https://leetcode.com/problems/customers-who-bought-all-products/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(*) FROM Product)
```
------------------------------------------------------------

## Advanced Select and Joins

### [1731. The Number of Employees Which Report to Each Employee](https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT mgr.employee_id, mgr.name,
    COUNT(e1.employee_id) AS reports_count,
    ROUND(AVG(e1.age), 0) AS average_age
FROM Employees mgr
JOIN Employees e1
ON mgr.employee_id = e1.reports_to
GROUP BY mgr.employee_id
ORDER BY mgr.employee_id

```

### [1789. Primary Department for Each Employee](https://leetcode.com/problems/primary-department-for-each-employee/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT employee_id, department_id
FROM Employee
WHERE primary_flag = "Y" OR
    employee_id in (
        SELECT employee_id
        FROM Employee
        GROUP BY employee_id
        HAVING COUNT(employee_id) = 1
        )
```

### [610. Triangle Judgement](https://leetcode.com/problems/triangle-judgement/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT x, y, z,
    CASE
        WHEN x + y > z AND y + z > x AND x + z > y THEN 'Yes'
        ELSE 'No'
    END AS triangle
FROM Triangle;
```

### [180. Consecutive Numbers](https://leetcode.com/problems/consecutive-numbers/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT DISTINCT l1.num AS ConsecutiveNums
FROM Logs l1, Logs l2, Logs l3
WHERE l1.id = l2.id + 1 AND l2.id = l3.id + 1 AND
      l1.num = l2.num AND l2.num = l3.num
```

### [1164. Product Price at a Given Date](https://leetcode.com/problems/product-price-at-a-given-date/description/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT distinct product_id, 10 as price
FROM Products
GROUP BY product_id
HAVING MIN(change_date) > "2019-08-16"

UNION

SELECT product_id, new_price as price
FROM Products
WHERE (product_id, change_date) IN (
    SELECT product_id, max(change_date) as recent_date
    FROM Products
    WHERE change_date <= "2019-08-16"
    GROUP BY product_id
)

GROUP BY product_id
HAVING MAX(change_date) <= "2019-08-16"
```

### [1204. Last Person to Fit in the Bus](https://leetcode.com/problems/last-person-to-fit-in-the-bus/?envType=study-plan-v2&envId=top-sql-50)

```sql
SELECT person_name
FROM Queue q1
WHERE 1000 >= (
    SELECT SUM(weight)
    FROM Queue q2
    WHERE q2.turn <= q1.turn
)
ORDER BY turn DESC
LIMIT 1;
```

### [1907. Count Salary Categories](https://leetcode.com/problems/count-salary-categories/?envType=study-plan-v2&envId=top-sql-50)

```sql
(SELECT 'Low Salary' AS category, COUNT(*) AS accounts_count FROM Accounts WHERE income < 20000)
UNION
(SELECT 'Average Salary' AS category, COUNT(*) AS accounts_count FROM Accounts WHERE income >= 20000 AND income <= 50000)
UNION
(SELECT 'High Salary' AS category, COUNT(*) AS accounts_count FROM Accounts WHERE income > 50000)
```
