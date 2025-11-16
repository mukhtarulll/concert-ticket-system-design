[![id](https://img.shields.io/badge/lang-id-blue.svg)](https://github.com/mukhtarulll/concert-ticket-system-design/blob/main/README.md)

# Concert Ticket Booking System — System Analysis & Design
## Project: Business Process Modeling & Information System Design

This group project analyzes and designs an information system for automating the concert ticket booking process. It includes Flowmap creation (as-is process), Context Diagram, Data Flow Diagram (DFD) Level 0, and Entity Relationship Diagram (ERD) for transforming manual processes into automated digital systems.

---

## Quick Summary

![Context Diagram](CD.png)

**System designed for:**
- **End-to-end automation** of ticket booking process (from booking to reporting)
- **Eliminate manual archives** with centralized database
- **Real-time payment processing** (cash, digital payment)
- **Automated reporting** for Cashier, Sales, and Manager
- **Automatic notifications** for ticket reminders to customers

---

## Business Problem

### Manual Condition (As-Is):
- Audience books tickets at counter with physical forms (2 copies)
- Manual approval by Event Department for data validation
- Physical archives scattered (Counter, Cashier, Sales, Manager)
- Sales reports created manually (3 separate copies)
- Risks: data loss, slow processing, human error, no audit trail

### Digital Solution (To-Be):
- **Online booking system** with real-time data validation
- **Centralized database** for all entities (Audience, Tickets, Payments, Reports)
- **Automated approval workflow** with notifications
- **Multi-payment gateway** (cash & digital)
- **Reporting dashboard** for sales analytics

---

## Deliverables

### 1. Flowmap (As-Is Process)
![Flowmap](Flowmap.png)

**Actors involved:**
- **Audience**: Book tickets, select type & quantity, make payment
- **Counter**: Fill booking form, validate data
- **Event Department**: Approve or reject booking based on data completeness
- **Cashier**: Receive payment, issue receipt, create sales reports
- **Sales**: Receive sales reports for inventory tracking
- **Manager**: Receive aggregate reports for decision making

**Documents generated:**
- Booking Form (2 copies)
- ACC/Reject Form from Event Department
- Payment Receipt
- Sales Report (3 copies)
- Concert Ticket (physical print)

### 2. Context Diagram (System Boundary)
![Context Diagram](CD.png)

Highest-level diagram showing system interaction with external entities:
- **Input**: Audience data, payment request, report request
- **Output**: Printed tickets, receipts, reports (Cashier/Sales/Manager), notifications

**External Entities:**
- Audience (Customer)
- Event Department (Approval Authority)
- Cashier (Payment Processor)
- Sales (Inventory)
- Manager (Reporting)

### 3. Data Flow Diagram (DFD) Level 0
![DFD Level 0](DFD%20LV.0.png)

**Main Processes:**
1. **Reporting** — System generates sales reports for Manager & Sales
2. **Notification** — System sends order notifications to Audience
3. **Payment** — Cashier receives payment, system issues receipt
4. **Ticket Printing** — System prints tickets based on valid receipts
5. **Booking** — Audience orders tickets, system validates & sends to Event for ACC

**Data Stores:**
- **D1: Reporting** — Stores sales reports for analytics
- **D2: Payment** — Stores payment transactions (cash, digital)
- **D3: Ticket Printing** — Stores printed tickets
- **D4: Orders** — Stores booking orders from Audience

### 4. Entity Relationship Diagram (ERD)
![Entity Relationship Diagram](ERD.png)

**Main Entities & Attributes:**

#### **1. Penonton (Audience)**
- **Primary Key**: id_penonton
- **Attributes**: nama (name), email
- **Relationships**: 
  - 1 Audience can make **N Payments** (1:N)
  - 1 Audience can make **N Bookings** (1:N)

#### **2. Acara (Event)**
- **Primary Key**: id_acara
- **Attributes**: tanggal (date), nama (name)
- **Relationships**:
  - 1 Event has **N Bookings** (1:N)

#### **3. Pemesanan (Booking)**
- **Primary Key**: Composite (id_acara, id_penonton)
- **Foreign Keys**: id_acara, id_penonton
- **Attributes**: tanggal (date), id_pemesanan (booking_id), jumlah (quantity), jenis (type), status
- **Relationships**:
  - N:1 with **Audience**
  - N:1 with **Event**
  - 1:N with **Payment**

#### **4. Tiket (Ticket)**
- **Primary Key**: id_tiket
- **Attributes**: jenis (type), tanggal (date)
- **Relationships**:
  - 1 Ticket printed through **N Ticket Printing** (1:N via relationship)
  - Many-to-Many with **Cashier** via **Ticket Printing**

#### **5. Kasir (Cashier)**
- **Primary Key**: id_kasir
- **Attributes**: nama (name)
- **Relationships**:
  - 1 Cashier processes **N Payments** (1:N)
  - 1 Cashier prints **N Tickets** via **Ticket Printing** (1:N)
  - 1 Cashier creates **N Reports** (1:N)

#### **6. Pembayaran (Payment)**
- **Primary Key**: Composite (id_pembayaran, id_pemesanan)
- **Foreign Keys**: id_pemesanan
- **Attributes**: jenis (type)
- **Relationships**:
  - N:1 with **Audience**
  - N:1 with **Cashier**

#### **7. Pencetakan Tiket (Ticket Printing)**
- **Relationship Entity** (Many-to-Many resolver)
- **Primary Key**: Composite (id_tiket, id_pembayaran)
- **Foreign Keys**: id_tiket, id_pembayaran, id_kasir
- **Relationships**:
  - N:1 with **Ticket**
  - N:1 with **Payment**
  - N:1 with **Cashier**

#### **8. Pelaporan (Reporting)**
- **Primary Key**: Composite (id_laporan, id_kasir)
- **Foreign Keys**: id_kasir
- **Attributes**: tanggal (date), total_penjualan (total_sales - aggregated from Payment)
- **Relationships**:
  - N:1 with **Cashier**
  - 1:1 with **Sales Department** (report copy)
  - 1:1 with **Manager** (report copy)

#### **9. Bagian Penjualan (Sales Department)**
- **Primary Key**: id_penjualan
- **Attributes**: nama (name), kontak (contact)
- **Relationships**:
  - 1:1 with **Reporting** (receives report)

#### **10. Manajer (Manager)**
- **Primary Key**: id_manajer
- **Attributes**: nama (name), kontak (contact)
- **Relationships**:
  - 1:1 with **Reporting** (receives report)

**Cardinality:**
- **1:N** — One-to-Many (e.g., 1 Cashier → N Payments)
- **N:M** — Many-to-Many (e.g., Ticket ↔ Payment via Ticket Printing)
- **1:1** — One-to-One (e.g., Reporting → Manager)

---

## Database Normalization (3NF)

### First Normal Form (1NF)
- All attributes are atomic (no multi-valued attributes)  
- Every table has a primary key

### Second Normal Form (2NF)
- Meets 1NF requirements  
- No partial dependency (all non-key attributes fully depend on primary key)  
- Example: Booking uses composite key (id_acara, id_penonton), all other attributes (date, quantity, status) fully depend on both keys

### Third Normal Form (3NF)
- Meets 2NF requirements  
- No transitive dependency (non-key attributes don't depend on other non-key attributes)  
- Example: Reporting only stores id_kasir (FK), doesn't store cashier_name (retrieved from Cashier table via JOIN)

---

## Key Features (System Requirements)

### Functional
1. **Online Ticket Selection**
   - Choose concert time
   - Select ticket type (VIP, Regular, etc.)
   - Select ticket quantity
   - Real-time availability check

2. **Automated Approval Workflow**
   - Form validation (complete data?)
   - Auto-send to Event Department for ACC
   - Email/notification for approval status

3. **Multi-Payment Gateway**
   - Cash (on-site)
   - Digital payment (e-wallet, credit card)
   - Automatic payment confirmation

4. **Ticket Generation & Delivery**
   - Generate QR code for e-ticket
   - Automatic email delivery
   - Physical print option at counter

5. **Automated Reporting**
   - Real-time dashboard for Cashier (daily sales)
   - Report for Sales (inventory sold)
   - Executive summary for Manager (trends, revenue)

6. **Notification System**
   - Booking confirmation via email/SMS
   - D-1 concert reminder
   - Payment confirmation

### Non-Functional
- **Performance**: Handle 1000+ concurrent bookings
- **Security**: Encrypted payment data, role-based access
- **Usability**: Mobile-friendly interface
- **Reliability**: 99.9% uptime, daily data backup

---

## Technology Stack (Proposed)

If this system were to be implemented, recommended stack:

| Component | Technology |
|-----------|------------|
| **Frontend** | React.js / Vue.js (responsive web app) |
| **Backend** | Node.js (Express) / Python (Flask/Django) |
| **Database** | PostgreSQL / MySQL |
| **Payment Gateway** | Midtrans / Stripe / Xendit |
| **Notification** | Twilio (SMS), SendGrid (Email) |
| **Reporting** | Chart.js / D3.js for dashboard visualization |
| **QR Code** | QR Code Generator library |
| **Deployment** | Docker, AWS/GCP, CI/CD pipeline |

---

## Business Impact

If this system were implemented:

### Efficiency Gains
- **70% reduction** in booking processing time (manual form → digital)
- **90% reduction** in reporting overhead (auto-generate vs manual 3-copy reports)
- **Eliminate paper waste** (forms, physical archives)

### Revenue Opportunities
- **24/7 booking availability** → increase sales window
- **Data analytics** → optimize pricing, identify peak demand
- **Customer retention** → email marketing & loyalty program via database

### Risk Mitigation
- **No data loss** → centralized database with backup
- **Audit trail** → track all transactions for compliance
- **Fraud prevention** → QR code validation, payment gateway security

---

## License

- **Project**: Academic assignment for educational purposes
- **Diagrams**: Open for reference & learning
- **Figma Prototype Link:** https://www.figma.com/design/g07HpTiRGC9yP1MyeeSamZ/ANSI?node-id=163-271&t=0aDKtOrFROnJeyAy-1
