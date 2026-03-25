# Prompt Library: Financial Services Firm — Excel & Office Edition

> Reusable operational prompts for a services firm that builds products and solutions for financial institutions — banks, insurance companies, investment firms, and capital markets. Each prompt is ready to paste into Claude Code. Replace all bracketed placeholders before use.

---

## How We Use Claude Code

Claude Code is used in two distinct modes at this firm:

**Internal** — increasing efficiency, productivity, and quality of internal work:
- Automating repetitive analyst tasks (data cleaning, report generation, reconciliations)
- Accelerating development of Excel tools and VBA macros
- Building and maintaining internal templates and standards
- Reviewing and auditing deliverables before they reach clients

**External** — expanding value to clients:
- Generating new solution proposals tailored to client-specific data and processes
- Prototyping automations and tools that clients can then adopt
- Producing richer, faster analysis and recommendations
- Building artifacts (scripts, templates, models) that are delivered as packaged files

---

## The Security Constraint — And How to Work Around It

Most of our financial institution clients operate under strict IT and CISO policies. Claude Code **cannot** be installed, run, or connect to anything inside a client environment directly.

**The creative approach: build the artifact locally, deliver it.**

Claude Code runs on our machines. We use it to build the output — the VBA macro, the Excel model, the Power Query script, the Word template, the specification document. The client receives a finished file that runs entirely within their approved tools (Excel, Word, Office). No connection, no agent, no installation required on their end.

| What Claude Code does (our side) | What the client gets |
|----------------------------------|----------------------|
| Writes and tests VBA macro | A .bas or .xlsm file they import and run |
| Builds Excel model with formulas | A .xlsx they open and use |
| Generates Power Query script | A query they paste into their Power Query editor |
| Writes a specification | A Word document their IT team implements |
| Produces analysis from exported data | A report or dashboard |

**Practical rules for client work:**
- Never connect Claude Code to live client systems, APIs, or databases
- Only use exported/anonymized client data on our local machines
- Deliver final artifacts as standalone files (xlsx, docx, bas, sql, pdf)
- All macros must work fully offline inside Excel/Word — no external calls
- When a client needs automation beyond Office tools, deliver a detailed spec for their IT team to implement

---

## Placeholder Glossary

- `[FIRM_NAME]` — our firm's name
- `[CLIENT_NAME]` — the specific financial institution client
- `[CLIENT_TYPE]` — bank / insurance / investment firm / other
- `[FILE_PATH]` — path to the relevant file on our local machine
- `[SHEET_NAME]` — name of the relevant worksheet
- `[REPORT_NAME]` — name of the report or output document
- `[ANALYST_NAME]` — responsible analyst
- `[PERIOD]` — reporting period (e.g., Q1 2025, January 2025)
- `[COLUMN_NAME]` — specific column or field name
- `[DATA_SOURCE]` — the data source (e.g., core banking export, actuarial system dump)

---

## 1. EXCEL AUTOMATION & VBA

### 1a. VBA Macro — Automated Report Generator
```text
Write a VBA macro for an Excel workbook at [FILE_PATH].

The macro should:
1. Open or activate the workbook.
2. Read raw data from the sheet named [SHEET_NAME].
3. Clean the data: remove blank rows, trim whitespace, standardize date formats to DD/MM/YYYY.
4. Apply the following calculations: [describe calculations, e.g., sum by category, running total, YoY %].
5. Write results to a new sheet named "Summary_[PERIOD]".
6. Format the summary sheet: bold headers, number format with commas, alternating row colors (light blue / white), auto-fit columns.
7. Save the workbook without prompts.

The macro must run fully offline — no external connections, no web calls.
Add error handling: if a sheet is missing, show a clear message box and stop gracefully.
Avoid using Select or Activate — use direct object references throughout.
```

