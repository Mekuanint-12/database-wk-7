Database Design and Normalization Assignment Explanation

This assignment demonstrates how to normalize a database step by step:

Question 1 → 1NF (First Normal Form)

Question 2 → 2NF (Second Normal Form)

 Question 1: Achieving 1NF
Problem with Original Table (ProductDetail)
OrderID	CustomerName	Products
101	John Doe	Laptop, Mouse
102	Jane Smith	Tablet, Keyboard, Mouse
103	Emily Clark	Phone

The Products column contains multiple values separated by commas.

This violates 1NF, which requires:

Atomic values (each column must hold only one value).

No repeating groups or arrays.

 Solution – Convert into 1NF

We create a new table ProductDetail_1NF where each product gets its own row.

CREATE TABLE ProductDetail_1NF (
    OrderID INT,
    CustomerName VARCHAR(100),
    Product VARCHAR(100)
);

INSERT INTO ProductDetail_1NF (OrderID, CustomerName, Product)
VALUES
(101, 'John Doe', 'Laptop'),
(101, 'John Doe', 'Mouse'),
(102, 'Jane Smith', 'Tablet'),
(102, 'Jane Smith', 'Keyboard'),
(102, 'Jane Smith', 'Mouse'),
(103, 'Emily Clark', 'Phone');

Result
OrderID	CustomerName	Product
101	John Doe	Laptop
101	John Doe	Mouse
102	Jane Smith	Tablet
102	Jane Smith	Keyboard
102	Jane Smith	Mouse
103	Emily Clark	Phone

Now:

One product per row.

All values are atomic.

Table is in 1NF.

Question 2: Achieving 2NF
Problem with Original Table (OrderDetails)
OrderID	CustomerName	Product	Quantity
101	John Doe	Laptop	2
101	John Doe	Mouse	1
102	Jane Smith	Tablet	3
102	Jane Smith	Keyboard	1
102	Jane Smith	Mouse	2
103	Emily Clark	Phone	1

Table is in 1NF, but not 2NF.

Issue: CustomerName depends only on OrderID (not on the whole composite key OrderID + Product).

This is a partial dependency, which violates 2NF.

Solution – Split into Two Tables

Orders Table

Keeps OrderID → CustomerName.

Each order has exactly one customer.

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerName VARCHAR(100)
);

INSERT INTO Orders (OrderID, CustomerName)
VALUES
(101, 'John Doe'),
(102, 'Jane Smith'),
(103, 'Emily Clark');


OrderDetails_2NF Table

Keeps OrderID + Product → Quantity.

Removes the partial dependency on just OrderID.

CREATE TABLE OrderDetails_2NF (
    OrderID INT,
    Product VARCHAR(100),
    Quantity INT,
    PRIMARY KEY (OrderID, Product),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
);

INSERT INTO OrderDetails_2NF (OrderID, Product, Quantity)
VALUES
(101, 'Laptop', 2),
(101, 'Mouse', 1),
(102, 'Tablet', 3),
(102, 'Keyboard', 1),
(102, 'Mouse', 2),
(103, 'Phone', 1);

 Resulting Tables

Orders

OrderID	CustomerName
101	John Doe
102	Jane Smith
103	Emily Clark

OrderDetails_2NF

OrderID	Product	Quantity
101	Laptop	2
101	Mouse	1
102	Tablet	3
102	Keyboard	1
102	Mouse	2
103	Phone	1
