# answers.sequel
Question 1 

`sql
-- Creating a new table to store the transformed data
CREATE TABLE ProductDetail_1NF (
    OrderID INT,
    CustomerName VARCHAR(100),
    Product VARCHAR(100)
);

-- Inserting data into the new table with one product per row
INSERT INTO ProductDetail_1NF (OrderID, CustomerName, Product)
SELECT OrderID, CustomerName, TRIM(SUBSTRING_INDEX(SUBSTRING_INDEX(Products, ',', n.n), ',', -1)) AS Product
FROM ProductDetail
JOIN (SELECT 1 AS n UNION ALL SELECT 2 UNION ALL SELECT 3 UNION ALL SELECT 4 UNION ALL SELECT 5) n
WHERE CHAR_LENGTH(Products)
-CHAR_LENGTH(REPLACE(Products, ',', '')) >= n.n - 1
ORDER BY OrderID, n.n;

Question 2 


1. *Orders Table* (storing *OrderID* and *CustomerName*):

   sql
   CREATE TABLE Orders (
       OrderID INT PRIMARY KEY,
       CustomerName VARCHAR(100)
   );
   

   Sample data for the `Orders` table:

   
   OrderID | CustomerName
   -------------------------
   101     | John Doe
   102     | Jane Smith
   103     | Emily Clark
   

2. *OrderDetails Table* (storing *OrderID*, *Product*, and *Quantity*):
3. sql
   CREATE TABLE OrderDetails (
       OrderID INT,
       Product VARCHAR(100),
       Quantity INT,
       PRIMARY KEY (OrderID, Product),
       FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
   );
   

   Sample data for the `OrderDetails` table:

   
   OrderID | Product  | Quantity
   -------------------------------
   101     | Laptop  | 2
   101     | Mouse   | 1
   102     | Tablet  | 3
   102     | Keyboard| 1
   102     | Mouse   | 2
   103     | Phone   | 1
   

SQL Query to Achieve 2NF:

sql
-- Create the Orders table
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerName VARCHAR(100)
);

-- Insert data into the Orders table
INSERT INTO Orders (OrderID, CustomerName)
VALUES
(101, 'John Doe'),
(102, 'Jane Smith'),
(103, 'Emily Clark');

-- Create the OrderDetails table
CREATE TABLE OrderDetails (
    OrderID INT,
    Product VARCHAR(100),
    Quantity INT,
    PRIMARY KEY (OrderID, Product),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
);

-- Insert data into the OrderDetails table
INSERT INTO OrderDetails (OrderID, Product, Quantity)
VALUES
(101, 'Laptop', 2),
(101, 'Mouse', 1),
(102, 'Tablet', 3),
(102, 'Keyboard', 1),
(102, 'Mouse', 2),
(103, 'Phone', 1);
```