### 1b. VBA Macro — Batch File Processor
```text
Write a VBA macro that processes all Excel files in a specified folder.

Requirements:
1. Ask the user to select a folder using a folder browser dialog.
2. Loop through every .xlsx and .xlsm file in that folder.
3. For each file:
   - Open it silently (no screen flicker, no alerts).
   - Read data from the sheet named [SHEET_NAME].
   - Apply transformation: [describe what to do].
   - Append the result to a master workbook (create it if it doesn't exist).
   - Close the source file without saving.
4. After processing all files, save the master workbook as "Master_[PERIOD].xlsx" in the same folder.
5. Show a completion message with the count of files processed and any that failed.

Include full error handling per file so one bad file doesn't stop the batch.
No external connections required — must run entirely within Excel.
```

### 1c. VBA Macro — Dynamic Pivot Table Builder
```text
Write a VBA macro that creates a pivot table from scratch.

Source: sheet [SHEET_NAME] in the active workbook.
Pivot table destination: a new sheet named "Pivot_[PERIOD]".

Pivot configuration:
- Row fields: [list row fields]
- Column fields: [list column fields]
- Values: [list value fields and aggregation]
- Filters: [list filter fields if any]

After creating the pivot:
- Apply "Report Layout" = Tabular Form.
- Apply number format with 2 decimal places and thousand separator.
- Auto-fit all columns.
- Add a title in cell A1 above the pivot: "[REPORT_NAME] — [PERIOD]".

Refresh the pivot before the macro ends.
```

---

## 2. EXCEL FORMULAS & DATA ANALYSIS

### 2a. Formula Audit & Fix
```text
Audit the Excel file at [FILE_PATH], sheet [SHEET_NAME], for formula problems.

Check for:
1. Broken references (#REF!, #NAME?, #VALUE!, #DIV/0!, #N/A).
2. Circular references.
3. Hard-coded numbers inside formulas that should reference cells instead.
4. Volatile functions (NOW, TODAY, RAND, INDIRECT, OFFSET) used unnecessarily.
5. Array formulas or SUMPRODUCT used inefficiently.
6. VLOOKUP that should be replaced with INDEX/MATCH or XLOOKUP.
7. Inconsistent formulas in a column.

Output: list each issue with its cell address, the current formula, what's wrong, and the corrected formula.
Fix all issues directly in the file and save.
```

### 2b. Data Cleaning & Standardization
```text
Clean the data in Excel file [FILE_PATH], sheet [SHEET_NAME].

This data was exported from [DATA_SOURCE]. Apply:
1. Remove completely blank rows and columns.
2. Remove duplicate rows (log which rows were removed).
3. Trim leading/trailing spaces from all text cells.
4. Standardize date columns to DD/MM/YYYY format.
5. Standardize number columns: remove currency symbols, commas, spaces so they are stored as true numbers.
6. Flag cells that contain text in a column that should be numeric — highlight in yellow.
7. Standardize text case in [COLUMN_NAME] column: Title Case.
8. Remove special characters from ID or reference columns.

Save a cleaned copy as [FILE_PATH]_cleaned.xlsx.
Write a summary log to a new sheet "Cleaning_Log": column, issue type, count of rows affected.
```

### 2c. Financial KPI Dashboard Builder
```text
Build a KPI dashboard in Excel from the data in [FILE_PATH], sheet [SHEET_NAME].

This is for [CLIENT_NAME] — a [CLIENT_TYPE]. The dashboard will be delivered as a standalone Excel file.

KPIs to calculate:
- Total Revenue ([PERIOD])
- Total Expenses ([PERIOD])
- Net Profit and Net Profit Margin %
- Month-over-Month Revenue Change %
- Top 5 items/clients by revenue
- Expense breakdown by category (as a donut chart)
- Revenue trend over last 12 months (as a line chart)

Dashboard layout:
- New sheet named "Dashboard".
- Large KPI cards at the top (bold numbers, labeled clearly).
- Charts below KPI cards.
- Color palette: navy blue (#1F3864) headers, light gray (#F2F2F2) backgrounds.
- No gridlines on dashboard sheet.
- All charts: title, clean formatting, no border, no legend clutter.
- No external data connections — all data sourced from this workbook only.

Data must link dynamically to [SHEET_NAME] — no hardcoded values in the dashboard.
```

---

