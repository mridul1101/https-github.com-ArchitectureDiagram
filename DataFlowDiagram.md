### Data flow Diagram -Slide Number 13 PPT

```
                              USER QUERY
                                  │
                                  ▼
                    "What products did John buy?"
                                  │
                                  ▼
                    ┌─────────────────────────┐
                    │       GEMINI AI         │
                    │    (NL → SQL with JOINs)│
                    └────────────┬────────────┘
                                 │
                                 ▼
     ┌───────────────────────────────────────────────────────┐
     │  SELECT p.product_name, s.quantity, s.total_amount    │
     │  FROM sales s                                         │
     │  JOIN customers c ON s.customer_id = c.customer_id    │
     │  JOIN products p ON s.product_id = p.product_id       │
     │  WHERE c.name LIKE '%John%'                           │
     └───────────────────────────┬───────────────────────────┘
                                 │
                                 ▼
     ┌─────────────────────────────────────────────────────────────┐
     │                     SQLite Database                         │
     │                                                             │
     │   ┌───────────┐      ┌───────────┐      ┌───────────┐     │
     │   │ Customers │──FK──│   Sales   │──FK──│ Products  │     │
     │   │           │      │           │      │           │     │
     │   │ id: 1     │      │ cust_id:1 │      │ id: 101   │     │
     │   │ John Smith│      │ prod_id:101      │ Laptop    │     │
     │   └───────────┘      └───────────┘      └───────────┘     │
     │                                                             │
     └─────────────────────────────────────────────────────────────┘
                                 │
                                 ▼
                    ┌─────────────────────────┐
                    │     JOINED RESULTS      │
                    │  product_name, qty, amt │
                    └─────────────────────────┘
```
