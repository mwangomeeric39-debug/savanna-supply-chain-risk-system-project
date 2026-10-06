# Savanna Supply Chain Risk Detection & Decision-Support System

> **From operational data to prioritized risk, ownership, and action.**

Savanna Distribution Ltd is a fictional East African distributor managing products across warehouses, suppliers, and customers.

The company has plenty of operational data — orders, inventory, purchase orders, suppliers, customers, and products — but the real challenge is turning that data into **timely decisions**.

This project builds an end-to-end **Supply Chain Risk Detection & Decision-Support System** that identifies emerging supply-chain risks, estimates their potential business impact, prioritizes them, assigns ownership, recommends an action, and automatically notifies the responsible person.

---

## Business Problem

Supply-chain teams often discover problems after they have already affected the business.

Examples include:

* Products running out of stock
* Suppliers becoming less reliable
* Purchase orders arriving late
* Customer orders becoming backordered
* Excess inventory tying up working capital
* Important risks being buried among hundreds of operational records

The core business question is:

> **Which supply-chain risks require attention right now, how serious are they, what could they cost, and who needs to act?**

The goal of this project is therefore not simply to build a dashboard.

It is to build a system that helps people **make decisions and take action**.

---

# What the System Does

The system transforms operational data through several business-focused stages:

```text
Raw Operational Data
        ↓
Silver — Clean & Standardize
        ↓
Gold — Business-Ready Data
        ↓
Intermediate — Build Business Evidence
        ↓
Risk Engine — Detect & Prioritize Risk
        ↓
Risk Alerts
        ↓
Power BI — Monitor & Investigate
        ↓
n8n — Automate Notifications
        ↓
Email
        ↓
Human Action
```

The core decision flow is:

```text
DATA
  ↓
EVIDENCE
  ↓
RISK
  ↓
IMPACT
  ↓
URGENCY
  ↓
PRIORITY
  ↓
OWNER
  ↓
ACTION
```

---

# Key Risk Areas

The Risk Engine currently evaluates several operational risk categories.

### 1. Stockout Risk

Identifies products that are already out of stock or are likely to run out before replenishment arrives.

The system considers:

* Current inventory
* Historical demand
* Days of cover
* Expected replenishment time
* Supplier delays
* Product criticality
* Unit price

It then estimates the potential financial impact.

---

### 2. Supplier Deterioration

Identifies suppliers whose recent performance is deteriorating compared with their historical baseline.

The system evaluates:

* Recent delivery delays
* Historical delivery performance
* Late-delivery rate
* Purchase-order history
* Fill rate

This helps procurement teams identify supplier problems before they become major operational issues.

---

### 3. Late Delivery Risk

Identifies open or backordered customer orders that are approaching or passing their promised delivery date.

The system provides:

* Order information
* SKU
* Location
* Financial impact
* Severity
* Recommended action
* Responsible owner

---

### 4. Excess Inventory

Identifies products with unusually high inventory coverage.

The purpose is to highlight inventory that may:

* Tie up working capital
* Require replenishment to be paused
* Need redistribution
* Require a promotion or sales intervention

---

# Risk Prioritization

Not every risk deserves the same level of attention.

The system therefore combines:

**Impact × Urgency = Priority**

### Impact

Impact represents the potential financial exposure associated with a risk.

For example, a stockout risk considers:

* Units potentially affected
* Daily demand
* Unit price
* Product criticality

### Urgency

Urgency represents how quickly the business needs to respond.

Urgency is scored from:

```text
1 = Low urgency
2 = Moderate
3 = Elevated
4 = High
5 = Immediate
```

### Priority

The system combines impact and urgency into a priority score.

Alerts are then grouped into:

```text
P1 — Highest priority
P2 — Medium priority
P3 — Lower priority
```

This prevents teams from receiving a long list of equally important alerts.

---

# Ownership

A risk without an owner is just information.

