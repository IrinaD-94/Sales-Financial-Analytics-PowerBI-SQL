# 📊 Sales & Financial Analytics Dashboard (SQL & Power BI)

## 📌 Executive Summary
Questo progetto dimostra un flusso di lavoro end-to-end per l'analisi delle performance di vendita e della redditività aziendale. 
Il processo parte dalla modellazione di un database relazionale e dall'estrazione dei dati tramite **SQL**, per poi passare alla modellazione dati, al calcolo delle metriche aziendali in **DAX** e alla visualizzazione interattiva su **Power BI**.

---

## 📸 Dashboard Preview

![Sales & Profitability Dashboard](Sales_Profitability_Dashboard)

---

## 🛠️ Tech Stack & Strumenti
* **SQL (MySQL):** DDL per creazione tabelle, DML per popolazione dati e query avanzate (`INNER JOIN`) per estrazione del dataset denormalizzato.
* **Power BI & Power Query:** Data Transformation, modellazione dati e reportistica interattiva con visual design aziendale.
* **DAX (Data Analysis Expressions):** Calcolo di KPI dinamici (`Total Sales`, `Total Profit`, `Profit Margin %`) e formattazione custom.
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
    (1, 'Marco Rossi', 'North'),
    (2, 'Elena Bianchi', 'Center'),
    (3, 'Tech Solutions', 'South');

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
| 1001 | Marco Rossi | North | Sedia Ergonomica | Furniture | 250.00 | 45.00 |
| 1004 | Marco Rossi | North | Monitor 4K | Technology | 420.00 | 85.00 |
| 1002 | Elena Bianchi | Center | Monitor 4K | Technology | 400.00 | 80.00 |
| 1003 | Tech Solutions | South | Sedia Ergonomica | Furniture | 500.00 | 90.00 |


---

## 📐 Business Logic & Formule DAX

Per l'analisi dei dati all'interno di Power BI sono state create misure DAX dedicate per garantire il calcolo corretto dei KPI e una formattazione chiara:

```dax
-- Totale Vendite
Total Sales = 
FORMAT(
    SUM(sales_extracted_data[Sales]) / 100, 
    "#,##0.00"
) & " €"

-- Totale Profitto
Total Profit = 
FORMAT(
    SUM(sales_extracted_data[Profit]), 
    "#,##0.00"
) & " €"
```

---

## 💡 Key Business Insights

* **Performance per Categoria:** La categoria **Technology** guida le vendite complessive con un totale di **820 €**, seguita da **Furniture** con **750 €**.

* **Distribuzione Geografica:** La regione **North** rappresenta il mercato principale con **670 €** di vendite, seguita da **South** (500 €) e **Center** (400 €).

* **Redditività Aziendale:** Il margine di profitto medio si attesta su un ottimo **19,11%**, a fronte di un profitto totale di **300 €** su **1.570 €** di fatturato complessivo.
