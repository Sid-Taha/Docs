# 🍃 MongoDB Aggregation Framework Tutorial

---

## 📘 Introduction to Aggregation

The aggregation framework processes data records and returns computed results.  
Think of it as a **pipeline** where documents pass through multiple stages,  
each stage transforming the documents in some way.

---

## 🧩 Aggregation Pipeline Stages (Most Common)

1. **$match** – Filter documents (like `WHERE` in SQL)
2. **$group** – Group documents and perform operations
3. **$project** – Select / transform fields (like `SELECT`)
4. **$sort** – Sort documents
5. **$limit** – Limit number of documents
6. **$skip** – Skip documents
7. **$lookup** – Join with another collection
8. **$unwind** – Deconstruct array fields
9. **$addFields** – Add new fields
10. **$count** – Count documents

---

## 🧮 Common Aggregation Operators

`$sum`, `$avg`, `$min`, `$max`, `$first`, `$last`, `$push`, `$addToSet`

---

## 🛒 Mock Data – E-Commerce Database

### 👤 Collection: `customers`

```js
db.customers.insertMany([
  { _id: 1, name: "Alice Johnson", email: "alice@email.com", age: 28, city: "New York", country: "USA", registeredDate: new Date("2023-01-15") },
  { _id: 2, name: "Bob Smith", email: "bob@email.com", age: 35, city: "London", country: "UK", registeredDate: new Date("2023-02-20") },
  { _id: 3, name: "Charlie Brown", email: "charlie@email.com", age: 42, city: "Toronto", country: "Canada", registeredDate: new Date("2023-03-10") },
  { _id: 4, name: "Diana Prince", email: "diana@email.com", age: 31, city: "New York", country: "USA", registeredDate: new Date("2023-01-25") },
  { _id: 5, name: "Eve Wilson", email: "eve@email.com", age: 26, city: "Sydney", country: "Australia", registeredDate: new Date("2023-04-05") },
  { _id: 6, name: "Frank Miller", email: "frank@email.com", age: 38, city: "London", country: "UK", registeredDate: new Date("2023-02-14") },
  { _id: 7, name: "Grace Lee", email: "grace@email.com", age: 29, city: "Singapore", country: "Singapore", registeredDate: new Date("2023-05-12") },
  { _id: 8, name: "Henry Davis", email: "henry@email.com", age: 45, city: "New York", country: "USA", registeredDate: new Date("2023-01-08") },
  { _id: 9, name: "Iris Chen", email: "iris@email.com", age: 33, city: "Tokyo", country: "Japan", registeredDate: new Date("2023-06-20") },
  { _id: 10, name: "Jack Turner", email: "jack@email.com", age: 27, city: "Berlin", country: "Germany", registeredDate: new Date("2023-03-30") }
]);
````

---

### 📦 Collection: `products`

```js
db.products.insertMany([
  { _id: 101, name: "Laptop Pro", category: "Electronics", brand: "TechCorp", price: 1200, stock: 50 },
  { _id: 102, name: "Wireless Mouse", category: "Electronics", brand: "TechCorp", price: 25, stock: 200 },
  { _id: 103, name: "Desk Chair", category: "Furniture", brand: "ComfortPlus", price: 350, stock: 30 },
  { _id: 104, name: "Coffee Maker", category: "Appliances", brand: "BrewMaster", price: 80, stock: 100 },
  { _id: 105, name: "Running Shoes", category: "Sports", brand: "SpeedFit", price: 120, stock: 75 },
  { _id: 106, name: "Yoga Mat", category: "Sports", brand: "FlexZone", price: 35, stock: 150 },
  { _id: 107, name: "Backpack", category: "Accessories", brand: "TravelGear", price: 60, stock: 90 },
  { _id: 108, name: "Smart Watch", category: "Electronics", brand: "TechCorp", price: 300, stock: 60 },
  { _id: 109, name: "Blender", category: "Appliances", brand: "BrewMaster", price: 50, stock: 120 },
  { _id: 110, name: "Office Desk", category: "Furniture", brand: "ComfortPlus", price: 450, stock: 20 }
]);
```

---

### 🧾 Collection: `orders`

```js
db.orders.insertMany([
  { _id: 1001, customerId: 1, orderDate: new Date("2024-01-15"), status: "delivered", totalAmount: 1200, items: [{ productId: 101, quantity: 1, price: 1200 }] },
  { _id: 1002, customerId: 2, orderDate: new Date("2024-01-16"), status: "delivered", totalAmount: 105, items: [{ productId: 102, quantity: 2, price: 25 }, { productId: 104, quantity: 1, price: 80 }] },
  { _id: 1003, customerId: 1, orderDate: new Date("2024-02-10"), status: "shipped", totalAmount: 385, items: [{ productId: 103, quantity: 1, price: 350 }, { productId: 106, quantity: 1, price: 35 }] },
  { _id: 1004, customerId: 3, orderDate: new Date("2024-02-12"), status: "delivered", totalAmount: 120, items: [{ productId: 105, quantity: 1, price: 120 }] },
  { _id: 1005, customerId: 4, orderDate: new Date("2024-02-20"), status: "delivered", totalAmount: 360, items: [{ productId: 108, quantity: 1, price: 300 }, { productId: 107, quantity: 1, price: 60 }] },
  { _id: 1006, customerId: 2, orderDate: new Date("2024-03-05"), status: "processing", totalAmount: 450, items: [{ productId: 110, quantity: 1, price: 450 }] },
  { _id: 1007, customerId: 5, orderDate: new Date("2024-03-08"), status: "delivered", totalAmount: 410, items: [{ productId: 103, quantity: 1, price: 350 }, { productId: 107, quantity: 1, price: 60 }] },
  { _id: 1008, customerId: 6, orderDate: new Date("2024-03-15"), status: "delivered", totalAmount: 1250, items: [{ productId: 101, quantity: 1, price: 1200 }, { productId: 109, quantity: 1, price: 50 }] },
  { _id: 1009, customerId: 7, orderDate: new Date("2024-04-01"), status: "cancelled", totalAmount: 300, items: [{ productId: 108, quantity: 1, price: 300 }] },
  { _id: 1010, customerId: 1, orderDate: new Date("2024-04-10"), status: "delivered", totalAmount: 180, items: [{ productId: 105, quantity: 1, price: 120 }, { productId: 107, quantity: 1, price: 60 }] }
]);
```

---

### ⭐ Collection: `reviews`

```js
db.reviews.insertMany([
  { _id: 2001, productId: 101, customerId: 1, rating: 5, comment: "Excellent laptop!", reviewDate: new Date("2024-01-20") },
  { _id: 2002, productId: 102, customerId: 2, rating: 4, comment: "Good mouse, works well", reviewDate: new Date("2024-01-22") },
  { _id: 2003, productId: 104, customerId: 2, rating: 5, comment: "Makes great coffee", reviewDate: new Date("2024-01-25") }
]);
```

---

## 🧠 PRACTICE SCENARIOS

*(As requested — unchanged)*

```text
// SCENARIO 1: BASIC FILTERING AND COUNTING
// Find how many customers are from the USA
// Expected result: Count of USA customers

