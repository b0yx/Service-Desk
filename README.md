# WorkshopOS (Frappe) — Automotive Workshop Management System

A lightweight, production-oriented workshop management system built with the **Frappe Framework**.  
It covers the full workshop flow: customers & vehicles, service requests, parts usage, inventory movements, invoicing, and payments — with professional print formats.

> **Status:** Active development (MVP ready for demo)  
> **Tech:** Frappe Framework (Python + MariaDB + JS)  

---

## Why this project exists

Most small workshops track jobs and customer debts using WhatsApp, paper notes, or Excel — which causes:
- missing service history
- lost parts usage and inventory inaccuracies
- inconsistent invoicing and payment follow-up

**WorkshopOS** provides a clean workflow and audit trail:
- Service Request is the operational record
- Inventory Movement logs stock changes
- Workshop Invoice is the financial snapshot + payments

---

## Core Features

### ✅ Customers & Assets
- **Workshop Customer** (Individual / Company)
- Customer **Vehicles/Devices** as a child table (Car / Device / Other)
- Track plate/serial, make, model, year, VIN, color, notes

### ✅ Service Operations
- **Service Request** lifecycle:
  - Draft → Diagnosing → Waiting Parts → Repairing → Completed → Delivered
- Assign technician + specialization
- Intake notes, diagnosis notes, work report
- Before/After photos support

### ✅ Parts & Inventory
- **Spare Part** catalog (SKU, barcode, unit, selling rate, cost rate, min qty)
- **parts_used** table in Service Request:
  - spare_part, qty, rate, amount, warehouse
- **Inventory Movement** audit log:
  - IN / OUT / ADJUST / TRANSFER
  - reference_doctype + reference_name for traceability

### ✅ Invoicing & Payments
- **Workshop Invoice** linked 1:1 to Service Request
- Invoice totals:
  - labor_cost + parts_total → grand_total
  - paid_amount + outstanding_amount
- Multi-payment support (Cash / Transfer / Card / Online)

### ✅ Professional Print Format
- Clean A4 invoice print template
- Logo support via `/files/LOGO.webp` or Company Logo field
- Print-friendly layout (cards + totals + payments table)

---

## Data Model (High Level)

- **Workshop Customer**
  - child: **Customer Vehicle**
- **Service Request**
  - link: Workshop Customer
  - child: **Service Parts Used**
  - link: **Workshop Invoice** (created on completion)
- **Spare Part**
- **Inventory Movement**
  - references: Service Request / other docs
- **Workshop Invoice**
  - child: **Invoice Item**
  - child: **Invoice Payment**

---

## Workflow (Typical Scenario)

1. Create / select **Workshop Customer**
2. Add/select **Vehicle Identifier**
3. Create **Service Request**
4. Diagnose & assign technician
5. Add **parts_used** and labor cost
6. Complete job → generate **Workshop Invoice**
7. Record payments → status updates (Unpaid / Partially Paid / Paid)
8. Deliver vehicle

---

## Installation (Developer)

### Requirements
- Frappe Bench (supported version)
- MariaDB + Redis
- Node + Yarn (Frappe default)

### Setup
```bash
# inside frappe-bench
bench get-app <your-repo-url>
bench --site <site-name> install-app <app-name>
bench --site <site-name> migrate
bench restart
