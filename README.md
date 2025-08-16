# 📚 SQL Project Documentation

This repository contains **SQL queries** for managing and analyzing data in a sample database `dab15`.  
The project demonstrates **basic to advanced SQL operations** on three main tables:

- **books**
- **customers**
- **orders**

---

## 📂 Project Structure

```
sql_project/
│── queries.sql        # All SQL queries
│── README.md          # Project documentation
```

---

## 🗂️ Database Initialization

```sql
create database dab15;
use dab15;
```

---

## 🔹 Basic SQL Queries

### 1. Retrieve all books in the Fiction Genre
```sql
select * from books 
where Genre="Fiction";
```

### 2. Find books published after the year 1950
```sql
select * from books 
where (Published_Year) > 1950;
```

### 3. List all customers from Canada
```sql
select * from customers 
where Country='Canada';
```

### 4. Show orders placed in November 2023
```sql
select * from orders 
where Order_Date > '2023-11-01' and Order_Date < '2023-11-30';
```

### 5. Retrieve the total stocks of books available
```sql
select sum(Stock) as total_stock 
from books;
```

### 6. Find the details of the most expensive book
```sql
select max(Price) from books;

select * from books 
where Price=(select max(Price) from books);

select * from books 
order by Price desc 
limit 1;
```

### 7. Show customers who ordered more than 1 quantity of a book
```sql
select * from orders 
where Quantity > 1;
```

### 8. Retrieve all orders where total amount exceeds $20
```sql
select * from orders 
where Total_Amount > 20;
```

### 9. List all genres available in the books table
```sql
select distinct(Genre) 
from books;
```

### 10. Find the book with the lowest stock
```sql
select * from books 
order by Stock 
limit 1;
```

### 11. Calculate the total revenue generated from all orders
```sql
select sum(Total_Amount) as revenue 
from orders;
```

---

## 🔹 Advanced SQL Queries

### 1. Retrieve the total number of books sold for each genre
```sql
select b.Genre, sum(o.Quantity) as Total_Books_Sold
from books as b 
join orders as o on b.Book_ID = o.Book_ID
group by b.Genre;
```

### 2. Find the average price of books in the "Fantasy" Genre
```sql
select avg(Price) as average_price 
from books
where Genre="Fantasy";
```

### 3. List customers who have placed at least 2 orders
```sql
select c.Customer_ID, c.Name, count(o.Order_ID) as total_order
from customers as c 
join orders as o on c.Customer_ID = o.Customer_ID
group by c.Customer_ID, c.Name
having total_order >= 2;
```

### 4. Find the most frequently ordered book
```sql
select Book_ID, count(Order_ID) as order_count
from orders
group by Book_ID
order by order_count desc;
```

### 5. Show the top 3 most expensive books of the "Fantasy" Genre
```sql
select Book_ID, Title, Genre, Price
from books
where Genre="Fantasy"
order by Price desc 
limit 3;
```

### 6. Retrieve the total quantity of books sold by each author
```sql
select b.Author, sum(o.Quantity) as total_quantity
from books as b 
join orders as o on b.Book_ID = o.Book_ID
group by b.Author;
```

### 7. List the cities where customers who spent over $30 are located
```sql
select c.City, o.Total_Amount
from orders as o 
join customers as c on c.Customer_ID = o.Customer_ID
where o.Total_Amount > 30;
```

### 8. Find the customer who spent the most on orders
```sql
select c.Customer_ID, c.Name, sum(o.Total_Amount) as total_spent
from customers as c 
join orders as o on c.Customer_ID = o.Customer_ID
group by c.Customer_ID, c.Name
order by total_spent desc 
limit 1;
```

### 9. Calculate the stock remaining after fulfilling all orders
```sql
select b.Book_ID, b.Title, b.Stock, coalesce(sum(o.Quantity),0) as total_quantity, 
       (b.Stock - coalesce(sum(o.Quantity),0)) as remaining_quantity
from books as b 
left join orders as o on b.Book_ID = o.Book_ID
group by b.Book_ID, b.Title, b.Stock;
```

---

## 🚀 How to Use

1. Clone the repository  
   ```bash
   git clone https://github.com/your-username/sql_project.git
   ```
2. Import the database schema into your SQL server.  
3. Run queries from **queries.sql** or directly from the README.

---

## 🤝 Contribution

- Fork the repo  
- Create a feature branch (`git checkout -b feature-name`)  
- Commit your changes  
- Submit a pull request  

---

✅ This project is designed for **SQL practice** and can be extended for real-world database management.