## 3. OFFICE AUTOMATION (Word / PowerPoint / Outlook)

### 3a. Word — Client Report Template
```text
Create a Word document report for [CLIENT_NAME] — [REPORT_NAME] — [PERIOD].

Structure:
1. Cover page: [FIRM_NAME] logo placeholder, report title, client name, period, analyst [ANALYST_NAME], date.
2. Executive Summary (1 page): key findings as bullet points.
3. Financial Overview: tables with key metrics. Source data: [DATA_SOURCE].
4. Analysis Sections: [list sections, e.g., Risk Assessment, Portfolio Analysis, Compliance Status].
5. Recommendations: numbered list.
6. Appendix: supporting tables.

Formatting:
- Font: Calibri 11pt body, 14pt headings.
- Colors: navy headings, black body text.
- Page numbers in footer.
- Auto-generated table of contents on page 2.
- All financial tables: thousand separators, consistent number format.

This file will be delivered to [CLIENT_NAME] as a standalone Word document.
Save as [REPORT_NAME]_[CLIENT_NAME]_[PERIOD].docx.
```

### 3b. PowerPoint — Client Presentation Builder
```text
Create a PowerPoint presentation for [CLIENT_NAME] — [PERIOD].

Slides:
1. Title slide: [REPORT_NAME], [CLIENT_NAME], [PERIOD], [FIRM_NAME].
2. Agenda (5–7 items).
3. Executive Summary: 3 key takeaways.
4. Key Metrics: KPI boxes with values.
5. Trend Chart: bar chart for [PERIOD].
6. Breakdown: donut or pie chart by category.
7. Key Risks & Flags: 3–5 items with RAG status (Red/Amber/Green).
8. Recommendations: numbered list.
9. Next Steps & Contacts.

Design:
- Clean professional theme: navy and white.
- Font: Calibri.
- No clip art, no decorative borders.
- Each slide: header with title in navy.

This will be delivered to [CLIENT_NAME] as a standalone .pptx file.
Save as [REPORT_NAME]_[CLIENT_NAME]_[PERIOD].pptx.
```

### 3c. Outlook — Automated Internal Report Sender (VBA)
```text
Write a VBA macro that sends a formatted email via Outlook for internal distribution.

The macro should:
1. Compose an email:
   - To: [internal recipient(s)]
   - CC: [ANALYST_NAME]
   - Subject: "[REPORT_NAME] — [CLIENT_NAME] — [PERIOD]"
   - Body: professional HTML email with greeting, 3–4 KPI bullet points, closing line.
2. Attach the file: [FILE_PATH].
3. Display the email for review before sending — do NOT auto-send.

Pull KPI values from the active workbook, sheet [SHEET_NAME], cells [specify].
If the attachment file doesn't exist, show an error and cancel.

Note: This is for internal distribution only. Client-facing emails are sent manually after review.
```

---

## 4. DATA IMPORT & TRANSFORMATION

### 4a. Power Query — Export File Connector
```text
Build a Power Query solution in Excel to process exported data from [DATA_SOURCE].

Context: [DATA_SOURCE] exports files to a folder on our local machine. We cannot connect directly to the source system — we work only with the exported files.

Steps:
1. Connect to the folder containing exports (CSV or Excel files).
2. Import all files for [PERIOD].
3. Apply transformations:
   - Filter rows where [COLUMN_NAME] meets condition [describe].
   - Rename columns to the standard naming convention.
   - Split or merge columns as needed.
   - Convert data types: dates, numbers, text.
   - Remove irrelevant columns.
4. Load cleaned data to a sheet named "Raw_[PERIOD]" as a Table.
5. Configure refresh to work from the local folder with one click.

Document each transformation step with a clear name in the Power Query editor.
The query must not require any external network connection.
```

