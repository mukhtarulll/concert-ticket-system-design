[![en](https://img.shields.io/badge/lang-en-red.svg)](https://github.com/mukhtarulll/concert-ticket-system-design/blob/main/README.en.md)

# Sistem Pemesanan Tiket Konser — System Analysis & Design
## Project: Business Process Modeling & Information System Design

Proyek kelompok ini menganalisis dan merancang sistem informasi untuk otomatisasi proses pemesanan tiket konser. Meliputi pembuatan Flowmap (as-is process), Context Diagram, Data Flow Diagram (DFD) Level 0, dan Entity Relationship Diagram (ERD) untuk transformasi proses manual menjadi sistem digital terotomatisasi.

---

## Ringkasan Singkat

![Context Diagram](CD.png)

**Sistem dirancang untuk:**
- **Otomatisasi end-to-end** proses pemesanan tiket (dari booking hingga reporting)
- **Eliminasi arsip manual** dengan database terpusat
- **Real-time payment processing** (cash, digital payment)
- **Automated reporting** untuk Kasir, Bagian Penjualan, dan Manajer
- **Notifikasi otomatis** pengingat tiket untuk customer

---

## Business Problem

### Kondisi Manual (As-Is):
- Penonton memesan tiket di loket dengan formulir fisik (2 rangkap)
- Approval manual oleh Bagian Acara untuk validasi data
- Arsip fisik tersebar (Loket, Kasir, Penjualan, Manajer)
- Laporan penjualan dibuat manual (3 rangkap terpisah)
- Risiko: data loss, slow processing, human error, no audit trail

### Solusi Digital (To-Be):
- **Sistem online booking** dengan validasi data real-time
- **Database terpusat** untuk semua entitas (Penonton, Tiket, Pembayaran, Laporan)
- **Automated approval workflow** dengan notifikasi
- **Multi-payment gateway** (cash & digital)
- **Dashboard reporting** untuk analitik penjualan

---

## Deliverables

### 1. Flowmap (As-Is Process)
![Flowmap](Flowmap.png)

**Aktor yang terlibat:**
- **Penonton**: Memesan tiket, memilih jenis & jumlah, melakukan pembayaran
- **Loket**: Mengisi formulir pemesanan, validasi data
- **Acara**: Approve atau reject booking berdasarkan kelengkapan data
- **Kasir**: Menerima pembayaran, menerbitkan kwitansi, membuat laporan penjualan
- **Penjualan**: Menerima laporan penjualan untuk inventory tracking
- **Manajer**: Menerima laporan agregat untuk decision making

**Dokumen yang dihasilkan:**
- Formulir Pemesanan (2 rangkap)
- Form ACC/Reject dari Bagian Acara
- Kwitansi Pembayaran
- Laporan Penjualan (3 rangkap)
- Tiket Konser (cetak fisik)

### 2. Context Diagram (System Boundary)
Diagram level tertinggi yang menunjukkan interaksi sistem dengan external entities:
- **Input**: Data penonton, permintaan pembayaran, permintaan laporan
- **Output**: Tiket cetak, kwitansi, laporan (Kasir/Penjualan/Manajer), notifikasi

**External Entities:**
- Penonton (Customer)
- Bagian Acara (Approval Authority)
- Kasir (Payment Processor)
- Penjualan (Inventory)
- Manajer (Reporting)

### 3. Data Flow Diagram (DFD) Level 0
![DFD Level 0](DFD%20LV.0.png)

**Proses Utama:**
1. **Pelaporan** — Sistem generate laporan penjualan untuk Manajer & Penjualan
2. **Notifikasi** — Sistem kirim notifikasi pesanan ke Penonton
3. **Pembayaran** — Kasir menerima pembayaran, sistem terbitkan kwitansi
4. **Pencetakan Tiket** — Sistem cetak tiket berdasarkan kwitansi valid
5. **Pemesanan** — Penonton order tiket, sistem validasi & kirim ke Acara untuk ACC

**Data Stores:**
- **D1: Pelaporan** — Store laporan penjualan untuk analitik
- **D2: Pembayaran** — Store transaksi pembayaran (cash, digital)
- **D3: Pencetakan Tiket** — Store tiket yang sudah tercetak
- **D4: Pesanan** — Store booking order dari Penonton

### 4. Entity Relationship Diagram (ERD)
**Entitas Utama:**
- **Penonton** (ID, Nama, Email, No. HP, Alamat)
- **Tiket** (ID, Jenis Tiket, Harga, Waktu Konser, Status)
- **Pesanan** (ID, ID_Penonton, ID_Tiket, Jumlah, Total Harga, Status)
- **Pembayaran** (ID, ID_Pesanan, Metode Pembayaran, Tanggal, Jumlah)
- **Laporan** (ID, Tanggal, Total Penjualan, Jumlah Tiket Terjual)

**Relationships:**
- Penonton **membuat** Pesanan (1:N)
- Pesanan **mencakup** Tiket (N:M via junction table)
- Pesanan **diproses menjadi** Pembayaran (1:1)
- Pembayaran **menghasilkan** Laporan (N:1, aggregated)

---

## Key Features (System Requirements)

### Fungsional
1. **Online Ticket Selection**
   - Pilih waktu konser
   - Pilih tipe tiket (VIP, Reguler, dll)
   - Pilih jumlah tiket
   - Real-time availability check

2. **Automated Approval Workflow**
   - Form validation (data lengkap?)
   - Auto-send ke Bagian Acara untuk ACC
   - Email/notifikasi approval status

3. **Multi-Payment Gateway**
   - Cash (on-site)
   - Digital payment (e-wallet, kartu kredit)
   - Payment confirmation otomatis

4. **Ticket Generation & Delivery**
   - Generate QR code untuk e-ticket
   - Email delivery otomatis
   - Cetak fisik option di loket

5. **Automated Reporting**
   - Dashboard real-time untuk Kasir (daily sales)
   - Report untuk Penjualan (inventory sold)
   - Executive summary untuk Manajer (trend, revenue)

6. **Notification System**
   - Konfirmasi booking via email/SMS
   - Reminder H-1 konser
   - Payment confirmation

### Non-Fungsional
- **Performance**: Handle 1000+ concurrent bookings
- **Security**: Encrypted payment data, role-based access
- **Usability**: Mobile-friendly interface
- **Reliability**: 99.9% uptime, backup data daily

---

## Dokumen Input-Output

### Input:
- Data penonton (nama, email, no. HP, alamat)
- Pilihan tiket (waktu, tipe, jumlah)
- Data pembayaran (metode, jumlah)
- Approval ACC dari Bagian Acara

### Output:
- Formulir pemesanan (digital, tersimpan di database)
- Kwitansi pembayaran (PDF/cetak)
- Tiket konser (QR code + cetak fisik)
- Laporan penjualan:
  - 1 copy untuk Kasir (daily summary)
  - 1 copy untuk Bagian Penjualan (inventory tracking)
  - 1 copy untuk Manajer (executive dashboard)

---

## Technology Stack (Proposed)

Jika sistem ini diimplementasikan, stack yang disarankan:

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
- **Business Process Modeling**: Mapping manual workflows dengan Flowmap
- **Requirements Analysis**: Identifying input/output, stakeholders, pain points
- **Process Optimization**: Transformasi manual → automated

### System Design
- **Context Diagram**: Defining system boundaries & external entities
- **Data Flow Diagram (DFD)**: Modeling information flow antar proses
- **Entity Relationship Diagram (ERD)**: Database schema design dengan relationships
- **Normalization**: Ensuring 3NF untuk mengurangi redundancy

### Documentation
- Clear, structured, professional documentation (academic report format)
- Visual diagrams dengan standard notation (Gane-Sarson DFD, Crow's foot ERD)
- Input-output specification

---

## Business Impact

Jika sistem ini diimplementasikan:

### Efficiency Gains
- **70% reduction** in booking processing time (manual form → digital)
- **90% reduction** in reporting overhead (auto-generate vs manual 3-copy reports)
- **Eliminate paper waste** (formulir, arsip fisik)

### Revenue Opportunities
- **24/7 booking availability** → increase sales window
- **Data analytics** → optimize pricing, identify peak demand
- **Customer retention** → email marketing & loyalty program via database

### Risk Mitigation
- **No data loss** → centralized database dengan backup
- **Audit trail** → track semua transaksi untuk compliance
- **Fraud prevention** → QR code validation, payment gateway security

---

## Lisensi

- **Project**: Academic assignment untuk educational purposes
- **Diagrams**: Open untuk referensi & learning
- **Figma Prototype Link:** [https://www.figma.com/design/g07HpTiRGC9yP1MyeeSamZ/ANSI?node-id=163-271&t=0aDKtOrFROnJeyAy-1](https://www.figma.com/proto/g07HpTiRGC9yP1MyeeSamZ/ANSI?node-id=195-4357&p=f&t=c40UTSRVGaDDmjo0-1&scaling=scale-down&content-scaling=fixed&page-id=163%3A271)
