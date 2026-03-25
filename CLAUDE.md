# CLAUDE.md — Financial Services Firm: Excel & Office Workspace

This file defines how Claude Code should behave, what it should know, and how to work in this workspace.
Claude Code reads this file automatically at the start of every session.

---

## Who We Are

We are a **services firm** that builds products, tools, and solutions for financial institutions — banks, insurance companies, investment firms, pension funds, and capital markets organizations.

We do not work inside their environments. We build things for them.

Our staff are financial analysts, project managers, and domain experts — not primarily software developers. Most deliverables are Excel-based or Office-based tools that our clients can use immediately in their own environment.

---

## Two Modes of Work

### Internal Mode
Claude Code is used to make our own work faster, better, and more consistent:
- Automating repetitive analyst tasks (reconciliations, data cleaning, report generation)
- Building and refining Excel tools, VBA macros, and Power Query scripts
- Auditing and QA-ing deliverables before they leave the firm
- Maintaining internal templates and standards

### External / Client Mode
Claude Code is used to build better products and solutions for clients:
- Generating automation tools tailored to a specific client's data and process
- Producing analysis, dashboards, and reports from client-exported data
- Building complete solution packages (Excel + documentation) that we hand over
- Creating more compelling and detailed client proposals

---

## The Security Constraint

Our financial institution clients operate under strict IT and CISO governance.
Claude Code **cannot run in, connect to, or touch any client environment** — directly or indirectly.

This is a hard constraint. Not a preference.

### How We Work Around It

**The artifact model:** Claude Code runs on our machines. Everything it produces is a self-contained file. The client receives a standalone artifact — they never need to know how it was built.

| We do (our environment) | Client receives |
|-------------------------|-----------------|
| Write and test VBA macro | .bas file or .xlsm to import |
| Build Excel model | .xlsx that opens and works standalone |
| Generate Power Query M script | Script text to paste into their Power Query editor |
| Write a specification | Word/PDF document for their IT team to implement |
| Analyze exported data | Report, dashboard, or analysis as a file |

### Rules for Client Work
- Never connect Claude Code to live client systems, APIs, or databases
- Only use data the client has explicitly exported and shared with us (as a file)
- Anonymize or sanitize any sensitive client data before passing it to Claude Code as context
- All deliverables must work in a standard Office installation with no extra software
- All VBA macros must be fully offline — no web requests, no external calls, no COM dependencies beyond Office
- No hardcoded paths that reference our internal machines in any deliverable
- Always test: can this file open and run correctly on a machine that has never seen it before?

---

## Communication Style

- Speak in financial domain language, not developer language. Say "running total" not "cumulative sum variable."
- Before writing any macro or complex formula, explain in one plain-language paragraph what it does.
- Include inline comments in all VBA and Power Query M code explaining every meaningful step.
- When something could go wrong, say so explicitly before executing.
- If a client data file is opened and contains sensitive-looking information, flag it and ask how to proceed.

---

## Code Standards — VBA

- Never use `.Select` or `.Activate` — always use direct object references.
- Always include error handling (`On Error GoTo` with cleanup label).
- Use `Application.ScreenUpdating = False` and `Application.DisplayAlerts = False` at start of any looping macro.
- Always restore these settings in the error handler and at the end.
- Comment block at the top of every macro:
  ```vba
  ' Macro: [Name]
  ' Purpose: [What it does — one sentence]
  ' Author: Claude Code / [ANALYST_NAME]
  ' Date: [Date]
  ' Delivery: Standalone — no external connections
  ```
- Use `Long` instead of `Integer` for row/count variables.
- Use meaningful variable names.
- No `Shell()` commands, no `CreateObject("WScript.Shell")`, no internet calls in any client-facing macro.

---

## Code Standards — Excel Formulas

- Prefer `XLOOKUP` over `VLOOKUP`. Use `INDEX/MATCH` only if XLOOKUP is unavailable.
- Prefer `SUMIFS` over `SUMIF`.
- Avoid volatile functions (`INDIRECT`, `OFFSET`, `NOW`, `TODAY`) unless genuinely required.
- Always handle division-by-zero with `IFERROR`.
- Test against edge cases: empty cells, zero denominators, missing matches.

---

## Code Standards — Power Query (M)

- Every step must have a descriptive name.
- Add a comment at the top of every query explaining its source and purpose.
- Never load uncleaned data directly to a sheet — always include type conversion, blank removal, and column renaming.
- Source must be a local file or folder — never a live URL, API, or network database path in client-facing queries.

---

## File Handling

- Never modify source/raw data files. Always work on copies.
- Naming convention: `[ClientName]_[ReportType]_[YYYY-MM].[ext]`
- When processing a folder of files, preview the file list before processing.
- Log all batch operations (file name, rows processed, status) to a summary sheet.
- Standalone test: before declaring any deliverable ready, confirm it can open correctly on a machine that doesn't have the source data or any of our internal paths.

---

## Workspace Structure

```
/clients/
  [ClientName]/
    /data/          ← client-exported files (read, never modify)
    /working/       ← our working copies
    /output/        ← final deliverables ready to send
    /proposals/     ← solution proposals and assessments
/internal/
  /templates/       ← firm-wide reusable templates
  /macros/          ← shared VBA modules (.bas files)
  /queries/         ← shared Power Query scripts
/docs/              ← internal process documentation
```

---

## Session Startup

At the start of any work session, confirm:
1. Which **mode**: Internal or Client?
2. Which **client** (if client mode)?
3. Which **file or files** will be worked on?
4. The **reporting period**.
5. Whether an existing template or macro can be reused before building from scratch.
6. For client work: confirm the output will be standalone and not reference any of our machines.

---

## Pre-Delivery Checklist

Before any file leaves as a client deliverable:
- [ ] No formula errors (#REF!, #VALUE!, #N/A) anywhere in the workbook
- [ ] No external links pointing to our machines or network paths
- [ ] All input cells clearly labeled and highlighted (yellow)
- [ ] All formula cells protected
- [ ] Number formats consistent throughout (thousand separators, 2dp for financials)
- [ ] Macros tested: run correctly with macros enabled prompt, handle errors gracefully
- [ ] File opens standalone on a fresh machine without any missing references
- [ ] File size is reasonable (no runaway empty data ranges)
- [ ] Named as per convention: `[ClientName]_[ReportType]_[YYYY-MM].[ext]`
- [ ] Output matches known control totals or cross-checks

---

## Prompt Library

The file `core.md` in this folder contains ready-to-use prompts organized by task type.
Copy the relevant prompt, fill in the `[PLACEHOLDERS]`, and paste here to execute.
