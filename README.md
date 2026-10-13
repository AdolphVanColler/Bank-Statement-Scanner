# Bank Statement Scanner

Upload a bank statement PDF, check what was read, and export the transactions to Excel.

Everything runs in the browser. The PDF is never uploaded to a server.

## Features

- **Page view** – every page is rendered as an image, with the lines read as transactions highlighted. Tap a highlight to see exactly what was read from that line.
- **Transaction table** – date, description, amount and balance, all editable. Each row is checked against the running balance (previous balance + amount = printed balance), and rows that don't add up are flagged.
- **All text view** – every line of text found in the PDF, with a button to turn a missed line into a transaction.
- **Locked PDFs** – asks for the password (many banks lock emailed statements).
- **Export options**
  - One sheet, a sheet per month, or a sheet per PDF page
  - One signed Amount column, or separate Money in / Money out columns
  - Real Excel dates (`dd/mm/yyyy` or `yyyy-mm-dd`) or the date text as printed
  - Choose columns: date, description, amount, balance, fees, PDF page, balance check
  - Totals row with `SUM` formulas, a monthly summary sheet, and a sheet with all text from the PDF
  - `.xlsx` or `.csv`
  - Copy the table to paste straight into Excel

## How the reader works

1. Text and its position on the page are extracted with [PDF.js](https://mozilla.github.io/pdf.js/).
2. Text items are grouped into lines by their vertical position, and into columns by the gaps between them.
3. A line becomes a transaction when it starts with a date and contains at least one amount. Column titles such as *Debits*, *Credits*, *Balance* and *Amount* are used to tell the amounts apart.
4. Lines directly under a transaction with no date or amount are added to its description.
5. Money in vs money out is worked out from the balance change, then from `Cr` / `Dr` suffixes and minus signs.
6. Missing years are filled in from the statement period, including statements that run from December into January.

Supported date styles include `31/01/2026`, `2026-01-31`, `31 Jan 2026`, `31 Jan`, `Jan 31, 2026` and Afrikaans month names. Amounts can use `1,234.56`, `1 234.56`, `1.234,56`, `1,234.56-`, `(1,234.56)` and `1,234.56 Cr`.

Scanned statements (images with no text layer) can't be read by PDF.js. When the page is opened as a Claude artifact, an **Ask Claude to read this page** button can read those pages instead.

## Run it

It's a single file. Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Click **Load sample** to try it with a built-in sample statement.

## Built with

- [PDF.js](https://github.com/mozilla/pdf.js) 3.11.174 for rendering and text extraction
- [SheetJS](https://sheetjs.com/) 0.18.5 for writing `.xlsx` and `.csv`
