# 📊 Sales & Financial Analytics Dashboard (SQL & Power BI)

## 📌 Executive Summary
Questo progetto dimostra un flusso di lavoro end-to-end per l'analisi delle performance di vendita e della redditività aziendale. 
Il processo parte dalla modellazione di un database relazionale e dall'estrazione dei dati tramite **SQL**, per poi passare alla modellazione dati, calcolo delle metriche aziendali in **DAX** e visualizzazione interattiva su **Power BI**.

---

## 🛠️ Tech Stack & Strumenti
* **SQL (MySQL):** DDL per creazione tabelle, DML per popolazione dati e query avanzate (`INNER JOIN`) per estrazione del dataset denormalizzato.
* **Power BI:** Data Modeling, Time Intelligence in DAX e reportistica interattiva.
* **GitHub:** Versionamento e documentazione dell'architettura dati.

---

## 🗄️ Database Architecture & SQL Extraction

### 1. Schema Relazionale (DDL)
Il database è composto da tre tabelle collegate tramite relazioni di chiave primaria (`PRIMARY KEY`) e chiave esterna (`FOREIGN KEY`):

```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    CustomerName VARCHAR(50),
    Region VARCHAR(50)
);

CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(50),
    Category VARCHAR(50)
);

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    ProductID INT,
    Sales DECIMAL(10,2),
    Profit DECIMAL(10,2),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID),
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID)
);
```

### 2. Popolamento Dati (DML)

```sql
INSERT INTO Customers (CustomerID, CustomerName, Region)
VALUES 
    (1, 'Marco Rossi', 'Nord'),
    (2, 'Elena Bianchi', 'Centro'),
    (3, 'Tech Solutions', 'Sud');

INSERT INTO Products (ProductID, ProductName, Category)
VALUES 
    (101, 'Sedia Ergonomica', 'Furniture'),
    (102, 'Monitor 4K', 'Technology');

INSERT INTO Orders (OrderID, CustomerID, ProductID, Sales, Profit)
VALUES 
    (1001, 1, 101, 250.00, 45.00),
    (1002, 2, 102, 400.00, 80.00),
    (1003, 3, 101, 500.00, 90.00),
    (1004, 1, 102, 420.00, 85.00);
```
### 3. Query di Estrazione Dati (Data Join)

```sql
SELECT 
    o.OrderID,
    c.CustomerName,
    c.Region,
    p.ProductName,
    p.Category,
    o.Sales,
    o.Profit
FROM Orders o
INNER JOIN Customers c ON o.CustomerID = c.CustomerID
INNER JOIN Products p ON o.ProductID = p.ProductID;
```
#### Risultato dell'Estrazione:

| OrderID | CustomerName | Region | ProductName | Category | Sales | Profit |
|---|---|---|---|---|---|---|
| 1001 | Marco Rossi | Nord | Sedia Ergonomica | Furniture | 250.00 | 45.00 |
| 1004 | Marco Rossi | Nord | Monitor 4K | Technology | 420.00 | 85.00 |
| 1002 | Elena Bianchi | Centro | Monitor 4K | Technology | 400.00 | 80.00 |
| 1003 | Tech Solutions | Sud | Sedia Ergonomica | Furniture | 500.00 | 90.00 |