// SCENARIO 2: GROUPING AND AGGREGATION
// Calculate the total number of customers per country
// Expected result: Country name and count of customers

// SCENARIO 3: MATCHING AND PROJECTING
// Get all orders with status "delivered" and show only orderId, customerId, and totalAmount
// Expected result: Filtered and projected order documents

// SCENARIO 4: SORTING AND LIMITING
// Find the top 5 most expensive products
// Expected result: 5 products sorted by price (highest first)

// SCENARIO 5: GROUPING WITH SUM
// Calculate total revenue (sum of totalAmount) for each order status
// Expected result: Status and total revenue for each status

// SCENARIO 6: AVERAGE CALCULATION
// Find the average rating for each product that has reviews
// Expected result: ProductId and average rating

// SCENARIO 7: UNWINDING ARRAYS
// Get all individual order items from the orders collection
// (Hint: Use $unwind on the items array)
// Expected result: Flattened documents with one item per document

// SCENARIO 8: LOOKUP (JOIN)
// Get all orders with customer details (join orders with customers)
// Expected result: Order documents with embedded customer information

// SCENARIO 9: COMPLEX GROUPING
// Find total quantity sold for each product
// (Hint: Unwind items array, then group by productId and sum quantity)
// Expected result: ProductId and total quantity sold

// SCENARIO 10: MULTIPLE STAGES
// Find customers from "New York" who have placed orders with status "delivered"
// Show customer name, email, and total amount spent
// Expected result: Aggregated customer data with spending

// SCENARIO 11: ADDING FIELDS
// Add a field to products showing if they are "expensive" (price > 200)
// Expected result: Products with new boolean field

// SCENARIO 12: COUNTING WITH FILTERS
// Count how many orders were placed each month in 2024
// Expected result: Month and count of orders