### 4b. Multi-Source Consolidation
```text
Consolidate data from multiple Excel files into a single master file.

Source files: all .xlsx files in folder [FILE_PATH] matching [describe pattern].
Each file has a sheet named [SHEET_NAME] with columns: [list columns].

Consolidation rules:
1. Stack all data vertically from all files.
2. Add a column "Source_File" with the original filename.
3. Add a column "Import_Date" with today's date.
4. Deduplicate on [COLUMN_NAME] — keep the most recent record.
5. Sort by [COLUMN_NAME] ascending.

Output: save as "Consolidated_[PERIOD].xlsx" with data in sheet "Master".
Include a "Summary" sheet: file name, row count, import status per file.
```

---

## 5. FINANCIAL REPORTING

### 5a. Budget vs Actual Analysis
```text
Perform a Budget vs Actual analysis using data in [FILE_PATH].

Inputs:
- Budget: sheet "Budget" — columns: Category, Sub-category, Budgeted_Amount.
- Actuals: sheet "Actuals" — columns: Date, Category, Sub-category, Amount.

Analysis:
1. Sum actuals by Category and Sub-category for [PERIOD].
2. Join to budget on Category + Sub-category.
3. Calculate: Variance (Actual - Budget) and Variance % ((Actual - Budget) / Budget * 100).
4. Flag rows where Variance % exceeds +/- 10%.
5. Create summary sheet "BvA_[PERIOD]" with formatted table.
6. Add bar chart: Budget vs Actual by Category.
7. Conditional formatting: green if under budget, red if over budget by >10%.

Protect the output sheet from accidental editing (allow read-only).
```

### 5b. Reconciliation Checker
```text
Perform a reconciliation between two data sets in [FILE_PATH].

Set A: sheet "[SHEET_NAME]_A" — represents [describe, e.g., core banking export].
Set B: sheet "[SHEET_NAME]_B" — represents [describe, e.g., accounting ledger].
Match key: [COLUMN_NAME].

Steps:
1. Match Set A records to Set B on the match key.
2. For matched records: compare [COLUMN_NAME] values. Flag differences.
3. Find records in A not in B.
4. Find records in B not in A.
5. Output a "Reconciliation" sheet:
   - Matched & agreed
   - Matched but disagreed (difference in red)
   - Unmatched from A (orange)
   - Unmatched from B (yellow)
6. Summary row: total records, matched, unmatched, total difference.

Save and protect the reconciliation sheet.
```

### 5c. Month-End Close Checklist
```text
Create a month-end close checklist workbook for [CLIENT_NAME] — delivered as a standalone Excel file.

The workbook:
1. "Checklist" sheet:
   - All close tasks as rows: [list tasks, e.g., Bank Recon, AP Aging, AR Aging, Accruals, Trial Balance, Management Report]
   - Columns: Task, Owner, Due Date, Status (dropdown: Not Started / In Progress / Complete / Blocked), Notes
   - Conditional formatting: row turns green on Complete, red on Blocked
   - Progress bar at top showing % complete

2. "Dashboard" sheet:
   - Count by status
   - Days remaining until close deadline
   - Open tasks by Owner

3. "History" sheet: logs each status change with timestamp, task, changer.

Include a "Send Status Update" button macro that emails the checklist summary via Outlook to [ANALYST_NAME] — display only, never auto-send.
```

---

## 6. QUALITY CONTROL & AUDIT

### 6a. Spreadsheet Audit
```text
Audit the Excel workbook at [FILE_PATH] before delivery to [CLIENT_NAME].

Check:
1. STRUCTURE: sheets named clearly, logical flow.
2. FORMULAS: all formulas correct and consistent, no errors.
3. HARD-CODED VALUES: nothing that should be formula-driven is hardcoded.
4. DATA INTEGRITY: no duplicate entries, no missing required fields, no invalid values.
5. LINKS: no broken external links. No links pointing to our internal machines (client must open standalone).
6. NAMED RANGES: all pointing to correct cells.
7. MACROS: if VBA exists — clean, commented, safe. No external calls, no web requests.
8. PROTECTION: correct cells locked.
9. SIZE & PERFORMANCE: no runaway data ranges, reasonable file size.
10. STANDALONE TEST: can the file be opened on another machine without missing references?

Output: audit report with severity (Critical / Warning / Info), cell location, recommended fix.
Fix all Critical issues before marking the file ready for delivery.
```

---

