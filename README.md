# CTE-queries question and answer

#Question No.1:- Identify a table in the sakila database that violates 1NF.Explain how would normalize to achive 1NF:-

To identify a table in the Sakila database that violates the First Normal Form (1NF), let's first understand what 1NF requires. A table is in 1NF if:
.Each column contains only atomic (indivisible) values. This means that each cell in a table should contain only one value, not a set or a list of values.
.Each column contains values of a single type.
.Each column must have a unique name.
.The order in which data is stored does not matter.

# How to Normalize to Achieve 1NF:-
To normalize this table to achieve 1NF, follow these steps:
.Remove Repeating Groups: The column that violates 1NF by holding multiple values (actors) should be split into multiple rows.
.Create Separate Tables: Split the table into two tables: one for movies and one for actors, with a junction table to manage the many-to-many relationship.

#Question No.2:- Choose a table in Sakila and describe how you would determine whether it is in 2NF.If it violates 2NF,
explain the steps to normalize it.

To determine whether a table in the Sakila database is in the Second Normal Form (2NF), you need to understand what 2NF requires. A table is in 2NF if:

1. **It is already in the First Normal Form (1NF).**
2. **All non-key attributes are fully functionally dependent on the entire primary key, not just a part of it.**

In other words, for tables with a composite primary key (a primary key that consists of more than one column), each non-key column must depend on the whole composite key, not just a part of it.

### Example of a Table in Sakila

Let’s use the `film_actor` table from the Sakila database as an example to illustrate the concept of 2NF.

**`film_actor` Table:**

| film_id | actor_id | actor_name         | film_title          |
|---------|----------|--------------------|---------------------|
| 1       | 1        | Keanu Reeves       | The Matrix          |
| 1       | 2        | Laurence Fishburne | The Matrix          |
| 2       | 3        | Leonardo DiCaprio  | Inception           |
| 3       | 1        | Keanu Reeves       | The Matrix Reloaded |
| 3       | 4        | Carrie-Anne Moss   | The Matrix Reloaded |

In this table:

- **Primary Key:** Composite key (film_id, actor_id)
- **Non-Key Attributes:** `actor_name`, `film_title`

### Checking for 2NF

1. **Is the Table in 1NF?**
   - Yes, each cell contains atomic values, so the table is in 1NF.

2. **Functional Dependencies:**
   - `film_id` and `actor_id` together determine the `actor_name` and `film_title`.
   - `film_id` determines `film_title` (since each film has a unique title).
   - `actor_id` determines `actor_name` (since each actor has a unique name).

### Determining if the Table Violates 2NF

To be in 2NF, all non-key attributes must be fully functionally dependent on the entire composite primary key. Let’s examine the functional dependencies:

- `film_id` → `film_title`
- `actor_id` → `actor_name`

Here, `film_title` depends only on `film_id`, not on the combination of `film_id` and `actor_id`. Similarly, `actor_name` depends only on `actor_id`, not on the combination of `film_id` and `actor_id`. This means:

- `film_title` is dependent on part of the composite key (`film_id`), not the entire key.
- `actor_name` is dependent on part of the composite key (`actor_id`), not the entire key.

Since `film_title` and `actor_name` are not fully dependent on the whole composite key, the `film_actor` table violates 2NF.

### Normalizing the Table to Achieve 2NF

To normalize this table to 2NF, we need to remove partial dependencies by splitting it into separate tables.

**Steps for Normalization:**

1. **Create a `film` Table:**

   | film_id | film_title         |
   |---------|--------------------|
   | 1       | The Matrix         |
   | 2       | Inception          |
   | 3       | The Matrix Reloaded|

   Here, `film_id` is the primary key, and `film_title` is dependent on `film_id`.

2. **Create an `actor` Table:**

   | actor_id | actor_name         |
   |----------|--------------------|
   | 1        | Keanu Reeves       |
   | 2        | Laurence Fishburne |
   | 3        | Leonardo DiCaprio  |
   | 4        | Carrie-Anne Moss   |

   Here, `actor_id` is the primary key, and `actor_name` is dependent on `actor_id`.

3. **Create a `film_actor` Junction Table:**

   | film_id | actor_id |
   |---------|----------|
   | 1       | 1        |
   | 1       | 2        |
   | 2       | 3        |
   | 3       | 1        |
   | 3       | 4        |

   This table captures the many-to-many relationship between films and actors, where `film_id` and `actor_id` together form the composite primary key.

**Normalized Schema:**

