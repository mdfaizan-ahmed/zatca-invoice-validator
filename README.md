# zatca-invoice-validator

# ZATCA Invoice Validator

Automated validation of Saudi ZATCA e-invoices using fuzzy matching.

## Problem

ZATCA compliance requires verifying supplier names and VAT numbers on every invoice. But:
- Supplier names have typos: "Saudi Telecom Company" vs "STC Solutions"
- VAT numbers have inconsistent formatting: spaces, dashes
- Manual review of 500 invoices takes hours

## Solution

This tool automates validation using:
- Fuzzy matching for supplier names (handles typos, abbreviations)
- Exact matching for VAT numbers (security requirement)
- Date validation (ZATCA 90-day reporting window)
- Format validation (15 digits, starts with 3)

## How It Works

1. Load your master vendor list (Excel/CSV)
2. Load your invoices (Excel/CSV)
3. Run validation
4. Export results with PASS/WARNING/FAIL status

## Sample Output

- PASS - Valid invoice
- WARNING - Company name matches but VAT number mismatch
- WARNING - VAT matches but company name unrecognized (manual review)
- FAIL - Neither name nor VAT matches master records

## Results from 20 test invoices

- PASS: 14
- WARNING: 6
- FAIL: 0

## Validation Logic

| Name Match (80%+) | VAT Exact Match | Result |
|-------------------|-----------------|--------|
| Yes | Yes | PASS |
| Yes | No | WARNING |
| No | Yes | WARNING (manual review) |
| No | No | FAIL |

## Production Use

```python
# Load your data
master_df = pd.read_excel("master_vendors.xlsx")
invoices = pd.read_excel("invoices.xlsx")

# Validate
results = validate_invoices(invoices, master_names, master_vats)

# Export
results.to_excel("validation_results.xlsx", index=False)

Tech Stack
Python 3

pandas

fuzzywuzzy

python-Levenshtein