The system therefore assigns each risk to the appropriate business owner.

Examples:

* Inventory/location risks → Planner
* Supplier risks → Procurement owner
* Customer delivery risks → Relevant operational owner

Each alert can contain:

```text
Owner
Owner Email
Recommended Action
```

This allows the automation layer to route the alert directly to the responsible person.

---

# Power BI — Supply Chain Risk Command Centre

The Power BI layer provides the operational monitoring interface.

The dashboard focuses on questions such as:

* How many active risks exist?
* How many are P1?
* What is the potential financial exposure?
* Which risk types are driving the problem?
* Which locations are affected?
* Which owners have the most urgent risks?
* What action should be taken?

The main dashboard is designed as a **risk command centre**, not simply a collection of charts.

---

# Automation with n8n

Power BI helps people see the risks.

But someone still needs to act.

The automation layer connects the risk engine to operational communication:

```text
Databricks
    ↓
Risk Alerts
    ↓
n8n
    ↓
Email
    ↓
Responsible Owner
```

n8n receives the risk records from Databricks and uses the alert information to construct operational notifications.

For example:

```text
SAVANNA SUPPLY CHAIN RISK ALERT

Priority: P1
Risk Type: Stockout Risk
SKU: SKU-035
Location: LOC-NBO

Potential Impact: KES 70,522
Urgency: 5/5
Severity: Critical

Reason:
Already out of stock or backordering.

Recommended Action:
Contact affected customers, expedite replenishment,
and review allocation.

Owner:
Procurement / Planning Owner
```

The important idea is:

> **The system doesn't stop at identifying the problem. It moves the problem toward the person who needs to act.**

---

# Data Architecture

The project uses a layered architecture.

## Silver

The Silver layer cleans and standardizes raw operational data.

Typical activities include:

* Deduplication
* Data-type standardization
* Date parsing
* Status normalization
* Invalid-value handling
* Data-quality corrections

---

## Gold

The Gold layer creates business-ready facts and dimensions.

### Dimensions

* Customers
* Products
* Suppliers
* Locations
* Dates

### Facts

* Order Lines
* Purchase Orders
* Daily Inventory

The Gold layer focuses on creating a reliable business model that downstream analysis can use.

---

## Intermediate

The Intermediate layer builds reusable business evidence.

Examples include:

* Supplier performance
* Inventory enrichment
* Demand history
* Days of cover
* Replenishment estimates
* Open purchase-order exposure

This layer answers:

> **What does the operational data tell us?**

---

## Risk Engine

The Risk Engine converts business evidence into risk decisions.

It determines:

* Whether a risk exists
* Risk type
* Severity
* Financial impact
* Urgency
* Priority
* Owner
* Recommended action

The output is the central risk-alert dataset used by downstream systems.

---

# Technology Stack

| Layer           | Technology     |
| --------------- | -------------- |
| Data processing | Databricks     |
| SQL             | Databricks SQL |
| Data modeling   | SQL            |
| Risk detection  | Databricks SQL |
| Visualization   | Power BI       |
| Automation      | n8n            |
| Notifications   | Email          |
| Version control | Git / GitHub   |

The project intentionally focuses on **business outcomes rather than demonstrating every available technology**.

---

# Project Structure

```text
savanna-supply-chain-risk-system-project/
│
├── 01 - Problem Statement/
│   └── savanna-problem-statement.pdf
│
├── 02 - Data/
│   ├── action_owners.csv
│   ├── customers.csv
│   ├── inventory_snapshots.csv
│   ├── locations.csv
│   ├── order_lines.csv
│   ├── products.csv
│   ├── purchase_orders.csv
│   └── suppliers.csv
│
├── 03 - Silver/
│   └── Silver SQL scripts
│
├── 04 - Gold/
│   └── Gold SQL scripts
│
├── 05 - Intermediate/
│   └── Intermediate SQL scripts
│
├── 06 - Risk Engine/
│   └── Risk Engine SQL
│
├── 07 - Power BI/
│   └── Supply Chain Command Center Dashboard.pbix
│
├── 08 - n8n/
│   └── Automation configuration
│
└── 09 - Documentation/
    └── Project documentation
```

