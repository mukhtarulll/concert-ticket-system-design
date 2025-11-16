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
**Main Entities:**
- **Audience** (ID, Name, Email, Phone, Address)
- **Ticket** (ID, Ticket Type, Price, Concert Time, Status)
- **Order** (ID, Audience_ID, Ticket_ID, Quantity, Total Price, Status)
- **Payment** (ID, Order_ID, Payment Method, Date, Amount)
- **Report** (ID, Date, Total Sales, Tickets Sold)

**Relationships:**
- Audience **creates** Orders (1:N)
- Orders **includes** Tickets (N:M via junction table)
- Orders **processed into** Payment (1:1)
- Payments **generate** Reports (N:1, aggregated)

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

## Input-Output Documents

### Input:
- Audience data (name, email, phone, address)
- Ticket selection (time, type, quantity)
- Payment data (method, amount)
- ACC approval from Event Department

### Output:
- Booking form (digital, stored in database)
- Payment receipt (PDF/print)
- Concert ticket (QR code + physical print)
- Sales reports:
  - 1 copy for Cashier (daily summary)
  - 1 copy for Sales Department (inventory tracking)
  - 1 copy for Manager (executive dashboard)

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

## Skills Demonstrated

### System Analysis
- **Business Process Modeling**: Mapping manual workflows with Flowmap
- **Requirements Analysis**: Identifying input/output, stakeholders, pain points
- **Process Optimization**: Manual → automated transformation

### System Design
- **Context Diagram**: Defining system boundaries & external entities
- **Data Flow Diagram (DFD)**: Modeling information flow between processes
- **Entity Relationship Diagram (ERD)**: Database schema design with relationships
- **Normalization**: Ensuring 3NF to reduce redundancy

### Documentation
- Clear, structured, professional documentation (academic report format)
- Visual diagrams with standard notation (Gane-Sarson DFD, Crow's foot ERD)
- Input-output specification

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
- **Figma Prototype Link:** https://www.figma.com/proto/g07HpTiRGC9yP1MyeeSamZ/ANSI?node-id=195-4357&p=f&t=c40UTSRVGaDDmjo0-1&scaling=scale-down&content-scaling=fixed&page-id=163%3A271