1. **`film` Table** (No partial dependency issue here)
2. **`actor` Table** (No partial dependency issue here)
3. **`film_actor` Table** (Properly manages many-to-many relationships)

In this normalized design:

- The `film` table contains information about films.
- The `actor` table contains information about actors.
- The `film_actor` table manages the relationships between films and actors without introducing partial dependencies.

# Question No.3 :- Identify a table in Sakila that violates 3NF. Describe the transitive dependencies present and outline the
steps to normalize the table to 3NF.

To identify a table in the Sakila database that violates the Third Normal Form (3NF), let’s first review the requirements for 3NF. A table is in 3NF if:

1. It is already in Second Normal Form (2NF).
2. There are no transitive dependencies.This means that all non-key attributes must be directly dependent on the primary key, not on other non-key attributes.

### Example of a Table Violating 3NF

Let’s use the `staff` table from the Sakila database as an example. This table can be analyzed for potential 3NF violations.

`staff` Table:

| staff_id | first_name | last_name | address_id | email                  | store_id | store_name  |
|----------|------------|-----------|------------|------------------------|----------|-------------|
| 1        | John       | Doe       | 1          | john.doe@example.com   | 1        | Main Store  |
| 2        | Jane       | Smith     | 2          | jane.smith@example.com | 1        | Main Store  |
| 3        | Mike       | Brown     | 3          | mike.brown@example.com | 2        | Branch Store|

Primary Key: `staff_id`

### Checking for 3NF

1. Is the Table in 2NF.
   - Yes, the table is in 2NF. Each non-key attribute is fully functionally dependent on the primary key `staff_id`.

