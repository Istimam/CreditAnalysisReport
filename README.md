# 📊 Managerium - Credit Analysis Report

> A streamlined, high-performance, single-report interactive mockup called **Credit Analysis Report** for **MANAGERIUM** (`POS > Reports > POS SALES REPORT`). Built purely with **Vanilla HTML5, CSS3, and JavaScript (ES6+)** — zero frameworks, zero external CDNs, zero npm/build steps, fully offline-capable, and double-clickable.

🔗 **Live Demo (GitHub Pages):**  
**[https://istimam.github.io/CreditAnalysisReport/](https://istimam.github.io/CreditAnalysisReport/)**

📂 **Archived Version:**  
The earlier multi-view iteration has been archived as [`credit-report-v1-four-views.html`](credit-report-v1-four-views.html).

---

## 📌 Overview & Simplified Design Philosophy

Following business analysis feedback, the cluttered multi-view design has been replaced by **ONE unified Credit Analysis Report** controlled by **three clear checkboxes**:
1. `[ ] Is Details`: Toggles between the **Customer Summary** view and the **Invoice-level Details** view.
2. `[ ] Is Collection`: Reconfigures date filtering to prioritize **Collection Date Range** (mandatory) alongside an optional **Sales Date Range**.
3. `[ ] Is Credit History`: Switches to a single-customer ledger statement modeled after the Managerium *Item History* screen.

The report area stays completely **blank on initial load** until the user clicks **View** and validation succeeds. `EXCEL` and `PRINT/PDF` buttons are disabled until a valid report is rendered.

---

## 🗂️ Report Modes & Layouts

### 1. 📋 Summary View (Default & Is Collection Summary)
* **Ordering**: Ordered strictly by **Partner Code** ascending. No cluttering group sub-rows.
* **Granularity**: One row per customer.
* **Partner Code Drilldown**: Clicking on the **Partner Code** hyperlink opens a **Customer Details** modal (matching the Managerium design) showing all invoices filtered in that customer's `Invoice Count`, complete with `Gross Amount`, `Credit Receive Amount`, `Due Amount`, `Remaining Balance`, `Net Amount`, aggregate totals, and an Excel export button.
* **Columns**:
  `SL` | `Partner Code` (clickable drilldown) | `Customer Name` (plain text) | `Phone` | `Customer Group` | `Invoice Count` |  
  **Sales Band**: `Gross Sale` | `Discount` | `Disc %` | `Round Off` | `Net Sale` | `Total Return` |  
  **Credit Band**: `Credit Sold` | `Credit Returned` | `Net Credit` |  
  **Collection Band**: `[One Column Per Active Account]` | `Total Collected` |  
  `Credit Due` | `Remaining Cr Balance` | `Cr Limit`
* **Dynamic Collection Columns**: Only accounts with at least one collection in the filtered result appear (e.g., `Cash`, `Cash-02`, `bKASH`, `UCB`, `Bank1`, `Bank2`); all-zero accounts are automatically omitted.
* **Sticky Elements & Aggregate Disc %**: Table header and dark teal `Grand Total` row stay sticky when scrolling vertically. The Grand Total line displays the aggregate discount percentage ($$\text{Grand Discount} / \text{Grand Gross}$$, 4 decimal places).

---

### 2. 📑 Details View (`Is Details` Mode)
* **Granularity**: One invoice block per invoice, with one sub-row per collection receipt.
* **Plain Invoice Numbers**: `Invoice No` is displayed as clean unclickable text.
* **Rowspan Logic**: Invoice-level metadata (`SL`, `Partner Code`, `Customer Name`, `ID`, `Phone`, `Customer Group`, `Invoice Date`, `Invoice No`, `Sales JV No`, Sales Band, Credit Band, `Total Collected`, `Credit Due`, `Remaining Cr Balance`, `Cr Limit`) use `rowspan` across all collection rows of that invoice.
  * Invoices with multiple collections (e.g., 2 or 3 collections across different payment methods) occupy multiple rows inside one clean block.
  * Invoices with no collections show a single row with blank/hyphen collection cells and `0.00` Total Collected.
* **Pagination**: Paginates by complete **invoice blocks**, ensuring an invoice's collection rows are never split across pages.
* **Voucher Drilldown**: Both Sales JV and Collection JV numbers are clickable links opening the **Account Journal Details** modal showing double-entry journal vouchers (`COA No`, `Account Name`, `Debit`, `Credit`).

---

### 3. 📜 Customer Credit History (`Is Credit History` Mode)
Modeled directly on the Managerium **Item History** interface:
* **Two-Column Key/Value Summary Card**:
  * *Left Column*: Customer Name, Partner Code, Phone, Customer Group, Credit Limit, Credit Days, Created Time.
  * *Right Column*: Credit Sold, Credit Returned, Total Collected, Closing Due (as of To Date), Remaining Cr Balance (with ℹ️ tooltip: `Cr Limit - current due`), and Invoice Count.
* **Chronological Running-Balance Ledger Table**:
  * `SL` | `Date` | `Transaction Type` (`Credit Sale`, `Credit Return`, `Collection`) | `Reference` | `JV/Collection No` | `Account/Method` | `Gross Sale` | `Discount` | `Net Sale` | `Credit Sold` | `Credit Returned` | `Collected` | `Balance (Due)`.
  * **First Row**: `Opening Balance` (accumulated debt prior to `From Date`).
  * **Last Row**: `Total:` with column sums and closing balance.
  * Running balance formula:
    $$\text{Balance} = \text{Previous Balance} + \text{Credit Sold} - \text{Credit Returned} - \text{Collected}$$

---

## 🧮 Accounting & Calculation Rules

| Metric | Business & Accounting Rule |
| :--- | :--- |
| **Credit Sold** | Portion of the invoice placed on credit at the time of sale. |
| **Total Return** | Total merchandise returned on the invoice (shown separately; not subtracted from Net Sale). |
| **Credit Returned** | Portion of return offsetting credit debt (never exceeds Credit Sold). |
| **Net Credit** | `Credit Sold − Credit Returned`. |
| **Net Sale** | `Gross Sale − Discount ± Round Off`. |
| **Disc %** | `Discount / Gross Sale`. |
| **Credit Due** | `Net Credit − Total Collected`. |
| **Remaining Cr Balance** | `Cr Limit − Customer's Total Current Credit Due`. |
| **Collection Date Rule** | A collection can never be dated before its corresponding invoice. |

> ⚠️ **Business Analyst Note (Is Collection Mode):**  
> In `Is Collection` mode, `Credit Due` is computed as `Net Credit - Total Collected (collected in range)`. The mock dataset is generated so this does not conflict with earlier collections. **The real-world business rule for handling prior collections in this mode requires formal confirmation from the Business Analyst (BA) team.**

---

## ✅ Validation Rules (Every `View` Click)

1. **Mandatory Field Checks**: Red outline and error text under any empty required fields. Report container remains **completely blank** if invalid.
2. **Date Order**: `From Date <= To Date` for all ranges.
3. **Cross-Range & Dynamic Calendar Constraints in Is Collection Mode**:
   * **Dynamic Calendar Range**: When `Collection From Date` and `Collection To Date` are selected, both `Sales From Date` and `Sales To Date` pickers automatically restrict valid calendar choices with `min = Collection From Date` and `max = Collection To Date`. Dates outside this window are disabled in the browser picker.
   * `Sales From Date` cannot be smaller than `Collection From Date`.
   * `Sales To Date` cannot be greater than `Collection To Date`.
   * If either `Sales From Date` or `Sales To Date` is filled, both are mandatory.
4. **Credit History Mode**: Requires a customer to be selected via autocomplete (`min 3 characters`).
5. **Auto-Clear on Filter Change**: Whenever any filter, input, or mode checkbox changes, the active report and export buttons immediately disappear until **View** is clicked again.

---

## 📤 Export & Print Capabilities

* **EXCEL (`.xls` Blob)**:
  * Downloads currently active view without page truncation.
  * In Details mode, customer and invoice details repeat on each row, while invoice-level money figures appear **only on the first row** to prevent double-counting in Excel sums without using fragile merged cells.
* **PRINT/PDF**:
  * Native `@media print` landscape stylesheet. Hides sidebar, top bar, and filter controls. Automatically repeats table headers on every printed page.

---

## 🧭 6-Step Verification & Demo Script

1. **Default Summary for November**:
   * Open `index.html`. Note the initial blank state and disabled export buttons.
   * Dates are prefilled to `01-11-2026` to `04-11-2026`. Click **View**.
   * Observe the table with Customer Partner Code ordering, Sales / Credit / Collection / Due bands, dynamic account columns, and sticky Grand Total.
2. **Is Details with Multiple Collections per Invoice**:
   * Check **Is Details** and click **View**.
   * Locate customer `FP-2709577` (KAMAL HOSSAIN) and invoice `132611104508`.
   * Observe the single invoice block with `rowspan="3"` across three separate collection receipts (`JV-26103440` UCB 500 BDT, `JV-26103441` Bank1 300 BDT, `JV-26103442` Cash 200 BDT).
3. **Is Collection Mode (October Sales Collected Today)**:
   * Check **Is Collection** (uncheck Is Details).
   * Set **Collection From** = `2026-11-04` and **Collection To** = `2026-11-04`.
   * Set **Sales From** = `2026-10-01` and **Sales To** = `2026-10-31`. Click **View**.
   * Only October credit invoices that had collections posted today are displayed.
4. **Validation Test (Sales To > Collection To)**:
   * Set **Sales To Date** = `2026-11-15` (leaving Collection To as `2026-11-04`). Click **View**.
   * Observe the validation failure: red outline on Sales To Date, error message `"Sales To date cannot be greater than Collection To Date"`, the report area clears to blank, and export buttons are disabled.
5. **Is Credit History (Opening Balance, Running Due & Total Row)**:
   * Check **Is Credit History**. Notice `Is Details` and `Is Collection` are hidden.
   * In Customer search, type `SELIM` and select `MD. SELIM HOWLADER (FP-2709573)`. Click **View**.
   * Review the two-column summary card and the chronological ledger. Note Row 1 is `Opening Balance`, rows reflect running due, and the bottom row is `Total:`.
6. **Excel Export and Print**:
   * Click **📊 EXCEL** to download the clean `.xls` spreadsheet.
   * Click **🖨️ PRINT/PDF** to verify the clean landscape print preview.
