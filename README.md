# Library Management System

This project is a **Library Management System** built using **SQL** that facilitates the efficient management of library operations. It includes features for tracking books, customers, employees, book issuance, returns, and branch management. The system is designed to streamline library operations and provide insights through SQL queries and data analysis.

## Database Setup

To get started, create a new database called `Library` to store all the required information:

```sql
CREATE DATABASE library;
USE library;
```

## Tables and Features

### 1. **Book Management**
   - Add, update, and remove books from the library's collection.
   - Track details such as **title**, **category**, **rental price**, **availability status**, **author**, and **publisher**.

### 2. **Customer Management**
   - Maintain records for each customer including **name**, **address**, **registration date**, and **issuance history**.

### 3. **Employee Management**
   - Manage employees, including **name**, **position**, **salary**, and **branch assignments**.

### 4. **Book Issuance and Returns**
   - Track issued books, customers who have borrowed them, and ensure timely returns.
   - Maintain a record of **issue date**, **return date**, and associated **book titles**.

### 5. **Branch Management**
   - Track library branches, including **branch numbers**, **manager assignments**, **addresses**, and **contact details**.

## SQL Schema

Below is the SQL code for creating the necessary tables and relationships:

### `Branch Table`
```sql
CREATE TABLE Branch (
  Branch_no VARCHAR(10) PRIMARY KEY,
  Manager_id VARCHAR(10),
  Branch_address VARCHAR(30),
  Contact_no VARCHAR(15)
);
```

### `Employee Table`
```sql
CREATE TABLE Employee (
  Emp_id VARCHAR(10) PRIMARY KEY,
  Emp_name VARCHAR(30),
  Position VARCHAR(30),
  Salary DECIMAL(10,2)
);
```

### `Customer Table`
```sql
CREATE TABLE Customer (
  Customer_Id VARCHAR(10) PRIMARY KEY,
  Customer_name VARCHAR(30),
  Customer_address VARCHAR(30),
  Reg_date DATE
);
```

### `Books Table`
```sql
CREATE TABLE Books (
  ISBN VARCHAR(10) PRIMARY KEY,
  Book_title VARCHAR(50),
  Category VARCHAR(30),
  Rental_Price DECIMAL(10,2),
  Status ENUM('Yes', 'No'),
  Author VARCHAR(30),
  Publisher VARCHAR(30)
);
```

### `IssueStatus Table`
```sql
CREATE TABLE IssueStatus (
  Issue_Id VARCHAR(10) PRIMARY KEY,
  Issued_cust VARCHAR(30),
  Issued_book_name VARCHAR(50),
  Issue_date DATE,
  Isbn_book VARCHAR(15),
  FOREIGN KEY (Issued_cust) REFERENCES Customer(Customer_Id) ON DELETE CASCADE,
  FOREIGN KEY (Isbn_book) REFERENCES Books(ISBN) ON DELETE CASCADE
);
```

### `ReturnStatus Table`
```sql
CREATE TABLE ReturnStatus (
  Return_id VARCHAR(10) PRIMARY KEY,
  Return_cust VARCHAR(30),
  Return_book_name VARCHAR(50),
  Return_date DATE,
  isbn_book2 VARCHAR(15),
  FOREIGN KEY (isbn_book2) REFERENCES Books(ISBN) ON DELETE CASCADE
);
```

## Data Insertions

To insert sample data into these tables:

```sql
-- Insert data for Branch, Employee, Customer, Books, IssueStatus, and ReturnStatus tables
INSERT INTO Branch VALUES ('B001', 'M101', '123 Main St', '+919099988676');
-- Similarly, insert for other tables (Employee, Customer, etc.)
```

## Key Queries for Data Analysis

### 1. Retrieve available books' titles, categories, and rental prices:
```sql
SELECT Book_title, Category, Rental_Price FROM Books WHERE Status = 'Yes';
```

### 2. List employees and their salaries in descending order:
```sql
SELECT Emp_name, Salary FROM Employee ORDER BY Salary DESC;
```

### 3. Retrieve books issued to customers:
```sql
SELECT IssueStatus.Issued_book_name, Customer.Customer_name 
FROM IssueStatus 
INNER JOIN Customer ON IssueStatus.Issued_cust = Customer.Customer_Id;
```

### 4. Display the total count of books in each category:
```sql
SELECT Category, COUNT(Book_title) FROM Books GROUP BY Category;
```

### 5. Employees with salaries above Rs. 50,000:
```sql
SELECT Emp_name, Position FROM Employee WHERE Salary > 50000;
```

### 6. Customers who registered before 2022-01-01 and haven’t issued any books:
```sql
SELECT Customer_name FROM Customer 
WHERE Reg_date < '2022-01-01' AND Customer_Id NOT IN (SELECT Issued_cust FROM IssueStatus);
```

### 7. Count employees per branch:
```sql
SELECT Branch_no, COUNT(Emp_id) FROM Employee GROUP BY Branch_no;
```

### 8. Customers who issued books in June 2023:
```sql
SELECT Customer.Customer_name 
FROM Customer 
INNER JOIN IssueStatus ON Customer.Customer_Id = IssueStatus.Issued_cust 
WHERE IssueStatus.Issue_date BETWEEN '2023-06-01' AND '2023-06-30';
```

### 9. Retrieve all book titles in the "History" category:
```sql
SELECT Book_title FROM Books WHERE Category = 'History';
```

### 10. Branches with more than 5 employees:
```sql
SELECT Branch_no, COUNT(Emp_id) FROM Employee 
GROUP BY Branch_no 
HAVING COUNT(Emp_id) > 5;
```

---

## Usage

1. Clone the repository or download the project files.
2. Set up your database using the provided SQL schema and queries.
3. Use the system to manage books, employees, customers, and more.