---

# Example Business Questions

This system can answer questions such as:

### Inventory

* Which products are at risk of stocking out?
* How many days of inventory remain?
* Will inventory last until replenishment arrives?
* What is the potential financial impact?
* Which products require immediate intervention?

### Suppliers

* Which suppliers are deteriorating?
* Are recent delays worse than historical performance?
* Which suppliers are chronically late?
* What purchase-order exposure is at risk?

### Customer Orders

* Which customer orders are late?
* Which orders are backordered?
* What financial value is affected?
* Which orders require intervention?

### Management

* What are the highest-priority risks?
* Where is the greatest exposure?
* Which owners have the most urgent work?
* What action should happen next?

---

# Sample Data

This project uses fictional demonstration data representing a supply-chain environment.

The dataset contains:

* Customers
* Products
* Suppliers
* Locations
* Orders
* Purchase Orders
* Inventory
* Action Owners

The data is designed to simulate realistic operational challenges such as:

* Duplicate records
* Inconsistent statuses
* Invalid dates
* Missing delivery information
* Invalid purchase-order dates
* Negative inventory values

No real customer, supplier, or employee data is used.

---

# Getting Started

To explore the project:

### 1. Clone the repository

```bash
git clone https://github.com/mwangomeeric39-debug/savanna-supply-chain-risk-system-project.git
```

### 2. Explore the problem statement

Start with:

```text
01 - Problem Statement/
```

This explains the business problem and the objectives of the system.

### 3. Explore the data

Move to:

```text
02 - Data/
```

Review the fictional operational datasets.

### 4. Follow the transformation layers

Work through:

```text
03 - Silver
04 - Gold
05 - Intermediate
06 - Risk Engine
```

in that order.

### 5. Explore the Power BI dashboard

Open:

```text
07 - Power BI/
```

and load the `.pbix` file in Power BI Desktop.

### 6. Explore the automation

The n8n configuration is available under:

```text
08 - n8n/
```

You will need to configure your own Databricks and email credentials.

**Never use credentials from this repository.**

---

# Important Note About Credentials

This repository is intended to be public.

Therefore:

* Do not commit Databricks personal access tokens.
* Do not commit Gmail credentials.
* Do not commit API keys.
* Do not commit passwords.
* Do not commit `.env` files containing secrets.

Configure your own credentials when reproducing the automation.

---

# What This Project Demonstrates

This project demonstrates how to move beyond:

```text
CSV → Dashboard
```

and build:

```text
Operational Data
       ↓
Reliable Data
       ↓
Business Evidence
       ↓
Risk Detection
       ↓
Financial Impact
       ↓
Prioritization
       ↓
Ownership
       ↓
Recommended Action
       ↓
Dashboard
       ↓
Automated Notification
       ↓
Human Decision
```

The dashboard is only one part of the solution.

The real objective is to connect **data to decisions and decisions to action**.

---

# Project Philosophy

> **Don't build a dashboard just to show data. Build a system that helps someone make a decision.**

A good BI solution should answer three questions:

1. **What is happening?**
2. **Why does it matter?**
3. **What should someone do next?**

That is the thinking behind the Savanna Supply Chain Risk Detection & Decision-Support System.

---

## Author

**Erick Mwangome**

BI Engineer | Founder & Technical Lead

Focused on helping data analysts become job-ready by building real-world Business Intelligence systems and end-to-end BI workflows.

---

## Disclaimer

Savanna Distribution Ltd is a fictional company created for educational and portfolio purposes.

All datasets, suppliers, customers, owners, and business scenarios are fictional.

No real business decisions should be made using this demonstration system.
