# 🇳🇵 Nepal Personal Income Tax Calculator — FY 2083/84

A production-ready, single-file HTML tax calculator for Nepal's progressive income tax system. No server, no dependencies, no install — open in any browser.

---

## Features

- **Two calculator modes** — Simple (direct taxable income) and Salary (full payroll breakdown)
- **Progressive slab engine** — correctly taxes each income portion at its applicable rate
- **Deductions support** — PF, CIT, insurance, retirement fund, and custom deductions
- **Monthly TDS calculation** — annual tax spread over configurable remaining months
- **Live preview** — gross, deductions, and taxable income update as you type
- **Slab breakdown table** — visual bars showing how income is distributed across slabs
- **Dark / light mode** — preference saved in localStorage
- **Print-ready** — clean print layout with tabs and buttons hidden
- **Share link** — encodes income in URL for easy sharing
- **Tax saving tips** — actionable suggestions to reduce liability
- **Progressive tax explainer** — plain-language breakdown with worked example
- **Nepali number formatting** — 10,00,000 style throughout
- **Config-driven slabs** — FY 2084/85 update requires changing only one object

---

## Tax Slabs — FY 2083/84

| # | Annual Taxable Income (Rs.) | Rate | Max Tax in Slab |
|---|----------------------------|------|-----------------|
| 1 | 0 – 10,00,000              | 1%   | Rs. 10,000      |
| 2 | 10,00,001 – 15,00,000      | 10%  | Rs. 50,000      |
| 3 | 15,00,001 – 25,00,000      | 20%  | Rs. 2,00,000    |
| 4 | 25,00,001 – 40,00,000      | 27%  | Rs. 4,05,000    |
| 5 | Above 40,00,000            | 29%  | No limit        |

---

## How Progressive Tax Works

Each slab rate applies **only to the income within that slab** — not the full income. Example:

```
Annual taxable income: Rs. 30,00,000

First Rs. 10,00,000  × 1%  =  Rs.   10,000
Next  Rs.  5,00,000  × 10% =  Rs.   50,000
Next  Rs. 10,00,000  × 20% =  Rs. 2,00,000
Next  Rs.  5,00,000  × 27% =  Rs. 1,35,000
                               ───────────
Total Tax                   =  Rs. 3,95,000
Effective Rate              =  13.17%
```

---

## Usage

1. Open `nepal-tax-calculator.html` in any modern browser
2. Choose a mode — **Simple** or **Salary**
3. Enter income details and deductions
4. Click **Calculate Tax** (or press Enter)

No installation, no internet connection required.

---

## Calculator Modes

### Simple Tax Calculator

Input your **annual taxable income** directly. The calculator applies progressive slabs and returns:

- Total tax payable
- Effective tax rate
- After-tax income
- Full slab breakdown with visual bars

### Salary Tax Calculator

Build up income from components:

| Input | Description |
|-------|-------------|
| Monthly Basic Salary | Base pay per month |
| Monthly Allowance | Taxable allowances per month |
| Months Worked | Number of months in the fiscal year |
| Bonus / Annual Incentive | One-time annual payments |
| Other Taxable Income | Any other income subject to tax |

**Deductions accepted:**

| Deduction | Notes |
|-----------|-------|
| Provident Fund (PF) | Employee contribution (annual) |
| Citizen Investment Trust (CIT) | Annual CIT contribution |
| Insurance Premium | Life insurance premium |
| Approved Retirement Fund | IRD-approved schemes |
| Other Allowed Deductions | Any other eligible deductions |

**Formula:**

```
Annual Gross    = (Basic + Allowance) × Months + Bonus + Other Income
Total Deductions = PF + CIT + Insurance + Retirement + Other
Taxable Income  = Annual Gross − Total Deductions
Annual Tax      = Progressive slab calculation on Taxable Income
Monthly TDS     = Annual Tax ÷ Remaining Salary Months
```

---

## Updating for FY 2084/85

All tax rules live in a single configuration object at the top of the `<script>` block. No calculation logic needs to change — only the config.

```js
const TAX_CONFIG = {
  fiscalYear: "2084/85",   // ← update year
  slabs: [
    { min: 0,        max: 1000000,  rate: 0.01, label: "0 – 10,00,000"         },
    { min: 1000000,  max: 1500000,  rate: 0.10, label: "10,00,001 – 15,00,000" },
    { min: 1500000,  max: 2500000,  rate: 0.20, label: "15,00,001 – 25,00,000" },
    { min: 2500000,  max: 4000000,  rate: 0.27, label: "25,00,001 – 40,00,000" },
    { min: 4000000,  max: Infinity, rate: 0.29, label: "Above 40,00,000"       },
    // Add or modify slabs here for the new fiscal year
  ]
};
```

---

## Code Architecture

```
nepal-tax-calculator.html
│
├── CSS Variables          — light/dark theming via :root and [data-theme="dark"]
│
├── TAX_CONFIG             — slab definitions (the only thing to update each year)
│
├── calculateTax(income)   — pure function, returns { totalTax, breakdown, effectiveRate }
│
├── fmtNPR(n)              — Nepali number format (10,00,000 style)
│
├── fmtRs(n)               — formats as "Rs. X"
│
├── buildBreakdownTable()  — renders slab breakdown table with visual bars
│
├── calcSimple()           — simple mode handler
│
├── calcSalary()           — salary mode handler with deductions logic
│
├── livePreview()          — updates gross/deductions/taxable as user types
│
└── init()                 — restores theme and URL param on load
```

---

## Edge Cases Tested

| Income | Total Tax | Effective Rate |
|--------|-----------|----------------|
| Rs. 0 | Rs. 0 | 0.00% |
| Rs. 10,00,000 | Rs. 10,000 | 1.00% |
| Rs. 10,00,001 | Rs. 10,000.10 | 1.00% |
| Rs. 15,00,000 | Rs. 60,000 | 4.00% |
| Rs. 15,00,001 | Rs. 60,000.20 | 4.00% |
| Rs. 25,00,000 | Rs. 2,60,000 | 10.40% |
| Rs. 40,00,000 | Rs. 6,65,000 | 16.63% |
| Rs. 30,00,000 | Rs. 3,95,000 | 13.17% |
| Rs. 1,00,00,000 | Rs. 24,05,000 | 24.05% |

---

## Browser Support

Works in all modern browsers — Chrome, Firefox, Safari, Edge. No polyfills required.

---

## Disclaimer

This calculator is for reference purposes only. Tax rules may change. Consult a certified tax professional or the Inland Revenue Department (IRD) Nepal for official advice.

**IRD Nepal:** [https://ird.gov.np](https://ird.gov.np)
