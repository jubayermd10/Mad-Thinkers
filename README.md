# SuperShop Management System

<div align="center">

![Java](https://img.shields.io/badge/Java-JDK%2017+-orange?style=flat-square&logo=java)
![Swing](https://img.shields.io/badge/GUI-Java%20Swing-blue?style=flat-square)
![Storage](https://img.shields.io/badge/Storage-CSV%20Files-green?style=flat-square)
![Pattern](https://img.shields.io/badge/Pattern-MVC-purple?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)

A Java desktop application for retail shop management, built with Swing GUI and MVC architecture.

</div>

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [Architecture](#architecture)
- [OOP Concepts Applied](#oop-concepts-applied)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Screenshots](#screenshots)
- [Team](#team)

---

## Overview

SuperShop Management System is a Java desktop application developed as part of **CSE 202: Object Oriented Programming I Lab** at the University of Asia Pacific. It simulates a real-world retail shop with role-based access for Admins and Employees, full inventory control, billing with a shopping cart, and CSV-based data persistence.

---

## Features

### Admin
- Secure role-based login
- Add, update, and delete employee records
- View daily sales revenue report
- Monitor inventory stock status
- Restock low-stock items

### Employee
- Secure role-based login
- Add, update, search, and delete inventory items
- Shopping cart with dynamic pricing (standard & discounted items)
- Generate customer invoices (bills)
- View itemised invoice history

### System
- CSV file persistence — data survives application restarts
- Auto-creates default Admin account on first run
- Bill counter continuity across sessions
- Exception handling for all file I/O and user input

---

## Project Structure

```
SuperShop/
├── Main.java
├── controllers/
│   ├── AuthController.java
│   ├── BillingController.java
│   ├── InventoryController.java
│   └── UserController.java
├── dao/
│   └── CSVHandler.java
├── models/
│   ├── User.java          (abstract)
│   ├── Admin.java
│   ├── Employee.java
│   ├── Item.java          (abstract)
│   ├── StandardItem.java
│   ├── DiscountItem.java
│   └── Bill.java
├── view/
│   ├── LoginFrame.java
│   ├── AdminDashboard.java
│   ├── EmployeeDashboard.java
│   ├── AddEmployeeDialog.java
│   ├── UpdateEmployeeDialog.java
│   ├── AddItemDialog.java
│   ├── UpdateItemDialog.java
│   ├── SearchItemDialog.java
│   ├── BillingDialog.java
│   └── SalesReportDialog.java
└── data/
    ├── employees.csv
    ├── items.csv
    └── invoices.csv
```

---

## Architecture

This project follows the **MVC (Model-View-Controller)** pattern with a clean persistence layer:

```
┌─────────────────┐     ┌──────────────────────┐     ┌─────────────────┐
│   View Layer    │────▶│  Controller Layer     │────▶│  Model Layer    │
│  (Swing UI)     │     │  (Business Logic)     │     │  (Data Entities)│
│                 │     │                       │     │                 │
│ LoginFrame      │     │ AuthController        │     │ User (abstract) │
│ AdminDashboard  │     │ UserController        │     │ ├─ Admin        │
│ EmployeeDashbrd │     │ InventoryController   │     │ └─ Employee     │
│ BillingDialog   │     │ BillingController     │     │ Item (abstract) │
│ SalesReport     │     │                       │     │ ├─ StandardItem │
└─────────────────┘     └──────────────────────┘     │ └─ DiscountItem │
                                  │                   │ Bill            │
                                  ▼                   └─────────────────┘
                    ┌─────────────────────────┐
                    │   DAO / Persistence      │
                    │   CSVHandler             │
                    │  employees.csv           │
                    │  items.csv               │
                    │  invoices.csv            │
                    └─────────────────────────┘
```

**Key rule:** Views never talk to Models directly. All data flows through Controllers.

---

## OOP Concepts Applied

| Concept | How it's used |
|---|---|
| **Abstraction** | `User` and `Item` are abstract classes defining contracts without implementation |
| **Inheritance** | `Admin`, `Employee` extend `User`; `StandardItem`, `DiscountItem` extend `Item` |
| **Encapsulation** | All fields are `private`/`protected`, accessed only via getters/setters |
| **Polymorphism** | `calculatePrice()` is overridden — `StandardItem` returns base price, `DiscountItem` applies discount |

### Inheritance Hierarchies

```
User (abstract)              Item (abstract)
├── Admin                    ├── StandardItem
└── Employee                 └── DiscountItem
     └── shift, phone              └── discountPercent
```

### Exception Handling

| Exception | Source | Strategy |
|---|---|---|
| `NumberFormatException` | User input parsing | Try-catch with retry |
| `FileNotFoundException` | Missing CSV on startup | Auto-create default files |
| `IOException` | File read/write | Error log + user notification |
| `NullPointerException` | Null references | Validation before access |

---

## Getting Started

### Prerequisites

- Java JDK 17 or higher
- Any Java IDE (IntelliJ IDEA, Eclipse, NetBeans) or command line

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/jubayermd10/Mad-Thinkers.git
   cd Mad-Thinkers
   ```

2. Open in your IDE and build the project, **or** compile from the command line:
   ```bash
   javac -d out src/**/*.java
   ```

3. Run the application:
   ```bash
   java -cp out Main
   ```

### Default Credentials

On first run, a default admin account is created automatically:

| Role | ID | Password |
|---|---|---|
| Admin | `ADMIN001` | `admin123` |

> You can add employee accounts from the Admin dashboard after logging in.

---

## Usage

### Admin workflow

```
Login (Admin) → Admin Dashboard
    ├── Add Employee      — create new staff accounts
    ├── Update Employee   — edit name, password, shift, phone
    ├── Delete Employee   — remove staff with confirmation dialog
    ├── Sales Report      — view today's transactions and net revenue
    ├── Stock Status      — see all items with in-stock / low-stock / out-of-stock flags
    └── Restock Item      — add quantity to any item
```

### Employee workflow

```
Login (Employee) → Employee Dashboard
    ├── Add Item          — add Standard or Discount item to inventory
    ├── Search Item       — search by ID or name
    ├── Update Item       — edit name, price, stock, discount
    ├── Delete Item       — remove item with confirmation
    └── Generate Bill     — shopping cart → add items → generate invoice
```

### CSV data format

`employees.csv`
```
# id,name,password,role,shift,phone
ADMIN001,Super Admin,admin123,Admin,N/A,N/A
EMP001,Md. Zubaer,pass123,Employee,Day,01540173018
```

`items.csv`
```
# id,name,basePrice,discountPercent,stock,type
1,Apple,200.0,0.0,100,Standard
2,Mango,150.0,10.0,50,Discount
```

`invoices.csv`
```
BILL-00001,EMP001,2026-06-21 14:32:10,459.00,2
```

---

## Screenshots

| Login | Admin Dashboard |
|---|---|
| Role selector (Admin / Employee), ID & password fields | Employee list with Add / Update / Delete / Sales Report / Stock Status / Restock |

| Employee Dashboard | Generate Bill |
|---|---|
| Inventory table with Base Price, Disc.%, Final Price, Stock, Type | Shopping cart with item ID input, quantity, grand total, and invoice generation |

---

## Team

**Mad Thinkers** — University of Asia Pacific, CSE Section B2

| Name | ID | Contribution |
|---|---|---|
| Md. Zubaer | 24101106 | Authentication & core base — abstract `User` classes, login logic, `LoginFrame` |
| Nasim Mahmud | 24201104 | Admin panel — employee management GUI, CRUD operations |
| Dipraj Mitra Chandon | 24201091 | Inventory system — `Item` hierarchy, polymorphism, item CRUD interface |
| Yeasen Ahmed | 24201089 | Billing system — shopping cart, dynamic pricing, invoice generation |

**Submitted to:** Nahida Marzan, Lecturer, Dept. of CSE, University of Asia Pacific  
**Submission date:** 18-05-2026

---

## License

This project was developed for academic purposes under **CSE 202: Object Oriented Programming I Lab**.  
Feel free to reference or build upon it with attribution.

---

<div align="center">
© 2026 Mad Thinkers — University of Asia Pacific
</div>
