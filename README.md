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
