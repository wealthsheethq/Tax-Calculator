# Side Hustle Tax Estimator

A free, single-page calculator from WealthSheetHQ that estimates how much of your side hustle income to set aside for federal taxes (self-employment tax plus income tax) and what your quarterly estimated payments could be.

Everything is in one file, `index.html`, with inline CSS and JavaScript. There are no libraries or CDNs, and nothing a user types leaves the browser.

## Hosting on GitHub Pages

1. Go to the repository's **Settings → Pages**.
2. Under **Build and deployment**, choose **Deploy from a branch**, pick `main` and `/ (root)`, and click **Save**.
3. After a minute, the site is live at `https://<your-username>.github.io/<repo-name>/`.

## Setting the Etsy link

The "Get the Side Hustle Budget Planner" button links to https://www.etsy.com/listing/4499434187/side-hustle-budget-planner-income. To change it, search `index.html` for `etsy-link` and edit its `href`.

## Updating the tax figures each year

All tax numbers live in a single object called `TAX_CONFIG` near the top of the `<script>` block in `index.html`. Every value has a comment. No other part of the file needs to change.

Each year, usually after the IRS publishes its inflation-adjustment Revenue Procedure in the fall and the SSA announces the new wage base in October:

| Field in `TAX_CONFIG` | What to update | Source |
| --- | --- | --- |
| `taxYear` | The new tax year (shown on the page) | — |
| `socialSecurityWageBase` | Social Security contribution and benefit base | SSA COLA announcement |
| `standardDeduction.single` / `.mfj` / `.hoh` | Standard deduction by filing status | IRS Rev. Proc. (inflation adjustments) |
| `brackets.single` / `.mfj` / `.hoh` | Top of each bracket, as `[top, rate]` pairs. Keep `Infinity` on the last one | IRS Rev. Proc. |
| `estimatedPaymentDueDates` | The four Form 1040-ES due dates (the last one is in January of the next year). Move any that fall on a weekend or holiday | Form 1040-ES instructions |

These rarely change, but check them too:

| Field | Current value | Meaning |
| --- | --- | --- |
| `seNetEarningsFactor` | 0.9235 | Net earnings from self-employment = 92.35% of net profit |
| `seSocialSecurityRate` | 0.124 | Social Security portion of SE tax |
| `seMedicareRate` | 0.029 | Medicare portion of SE tax |
| `seMinimumNetEarnings` | 400 | No SE tax below this amount of net SE earnings |
| `seDeductionShare` | 0.5 | Share of SE tax that is deductible |
| `qbiRate` | 0.20 | Simplified QBI deduction rate |

After editing, open `index.html` in a browser, enter a scenario you've worked out by hand, and compare it with the "How we calculated it" table.

## What the calculator does (and doesn't) include

**Includes:** net SE profit, SE tax (Social Security portion capped at the wage base left after the side-hustle earner's own W-2 wages; for Married Filing Jointly a separate field asks for that spouse's wages, while total household wages still drive income tax), the deduction for half of SE tax, the standard deduction, a simplified 20% QBI deduction (limited to 20% of taxable income before QBI, with no phase-outs), federal income tax brackets, the extra tax caused by the side hustle (tax with it minus tax without it), and a balance due split across the estimated payment dates that haven't passed yet (past dates show "Past due"). The balance is total tax minus W-2 withholding; if W-2 wages are entered but withholding is left blank, it's the side hustle's extra tax only, on the assumption that withholding covers the tax on wages.

**Excludes:** state and local tax, credits, itemized deductions, other income, Additional Medicare Tax, QBI phase-outs and SSTB limits, and deductions for SE health insurance or retirement contributions.

When "Year to date" is selected, side hustle income and expenses are projected to a full year at the same daily pace, based on today's date.