// SCENARIO 13: NESTED LOOKUP
// Get products with their average rating and total number of reviews
// Expected result: Products with rating statistics

// SCENARIO 14: TOP PERFORMERS
// Find the top 3 customers by total spending (sum of their order amounts)
// Expected result: Customer details with total spending

// SCENARIO 15: CONDITIONAL GROUPING
// Count orders by status, but only for orders above $200
// Expected result: Status and count of high-value orders

// SCENARIO 16: DATE OPERATIONS
// Find all orders placed in Q1 2024 (January-March)
// Expected result: Orders from first quarter

// SCENARIO 17: COMPLEX AGGREGATION
// Find products that have never been ordered
// (Hint: Use $lookup with orders.items.productId)
// Expected result: Products not in any order

// SCENARIO 18: MULTIPLE CALCULATIONS
// For each category, find: total products, average price, min price, max price
// Expected result: Category statistics

// SCENARIO 19: CUSTOMER INSIGHTS
// Find customers who have placed more than 2 orders
// Show their name, email, order count, and total spent
// Expected result: High-frequency customers

// SCENARIO 20: RATING ANALYSIS
// Find products with average rating >= 4.5 and at least 2 reviews
// Show product name, average rating, and review count
// Expected result: Highly-rated products with sufficient reviews
```

---

## ✅ SOLUTIONS

Each solution is written using MongoDB aggregation pipelines 👇
(Kept intact, just wrapped for Markdown readability)

```js
// SOLUTION 1: BASIC FILTERING AND COUNTING
db.customers.aggregate([
  { $match: { country: "USA" } },
  { $count: "totalUSACustomers" }
]);

// SOLUTION 2: GROUPING AND AGGREGATION
db.customers.aggregate([
  { $group: { _id: "$country", customerCount: { $sum: 1 } } },
  { $sort: { customerCount: -1 } }
]);

// SOLUTION 3: MATCHING AND PROJECTING
db.orders.aggregate([
  { $match: { status: "delivered" } },
  { $project: { _id: 1, customerId: 1, totalAmount: 1 } }
]);

// SOLUTION 4: SORTING AND LIMITING
db.products.aggregate([
  { $sort: { price: -1 } },
  { $limit: 5 }
]);

// SOLUTION 5: GROUPING WITH SUM
db.orders.aggregate([
  { $group: { _id: "$status", totalRevenue: { $sum: "$totalAmount" } } },
  { $sort: { totalRevenue: -1 } }
]);

// SOLUTION 6: AVERAGE CALCULATION
db.reviews.aggregate([
  { $group: { 
      _id: "$productId", 
      averageRating: { $avg: "$rating" },
      reviewCount: { $sum: 1 }
  }},
  { $sort: { averageRating: -1 } }
]);

// SOLUTION 7: UNWINDING ARRAYS
db.orders.aggregate([
  { $unwind: "$items" },
  { $project: { 
      orderId: "$_id", 
      customerId: 1, 
      productId: "$items.productId", 
      quantity: "$items.quantity", 
      price: "$items.price" 
  }}
]);

// SOLUTION 8: LOOKUP (JOIN)
db.orders.aggregate([
  { $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customerDetails"
  }},
  { $unwind: "$customerDetails" },
  { $project: {
      _id: 1,
      orderDate: 1,
      totalAmount: 1,
      "customerDetails.name": 1,
      "customerDetails.email": 1,
      "customerDetails.city": 1
  }}
]);

// SOLUTION 9: COMPLEX GROUPING
db.orders.aggregate([
  { $unwind: "$items" },
  { $group: {
      _id: "$items.productId",
      totalQuantitySold: { $sum: "$items.quantity" }
  }},
  { $sort: { totalQuantitySold: -1 } }
]);

// SOLUTION 10: MULTIPLE STAGES
db.customers.aggregate([
  { $match: { city: "New York" } },
  { $lookup: {
      from: "orders",
      localField: "_id",
      foreignField: "customerId",
      as: "orders"
  }},
  { $unwind: "$orders" },
  { $match: { "orders.status": "delivered" } },
  { $group: {
      _id: "$_id",
      name: { $first: "$name" },
      email: { $first: "$email" },
      totalSpent: { $sum: "$orders.totalAmount" }
  }}
]);

// SOLUTION 11: ADDING FIELDS
db.products.aggregate([
  { $addFields: { 
      isExpensive: { $gt: ["$price", 200] } 
  }}
]);