2. Transitive Dependencies:
   - `address_id` → `address` (assuming there's an `address` table where `address_id` is the key)
   - `store_id` → `store_name` (assuming there's a `store` table where `store_id` is the key)

   Here, we can see that `store_name` depends on `store_id`, which is not a direct function of `staff_id` but rather through `store_id`. Similarly, `address` depends on `address_id`.

### Identifying Transitive Dependencies:

- Transitive Dependency on `store_name`:** `staff_id` → `store_id` → `store_name`
  - `store_name` depends on `store_id`, which is indirectly dependent on `staff_id` through `store_id`.

- Transitive Dependency on Address:** `staff_id` → `address_id` → `address`
  - `address` depends on `address_id`, which is indirectly dependent on `staff_id` through `address_id`.

### Steps to Normalize to 3NF

To normalize the `staff` table to 3NF, we need to eliminate these transitive dependencies by decomposing the table into separate tables.

1. Create a `staff` Table:

   `staff` Table:

   | staff_id | first_name | last_name | address_id | email                  | store_id |
   |----------|------------|-----------|------------|------------------------|----------|
   | 1        | John       | Doe       | 1          | john.doe@example.com   | 1        |
   | 2        | Jane       | Smith     | 2          | jane.smith@example.com | 1        |
   | 3        | Mike       | Brown     | 3          | mike.brown@example.com | 2        |

   Here, `staff_id` is the primary key. We only keep direct dependencies.

#Question No.4:- Take a specific table in Sakila and guide through the process of normalizing it from the initial
unormalized form up to at least 2NF.

To illustrate the normalization process from the initial unnormalized form up to at least Second Normal Form (2NF), let’s use the `rental` table from the Sakila database. This table is a good example because it contains multiple attributes and relationships.

### Unnormalized Form (UNF)

**`rental` Table:**

| rental_id | rental_date | inventory_id | customer_id | first_name | last_name | address_id | email                  | return_date | staff_id | staff_name    |
|-----------|-------------|--------------|-------------|------------|-----------|------------|------------------------|-------------|----------|---------------|
| 1         | 2024-07-01  | 1001         | 1           | John       | Doe       | 1          | john.doe@example.com   | 2024-07-10  | 1        | Alice Johnson |
| 2         | 2024-07-02  | 1002         | 2           | Jane       | Smith     | 2          | jane.smith@example.com | 2024-07-12  | 1        | Alice Johnson |
| 3         | 2024-07-03  | 1003         | 1           | John       | Doe       | 1          | john.doe@example.com   | 2024-07-15  | 2        | Bob Brown     |

### Identifying the Primary Key and Functional Dependencies

**Primary Key:** `rental_id`

**Functional Dependencies:**

- `rental_id` → `rental_date`, `inventory_id`, `customer_id`, `first_name`, `last_name`, `address_id`, `email`, `return_date`, `staff_id`, `staff_name`

### 1NF: First Normal Form

A table is in 1NF if it has only atomic values and each column contains only one value. The given table already adheres to 1NF because:

- Each cell contains a single value.
- There are no repeating groups or arrays.

### 2NF: Second Normal Form

A table is in 2NF if:

1. **It is in 1NF.**
2. **All non-key attributes are fully functionally dependent on the entire primary key.**

In the `rental` table:

- `rental_id` is the primary key.
- `first_name`, `last_name`, `address_id`, `email`, and `staff_name` are not fully dependent on the primary key but on other non-key attributes like `customer_id` and `staff_id`.

**Steps to Normalize to 2NF:**

1. **Identify Partial Dependencies:**
   - `first_name`, `last_name`, `address_id`, and `email` depend only on `customer_id`, not on `rental_id`.
   - `staff_name` depends only on `staff_id`, not on `rental_id`.

2. **Decompose the Table:**

   **1. Create `rental` Table:**

   This table will store the rental transactions, removing customer and staff details.

   | rental_id | rental_date | inventory_id | customer_id | staff_id | return_date |
   |-----------|-------------|--------------|-------------|----------|-------------|
   | 1         | 2024-07-01  | 1001         | 1           | 1        | 2024-07-10  |
   | 2         | 2024-07-02  | 1002         | 2           | 1        | 2024-07-12  |
   | 3         | 2024-07-03  | 1003         | 1           | 2        | 2024-07-15  |

   **2. Create `customer` Table:**

   This table will store customer details.

   | customer_id | first_name | last_name | address_id | email                  |
   |-------------|------------|-----------|------------|------------------------|
   | 1           | John       | Doe       | 1          | john.doe@example.com   |
   | 2           | Jane       | Smith     | 2          | jane.smith@example.com |

   **3. Create `staff` Table:**

   This table will store staff details.

   | staff_id | staff_name    |
   |----------|---------------|
   | 1        | Alice Johnson |
   | 2        | Bob Brown     |

### Normalized Schema:

1. **`rental` Table:**

   | rental_id | rental_date | inventory_id | customer_id | staff_id | return_date |
   |-----------|-------------|--------------|-------------|----------|-------------|
   | 1         | 2024-07-01  | 1001         | 1           | 1        | 2024-07-10  |
   | 2         | 2024-07-02  | 1002         | 2           | 1        | 2024-07-12  |
   | 3         | 2024-07-03  | 1003         | 1           | 2        | 2024-07-15  |

2. **`customer` Table:**

   | customer_id | first_name | last_name | address_id | email                  |
   |-------------|------------|-----------|------------|------------------------|
   | 1           | John       | Doe       | 1          | john.doe@example.com   |
   | 2           | Jane       | Smith     | 2          | jane.smith@example.com |

3. **`staff` Table:**

   | staff_id | staff_name    |
   |----------|---------------|
   | 1        | Alice Johnson |
   | 2        | Bob Brown     |

In this normalized schema:

- The `rental` table contains only data related to the rental transaction.
- The `customer` table contains data about the customer.
- The `staff` table contains data about the staff members.

Each non-key attribute in the `rental` table is now fully functionally dependent on the primary key `rental_id`, thus achieving 2NF. The decomposition removes partial dependencies and maintains a clear separation of concerns.

#Question No.5:- Write a query using a CTE to retrieve the distinct list of actor names and the number of films they have
acted it from the and tables.
To retrieve a distinct list of actor names and the number of films they have acted in using a Common Table Expression (CTE) in SQL, you'll need to work with the actor, film_actor, and film tables from the Sakila database.

Here's a step-by-step breakdown of the query:

Join the actor and film_actor Tables: To get each actor’s participation in films.
Aggregate the Results: Count the number of films each actor has participated in.
Select Distinct Actor Names and Their Film Counts.
Here’s how you can write this query using a CTE:
-- Define the CTE to get actor and film count information
WITH ActorFilmCount AS (
    SELECT
        a.actor_id,
        a.first_name,
        a.last_name,
        COUNT(fa.film_id) AS film_count
    FROM
        actor a
    JOIN
        film_actor fa ON a.actor_id = fa.actor_id
    GROUP BY
        a.actor_id, a.first_name, a.last_name
)

-- Select distinct actor names and their film counts
SELECT
    DISTINCT
    afc.first_name || ' ' || afc.last_name AS actor_name,
    afc.film_count
FROM
    ActorFilmCount afc
ORDER BY
    afc.film_count DESC;

#Question No.6:-Use a recursive CTE to ge?erate a hierarchical list of categories and their subcategories from the
table in Sakila.

To generate a hierarchical list of categories and their subcategories from the Sakila database using a recursive Common Table Expression (CTE), you would use the `category` table, which contains information about film categories and their hierarchical relationships.

In the Sakila database, there is no direct `category` table with a self-referencing relationship in the standard schema. However, for this example, let’s assume we have a `category` table with a hierarchical relationship, which is a common design pattern for categories and subcategories.

Let’s assume the table structure is as follows:

**`category` Table:**

| category_id | category_name | parent_id |
|-------------|---------------|-----------|
| 1           | Action        | NULL      |
| 2           | Comedy        | NULL      |
| 3           | Thriller      | NULL      |
| 4           | Sci-Fi        | 1         |
| 5           | Adventure     | 1         |
| 6           | Romantic      | 2         |
| 7           | Stand-up      | 2         |

In this structure:
- `category_id` is the unique identifier for each category.
- `category_name` is the name of the category.
- `parent_id` is the ID of the parent category (NULL for top-level categories).

### Recursive CTE Query

To generate a hierarchical list of categories and their subcategories, you can use a recursive CTE as follows:
-- Define the recursive CTE
WITH RECURSIVE CategoryHierarchy AS (
    -- Anchor member: select top-level categories (those without a parent)
    SELECT
        category_id,
        category_name,
        parent_id,
        1 AS level,
        CAST(category_name AS VARCHAR(255)) AS path
    FROM
        category
    WHERE
        parent_id IS NULL
    
    UNION ALL
    
    -- Recursive member: select subcategories
    SELECT
        c.category_id,
        c.category_name,
        c.parent_id,
        ch.level + 1 AS level,
        CAST(ch.path || ' -> ' || c.category_name AS VARCHAR(255)) AS path
    FROM
        category c
    INNER JOIN
        CategoryHierarchy ch ON c.parent_id = ch.category_id
)

-- Select from the CTE
SELECT
    category_id,
    category_name,
    parent_id,
    level,
    path
FROM
    CategoryHierarchy
ORDER BY
    path;

#Question No.7:- Create a CTE that combines i?formation from the and tables to display the film title, language
name, and rental rate.
To create a Common Table Expression (CTE) that combines information from the `film`, `language`, and `rental` tables in the Sakila database, you can follow these steps:

1. **Join the `film` table with the `language` table** to get the film title and language name.
2. **Join this result with the `rental` table** to include the rental rate information.

### Table Structure:

- **`film`**: Contains information about films, including `film_id`, `title`, `language_id`, and `rental_rate`.
- **`language`**: Contains information about languages, including `language_id` and `name`.
- **`rental`**: Contains rental transactions, but for this query, we will focus on the film information rather than the specific rental details.

### CTE Query:

Here’s how you can write a CTE to combine this information:

```sql
-- Define the CTE to combine film, language, and rental information
WITH FilmLanguageRental AS (
    SELECT
        f.title AS film_title,
        l.name AS language_name,
        f.rental_rate
    FROM
        film f
    JOIN
        language l ON f.language_id = l.language_id
)

-- Select from the CTE
SELECT
    film_title,
    language_name,
    rental_rate
FROM
    FilmLanguageRental
ORDER BY
    film_title;
```

#Question No.8:- Write a query using a CTE to find the total revenue generated by each customer (sum of payments) from
the and tables.
To find the total revenue generated by each customer (i.e., the sum of payments) using a Common Table Expression (CTE), you need to join the `payment` and `customer` tables. The `payment` table contains details about each payment made, including the amount paid, and the `customer` table contains customer information.

Here’s a step-by-step approach to writing this query using a CTE:

1. **Join the `payment` and `customer` Tables**: To associate payments with customers.
2. **Aggregate the Results**: Calculate the total payment amount for each customer.

### Table Structure:

- **`payment`**: Contains payment details with columns such as `payment_id`, `customer_id`, and `amount`.
- **`customer`**: Contains customer details with columns such as `customer_id` and `first_name`, `last_name`.

### CTE Query:

Here’s how you can write the query using a CTE to find the total revenue generated by each customer:

```sql
-- Define the CTE to calculate total payments for each customer
WITH CustomerRevenue AS (
    SELECT
        c.customer_id,
        c.first_name,
        c.last_name,
        SUM(p.amount) AS total_revenue
    FROM
        customer c
    JOIN
        payment p ON c.customer_id = p.customer_id
    GROUP BY
        c.customer_id, c.first_name, c.last_name
)

-- Select from the CTE
SELECT
    customer_id,
    first_name,
    last_name,
    total_revenue
FROM
    CustomerRevenue
ORDER BY
    total_revenue DESC;
```

#Question No.9:-Utilize a CTE with a window function to rank films based on their rental duration from the table.
To rank films based on their rental duration using a Common Table Expression (CTE) with a window function, you need to follow these steps:

1. **Use a CTE to aggregate or prepare data.**
2. **Apply a window function to rank films according to their rental duration.**

In the Sakila database, the `film` table includes a `rental_duration` column that specifies how long a film can be rented. We will use a window function to rank films based on this `rental_duration`.

### CTE with Window Function Query

Here's how you can create a CTE to rank films by their rental duration:

```sql
-- Define the CTE to rank films based on rental duration
WITH RankedFilms AS (
    SELECT
        film_id,
        title,
        rental_duration,
        ROW_NUMBER() OVER (ORDER BY rental_duration DESC) AS rank
    FROM
        film
)

-- Select from the CTE
SELECT
    film_id,
    title,
    rental_duration,
    rank
FROM
    RankedFilms
ORDER BY
    rank;
```


#Question No.10:-Create a CTE to list customers who have made more that two rentals, a?d the? join this CTE with the customer
table to retrieve additional customer details.
To list customers who have made more than two rentals and then retrieve additional customer details using a Common Table Expression (CTE), follow these steps:

1. **Create a CTE** to count the number of rentals per customer and filter for those with more than two rentals.
2. **Join this CTE** with the `customer` table to retrieve additional customer details.

### Step-by-Step Query:

**Assumptions:**
- **`payment`** table contains `customer_id` and rental details.
- **`customer`** table contains customer details like `customer_id`, `first_name`, `last_name`, `email`, etc.

### CTE Query

```sql
-- Define the CTE to list customers with more than two rentals
WITH FrequentRenters AS (
    SELECT
        p.customer_id,
        COUNT(p.payment_id) AS rental_count
    FROM
        payment p
    GROUP BY
        p.customer_id
    HAVING
        COUNT(p.payment_id) > 2
)

-- Join the CTE with the customer table to retrieve additional customer details
SELECT
    c.customer_id,
    c.first_name,
    c.last_name,
    c.email,
    c.address_id,
    c.create_date,
    fr.rental_count
FROM
    FrequentRenters fr
JOIN
    customer c ON fr.customer_id = c.customer_id
ORDER BY
    fr.rental_count DESC;
```

#Question No.11:-Write a query using a CTE to find the total number of rentals made each month, considering the
from the rental table.
To find the total number of rentals made each month using a Common Table Expression (CTE) in SQL, you'll first need to aggregate rental data by month. This involves extracting the year and month from the `rental_date` and then counting the number of rentals for each month.

Here’s a step-by-step approach:

1. **Create a CTE** to extract the year and month from the `rental_date` and count the rentals.
2. **Select from the CTE** to get the total number of rentals per month.

### CTE Query

```sql
-- Define the CTE to calculate total rentals per month
WITH MonthlyRentals AS (
    SELECT
        DATE_TRUNC('month', rental_date) AS rental_month, -- Truncate to the start of the month
        COUNT(*) AS total_rentals
    FROM
        rental
    GROUP BY
        DATE_TRUNC('month', rental_date)
)

-- Select from the CTE to display the results
SELECT
    rental_month,
    total_rentals
FROM
    MonthlyRentals
ORDER BY
    rental_month;
```

#Question No.12:-Use a CTE to pivot the data from the payment table to display the total payments made by each customer in
separate columns for different payment methods.
To pivot data from the `payment` table and display the total payments made by each customer in separate columns for different payment methods using a Common Table Expression (CTE), you will follow these steps:

1. **Create a CTE** to calculate the total payments for each customer and each payment method.
2. **Use conditional aggregation** to pivot the data and display totals in separate columns for each payment method.

### Assumptions:
- The `payment` table contains columns such as `customer_id`, `amount`, and `payment_method` (though typically payment method would be represented in a separate table, we'll assume it's directly in the `payment` table for this example).

### SQL Query

```sql
-- Define the CTE to aggregate payments by customer and payment method
WITH PaymentSummary AS (
    SELECT
        customer_id,
        payment_method,
        SUM(amount) AS total_amount
    FROM
        payment
    GROUP BY
        customer_id, payment_method
)

-- Pivot the data to show total payments per customer for each payment method
SELECT
    customer_id,
    COALESCE(SUM(CASE WHEN payment_method = 'Credit Card' THEN total_amount ELSE 0 END), 0) AS credit_card_total,
    COALESCE(SUM(CASE WHEN payment_method = 'Debit Card' THEN total_amount ELSE 0 END), 0) AS debit_card_total,
    COALESCE(SUM(CASE WHEN payment_method = 'Cash' THEN total_amount ELSE 0 END), 0) AS cash_total
FROM
    PaymentSummary
GROUP BY
    customer_id
ORDER BY
    customer_id;
```

### Explanation:

1. **CTE Definition (`PaymentSummary`):**
   - **SELECT Clause:** Retrieves `customer_id`, `payment_method`, and calculates the total payment amount (`SUM(amount)`) for each combination of `customer_id` and `payment_method`.
   - **FROM Clause:** Specifies the `payment` table as the source of data.
   - **GROUP BY Clause:** Groups the results by `customer_id` and `payment_method` to aggregate payments.

2. **Pivoting the Data:**
   - **SELECT Clause:** Uses conditional aggregation to pivot the `payment_summary` data. Each `CASE` statement calculates the total payment for a specific `payment_method`:
     - `COALESCE` is used to handle cases where a payment method might not exist for a customer, ensuring that it returns `0` if no payments are found for that method.
   - **GROUP BY Clause:** Groups the results by `customer_id` to ensure each customer’s totals are aggregated properly.

3. **ORDER BY Clause:**
   - Orders the results by `customer_id` to present the data in a readable format.

#Question No.13:-Create a CTE to generate a report showing pairs of actors who have appeared in the same film together,
using the Film_actor table.
To generate a report showing pairs of actors who have appeared in the same film together using the `film_actor` table, you need to:

1. **Create a CTE** to find all pairs of actors who have acted in the same film.
2. **Select from the CTE** to display these pairs.

Here's a step-by-step guide to writing this query:

### Table Structure

- **`film_actor`**: Contains columns `film_id` and `actor_id`, representing which actors have appeared in which films.

### Query

```sql
-- Define the CTE to find actor pairs appearing in the same film
WITH ActorPairs AS (
    SELECT
        fa1.actor_id AS actor1_id,
        fa2.actor_id AS actor2_id,
        fa1.film_id
    FROM
        film_actor fa1
    JOIN
        film_actor fa2
        ON fa1.film_id = fa2.film_id
        AND fa1.actor_id < fa2.actor_id
)

-- Select from the CTE to display the pairs of actors
SELECT
    ap.actor1_id,
    ap.actor2_id,
    COUNT(*) AS common_films
FROM
    ActorPairs ap
GROUP BY
    ap.actor1_id,
    ap.actor2_id
ORDER BY
    ap.actor1_id,
    ap.actor2_id;
```

#Question No.14:-Implement a recursive CTE to find all employees in the staff table who report to a specific manager,
co?sidering the reports_to column.
To find all employees who report to a specific manager using a recursive Common Table Expression (CTE), you need to handle hierarchical data where employees report to other employees. 

Assuming the `staff` table has a `staff_id` and a `reports_to` column indicating the manager each employee reports to, you can use a recursive CTE to traverse this hierarchy and find all employees reporting directly or indirectly to a specific manager.

### Table Structure

- **`staff`**: Contains `staff_id`, `first_name`, `last_name`, and `reports_to` (which refers to `staff_id` of the manager).

### Query

Let’s say you want to find all employees who report to the manager with `staff_id = 5`. Here’s how you can do it:

```sql
-- Define the recursive CTE
WITH RECURSIVE EmployeeHierarchy AS (
    -- Anchor member: select the specified manager
    SELECT
        staff_id,
        first_name,
        last_name,
        reports_to
    FROM
        staff
    WHERE
        staff_id = 5  -- Specify the manager's ID here
    
    UNION ALL
    
    -- Recursive member: find employees reporting to the current level of employees
    SELECT
        s.staff_id,
        s.first_name,
        s.last_name,
        s.reports_to
    FROM
        staff s
    INNER JOIN
        EmployeeHierarchy eh ON s.reports_to = eh.staff_id
)

-- Select from the CTE to display all employees reporting to the specified manager
SELECT
    staff_id,
    first_name,
    last_name,
    reports_to
FROM
    EmployeeHierarchy
ORDER BY
    staff_id;
```
