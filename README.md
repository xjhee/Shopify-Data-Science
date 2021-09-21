# Shopify-Data-Science

## Question1
1. Think about what could be going wrong with our calculation. Think about a better way to evaluate this data。
Ans: The incorrect AOV is because total_items are calculated with count(), we should instead calculate them with sum() 

2. What metric would you report for this dataset?
Ans: There are several metrics I think we could consider
- sum of order amount, which can be derived by 
 ```
 df = pd.read_csv('...')
 sum_order_amount = df['order_amount'].sum()
 ```
- sum of total items, which can be derived by 
 ```
 df = pd.read_csv('...')
 sum_total_items = df['total_items'].sum()
 ```
 
3. What is its value?
Ans: AOV can be calculated by deviding sum_order_amount by sum_total_items, which gives us 357.92
```
aov = round(sum_order_amount / sum_total_items, 2)
```


## Question2
1. How many orders were shipped by Speedy Express in total?
Ans: 54
```
SELECT 
COUNT(ShipperID)
FROM Orders
WHERE ShipperID == 1;
```


2. What is the last name of the employee with the most orders?
Ans: Peacock
```
SELECT e.LastName
FROM Employees AS e
WHERE (SELECT o.EmployeeID, COUNT(o.EmployeeID)
FROM Orders AS o
GROUP BY o.EmployeeID
ORDER BY COUNT(o.EmployeeID) DESC
LIMIT 1;
```


3. What product was ordered the most by customers in Germany?
Ans: Camembert Pierrot
```
SELECT p.ProductName, SUM(Quantity) AS sumQuantity
FROM Orders AS o, OrderDetails AS od, Customers AS c, Products AS p
WHERE c.Country = "Germany" AND od.OrderID = o.OrderID 
AND od.ProductID = p.ProductID AND c.CustomerID = o.CustomerID
GROUP BY p.ProductID
ORDER BY sumQuantity DESC
LIMIT 1;
```