## 7. SOLUTION PROPOSALS FOR CLIENTS

### 7a. Automation Opportunity Assessment
```text
Review the process described below and identify where automation with Excel, VBA, or Power Query can add value for [CLIENT_NAME] — a [CLIENT_TYPE].

Process description: [describe the client's current manual process]

Assess:
1. Which steps are repetitive and can be fully automated?
2. Which steps require human judgment and should stay manual?
3. What data inputs does the process consume? Can they be loaded automatically?
4. What outputs does it produce? Can they be formatted and distributed automatically?
5. What is the rough time saving estimate (hours per month)?
6. What is the implementation complexity (Low / Medium / High)?

Output: a structured opportunity assessment with a recommended automation roadmap.
Format as a clean Word document that can be shared with the client.
Constraint: all proposed automations must run within the client's existing Office environment — no new software installations required.
```

### 7b. Solution Package Builder
```text
Build a complete solution package for [CLIENT_NAME] based on the automation opportunity identified.

The package must be fully self-contained — the client opens it in Excel/Word and it works without any external tools, cloud services, or additional installations.

Package contents:
1. Main Excel workbook with:
   - Instructions sheet (clear, non-technical)
   - Data input sheet with validation
   - Automated processing (VBA macro or Power Query)
   - Output / report sheet
2. User guide (Word document, max 2 pages): what it does, how to use it, what to do if something goes wrong.
3. Macro setup instructions (if VBA): how to enable macros, one-time setup steps.

Standards:
- All input cells labeled and highlighted in yellow
- All formula cells protected
- Macro includes error handling and user-friendly messages
- No hard-coded paths, names, or dates — everything configurable
- Tested on a machine that has never seen this file before

Deliver as a zip file: [CLIENT_NAME]_[SOLUTION_NAME]_v1.0.zip
```

---

## 8. TEMPLATES & STANDARDIZATION

### 8a. Standard Template Generator
```text
Create a standard Excel template for [REPORT_NAME] to be used by all analysts at [FIRM_NAME].

The template:
1. "Instructions" sheet: how to use the template.
2. "Config" sheet: Client Name, Period, Analyst Name, Report Date — these feed all other sheets.
3. "Input" sheet: raw data entry with headers, data validation, protected formulas.
4. "Output" sheet: auto-calculates and formats the report from Input data.
5. "Cover" sheet: auto-populates from Config.

Standards:
- All formula cells locked and protected.
- Input cells highlighted in light yellow.
- Consistent fonts (Calibri), colors (navy / white / light gray), number formats throughout.
- Version number in Config sheet cell A1.
- "Reset" macro button: clears Input data only, not formulas.

Save as [REPORT_NAME]_TEMPLATE_v1.0.xlsx.
```

---

## How To Use This Library

**For Claude Code sessions:** Copy the relevant prompt, fill in the placeholders, paste into Claude Code, and let it execute.

**For internal work:** Use sections 1–6 to speed up analyst and delivery work.

**For client proposals:** Use sections 7–8 to generate solution packages that run inside the client's Office environment.

**The delivery rule:** Everything Claude Code produces must be a standalone file that works without Claude Code on the receiving end.

**Cadence recommendation:**

| Prompt | Frequency | Use |
|--------|-----------|-----|
| 1a VBA Report Generator | Per report cycle | Internal / Client |
| 1b Batch Processor | As needed | Internal |
| 2a Formula Audit | Before every delivery | QA |
| 2b Data Cleaning | Every new data import | Internal |
| 2c Dashboard Builder | Monthly | Internal / Client |
| 3a Word Report | Per client delivery | Client |
| 3b PowerPoint | Per presentation | Client |
| 4a Power Query Connector | New data source setup | Internal |
| 5a Budget vs Actual | Monthly | Internal / Client |
| 5b Reconciliation | Monthly | Internal / Client |
| 5c Month-End Checklist | Monthly | Client |
| 6a Spreadsheet Audit | Before every client delivery | QA |
| 7a Automation Assessment | New client engagement | Business Dev |
| 7b Solution Package | Per new client solution | Delivery |