// SOLUTION 12: COUNTING WITH FILTERS
db.orders.aggregate([
  { $match: { 
      orderDate: { 
          $gte: new Date("2024-01-01"), 
          $lt: new Date("2025-01-01") 
      }
  }},
  { $group: {
      _id: { $month: "$orderDate" },
      orderCount: { $sum: 1 }
  }},
  { $sort: { _id: 1 } },
  { $project: {
      month: "$_id",
      orderCount: 1,
      _id: 0
  }}
]);

// SOLUTION 13: NESTED LOOKUP
db.products.aggregate([
  { $lookup: {
      from: "reviews",
      localField: "_id",
      foreignField: "productId",
      as: "reviews"
  }},
  { $project: {
      name: 1,
      price: 1,
      category: 1,
      averageRating: { $avg: "$reviews.rating" },
      reviewCount: { $size: "$reviews" }
  }},
  { $match: { reviewCount: { $gt: 0 } } }
]);

// SOLUTION 14: TOP PERFORMERS
db.orders.aggregate([
  { $group: {
      _id: "$customerId",
      totalSpent: { $sum: "$totalAmount" },
      orderCount: { $sum: 1 }
  }},
  { $sort: { totalSpent: -1 } },
  { $limit: 3 },
  { $lookup: {
      from: "customers",
      localField: "_id",
      foreignField: "_id",
      as: "customerInfo"
  }},
  { $unwind: "$customerInfo" },
  { $project: {
      _id: 0,
      customerId: "$_id",
      name: "$customerInfo.name",
      email: "$customerInfo.email",
      totalSpent: 1,
      orderCount: 1
  }}
]);

// SOLUTION 15: CONDITIONAL GROUPING
db.orders.aggregate([
  { $match: { totalAmount: { $gt: 200 } } },
  { $group: {
      _id: "$status",
      orderCount: { $sum: 1 },
      averageAmount: { $avg: "$totalAmount" }
  }}
]);

// SOLUTION 16: DATE OPERATIONS
db.orders.aggregate([
  { $match: {
      orderDate: {
          $gte: new Date("2024-01-01"),
          $lt: new Date("2024-04-01")
      }
  }},
  { $project: {
      _id: 1,
      customerId: 1,
      orderDate: 1,
      totalAmount: 1,
      status: 1
  }}
]);

// SOLUTION 17: COMPLEX AGGREGATION
db.products.aggregate([
  { $lookup: {
      from: "orders",
      let: { productId: "$_id" },
      pipeline: [
          { $unwind: "$items" },
          { $match: { $expr: { $eq: ["$items.productId", "$$productId"] } } }
      ],
      as: "orderItems"
  }},
  { $match: { orderItems: { $size: 0 } } },
  { $project: { name: 1, category: 1, price: 1 } }
]);

// SOLUTION 18: MULTIPLE CALCULATIONS
db.products.aggregate([
  { $group: {
      _id: "$category",
      totalProducts: { $sum: 1 },
      averagePrice: { $avg: "$price" },
      minPrice: { $min: "$price" },
      maxPrice: { $max: "$price" }
  }},
  { $sort: { averagePrice: -1 } }
]);

// SOLUTION 19: CUSTOMER INSIGHTS
db.orders.aggregate([
  { $group: {
      _id: "$customerId",
      orderCount: { $sum: 1 },
      totalSpent: { $sum: "$totalAmount" }
  }},
  { $match: { orderCount: { $gt: 2 } } },
  { $lookup: {
      from: "customers",
      localField: "_id",
      foreignField: "_id",
      as: "customerInfo"
  }},
  { $unwind: "$customerInfo" },
  { $project: {
      _id: 0,
      name: "$customerInfo.name",
      email: "$customerInfo.email",
      orderCount: 1,
      totalSpent: 1
  }},
  { $sort: { totalSpent: -1 } }
]);

// SOLUTION 20: RATING ANALYSIS
db.reviews.aggregate([
  { $group: {
      _id: "$productId",
      averageRating: { $avg: "$rating" },
      reviewCount: { $sum: 1 }
  }},
  { $match: { 
      averageRating: { $gte: 4.5 },
      reviewCount: { $gte: 2 }
  }},
  { $lookup: {
      from: "products",
      localField: "_id",
      foreignField: "_id",
      as: "productInfo"
  }},
  { $unwind: "$productInfo" },
  { $project: {
      _id: 0,
      productId: "$_id",
      productName: "$productInfo.name",
      category: "$productInfo.category",
      averageRating: { $round: ["$averageRating", 2] },
      reviewCount: 1
  }},
  { $sort: { averageRating: -1 } }
]);
```
