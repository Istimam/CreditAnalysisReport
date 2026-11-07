# 📊 Managerium - Enhanced Credit Customer Report

> A high-performance, self-contained interactive mockup of the enhanced **Credit Customer Report** for **MANAGERIUM** (`POS > Reports > POS SALES REPORT`). Built strictly with **Vanilla HTML, CSS, and JavaScript** — zero external frameworks, zero CDNs, zero npm/build steps, fully offline-capable, and double-clickable.

🔗 **Live Demo (GitHub Pages):**  
**[https://istimam.github.io/CreditAnalysisReport/](https://istimam.github.io/CreditAnalysisReport/)**

---

## 📌 Executive Summary & Purpose

In standard retail & enterprise POS workflows, customer credit accounts often suffer from discrepancies when returns and post-sale adjustments occur. This enhanced report solves these accounting ambiguities by:
1. **Never confusing returns with credit balances**: Segregating **Credit Sold**, **Credit Returned**, and **Net Credit** side-by-side.
2. **Reconciling POS invoices with accounting sub-ledgers**: Identifying invoices modified post-sale without automated journal voucher entries.
3. **Providing instant cohort tracking**: Analyzing invoices sold in any period through their collection lifecycle with visual funnels and aging brackets.

---

## 🚀 Key Features & Architectural Highlights

### 1. 🗂️ Five Specialized Report Views
* **Customer Credit Report (Default)**: Hierarchical grouping by Customer Group (e.g., *Ziska Factory*, *Office Staff*, *Warehouse Staff*, *Walk-in Credit*) with collapsible group rows, subtotal summaries, sticky Grand Total, and sticky first 3 columns.
* **Credit Sales Report**: Granular invoice-level audit table. Includes status badges (`Open`, `Partly collected`, `Collected`, `Part-returned`, `Fully returned`), hoverable return vouchers with details, and orange post-sale edit audit flags.
* **Collection Report**: Individual payment receipts (`CR-YYMMNNN`), method classifications (*Cash*, *Bank - Brac/UCB*, *Wallet - bKash/Nagad*), sources (*POS Single*, *Bulk Batch*, *Salary Deduction*), and a top collection summary bar.
* **Sales vs Collection (Cohort View)**: Invoices sold in date range tracked against collection progress. Features a **Mini-Funnel** (`Sold` ➔ `Fully Collected` ➔ `Partly Collected` ➔ `Not Collected` + `Fully Returned`) and a stacked **Aging Bar** (*Not Due*, *1–30d*, *31–60d*, *60+d* overdue).
* **Single Customer Details (`Is Details` Mode)**: Clean 2-column customer financial summary, plus a full chronological running-balance ledger with an **Accounting View (Debit / Credit)** toggle.

---

### 2. 🧮 Core Accounting Definitions

| Metric | Accounting Definition |
| :--- | :--- |
| **Credit Sold** | The amount of the invoice placed on credit at the exact time of sale. |
| **Total Return** | Value of all goods returned or exchanged on the invoice (cash + credit portions). |
| **Credit Returned** | The return portion that directly offsets credit owed (never exceeds Credit Sold). |
| **Net Credit** | `Credit Sold − Credit Returned` (The true receivable asset recognized). |
| **Collected** | Actual payments received against net credit. |
| **Credit Due** | `Net Credit − Collected` (The true outstanding customer debt). |
| **Net Sale** | `Gross − Discount − Total Return ± Round Off`. |

---

### 3. ⚖️ Summary Strip & Mathematical Reconciliation
Above all tables, an interactive KPI strip provides live mathematical verification:
$$\text{Opening Due} + \text{Credit Sold} - \text{Credit Returned} - \text{Collections} = \text{Closing Due}$$
Displays a green **✔ Perfectly Balanced** indicator when numbers reconcile to within 0.01 BDT.

---

### 4. 🔍 Ledger Difference & Edit-Audit Drill-Down
* **Show Ledger Difference**: Compares POS Report Due with Accounts Sub-Ledger Balances. Highlights discrepancies in bold red.
* **"Why the Difference?" Modal**: Explains root causes (e.g., *"Payment method edited after sale from Cash to Credit, no automated ledger JV generated"*).
* **Edit History Audit Trail**: Tracks who edited the ticket, date/time, original payment split, new payment split, and unposted accounting flags.

---

### 5. 📑 Modals & Interactive Journal Details
* **Invoice Detail Modal**: Full breakdown of sale payment splits, attached return vouchers (`RT-xxxx`), collection receipts (`CR-xxxx`), and the post-sale edit history.
* **Account Journal Details Modal**: Inspects mock double-entry general ledger lines (`Chart of Account No`, `Account Name`, `Debit`, `Credit`, `Partner`).

---

### 6. 📤 Native Excel Export & Print-to-PDF
* **Excel (`.xls` Blob)**: Generates a complete Excel workbook client-side containing the Definitions reference table, column bands, all records (bypassing pagination), and voucher references.
* **Print / PDF**: `@media print` stylesheet formatted specifically for **A4 Landscape**, suppressing navigation elements while repeating table headers across pages.

---

## 🛠️ Technology Stack

* **HTML5**: Semantic markup matching Managerium design guidelines.
* **CSS3**: Custom Managerium palette, responsive layout, sticky column headers, custom scrollbars, print stylesheet.
* **JavaScript (ES6+)**:
  * In-memory deterministic dataset (1,500 customers, 6,000 credit invoices, 4,000+ collections, 600 returns) powered by a seeded Mulberry32 pseudo-random generator.
  * Real-time client-side filtering, sorting, pagination (25 / 50 / 100 rows per page), and multi-criteria autocomplete.

---

## 💻 Local Setup & Usage

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Istimam/CreditAnalysisReport.git
   cd CreditAnalysisReport
   ```
2. **Open the report:**
   * Simply **double-click** `index.html` in your file explorer, OR
   * Serve with any local HTTP server:
     ```bash
     npx serve .
     # or
     python -m http.server 8000
     ```

---

## 🧭 7-Step Interactive Demo Script

1. **Default Customer Credit Report**: View current month (`01-11-2026` to `04-11-2026`) and review the reconciliation equation and side-by-side Credit Sold / Returned / Net columns.
2. **Preset Ranges**: Click the **Last month** preset chip (`01-10-2026` to `31-10-2026`), open **⚙️ More filters**, and set Min/Max credit limits.
3. **Credit Sales Report**: Switch to **Credit Sales Report**, check **Returned invoices only**, and hover over the `↩` return icons.
4. **Sales vs Collection**: Switch to **Sales vs Collection** to inspect the Cohort Funnel and Overdue Aging Bar.
5. **Collection Report**: Switch to **Collection Report**, filter by **Bank**, and click a voucher (`CR-xxxx`) to view double-entry journal lines.
6. **Ledger Discrepancy Analysis**: Toggle **Show Ledger Difference**, filter by **Show only customers with difference**, and click a red difference value (`+540.00 ⚠`) to drill into the root cause.
7. **Is Details & Export**: Click any customer link to enter **Is Details** mode, toggle **Accounting View (Debit / Credit)**, and click **EXCEL** or **PRINT/PDF**.

---

## 📄 License
Internal prototype designed for **MANAGERIUM POS (AKIJ iBOS)**. All rights reserved.
