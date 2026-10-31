# Tableau Dashboard — FY2027 Headcount & OpEx

An interactive Tableau Public dashboard built on the output of the Excel [Headcount & OpEx Planning Model](README.md). It lets a business partner switch between the four hiring scenarios and see total spend against the $15.0M leadership ceiling, spend by department, monthly cost trend and headcount growth.

**Live dashboard:** _add your Tableau Public link here_

**Data:** [`brightwater_fy2027_opex.csv`](brightwater_fy2027_opex.csv). Exported from the Excel model by recalculating it under each scenario (4 scenarios × 6 departments × 12 months × 9 cost lines = 2,592 rows). Totals tie exactly to the model's Scenarios sheet.

| Column | Meaning |
|---|---|
| Scenario | Plan, Hiring freeze, Accelerated growth, Deferred hiring |
| Department | Engineering, Product, Sales, Marketing, Customer Success, G&A |
| Month | First day of each month, Jan–Dec 2027 |
| Line Item | Salaries, Bonus, Payroll taxes, Benefits, Recruiting, Software, Travel, Facilities, Programs & vendors |
| Cost Type | People or Non-people |
| Amount | Budgeted expense ($) |
| FTE | Average FTE for the department that month (filled on the Salaries row only, so it can be summed safely) |

---

## How to build it (about 1–2 hours)

You need a free [Tableau Public](https://public.tableau.com) account. Everything below works in the browser (Create → Web Authoring) or in the free Tableau Public desktop app.

### 1. Connect the data
1. Sign in to Tableau Public → click your profile → **Create a Viz**.
2. Upload `brightwater_fy2027_opex.csv`.
3. On the data screen, check that **Month** is a Date, **Amount** and **FTE** are numbers (Measures). If Month shows as text, click the "Abc" icon above it and change it to Date.

### 2. Create calculated fields
In any sheet: **Analysis → Create Calculated Field**. Make these four:

| Name | Formula |
|---|---|
| Ceiling | `15000000` |
| Average FTE | `SUM([FTE]) / COUNTD([Month])` |
| Over / (Under) Ceiling | `SUM([Amount]) - 15000000` |
| Status | `IF SUM([Amount]) > 15000000 THEN "Over ceiling" ELSE "Within ceiling" END` |

### 3. Build five sheets
Rename each sheet (double-click the tab) with the name in bold.

**Sheet 1 — "KPIs"**
1. Drag **Amount** to Text on the Marks card. Right-click it → Format → Currency (custom), millions, 2 decimals → shows $15.40M.
2. Drag **Over / (Under) Ceiling** to Text, format the same way.
3. Drag **Status** to Color.
4. Edit the text (click Text on the Marks card) so it reads:
   `Total FY2027 OpEx: <SUM(Amount)>` / `vs. $15.0M ceiling: <Over / (Under) Ceiling>`

**Sheet 2 — "Scenario Comparison"**
1. **Scenario** to Rows, **Amount** to Columns → horizontal bars.
2. Drag **Status** to Color (red = over, gray or blue = within).
3. Right-click the Amount axis → **Add Reference Line** → Constant = 15,000,000, label "Ceiling $15.0M".
4. Sort bars descending (sort icon on the toolbar).
5. Leave this sheet **unfiltered** so it always shows all four scenarios.

**Sheet 3 — "OpEx by Department"**
1. **Department** to Rows, **Amount** to Columns. Sort descending.
2. Drag **Cost Type** to Color → stacked People vs. Non-people.
3. Drag **Amount** to Label.

**Sheet 4 — "Monthly OpEx Trend"**
1. Drag **Month** to Columns. Click its dropdown → choose the second **Month** (the continuous one, green pill).
2. **Amount** to Rows. On the Marks card, change Automatic to **Bar** (or Area).
3. **Cost Type** (or **Department**) to Color.

**Sheet 5 — "Headcount"**
1. **Month** (continuous) to Columns, **FTE** to Rows, Marks = **Line**.
2. **Department** to Color.
3. Add a Title: "Average FTE by month — 60 → 71 under Plan".

### 4. Add the scenario filter
1. On Sheet 1, drag **Scenario** to the Filters shelf → select **Plan** → OK.
2. Right-click the Scenario filter → **Show Filter**. On the filter card's menu choose **Single Value (dropdown)** and untick "All".
3. Right-click the filter pill → **Apply to Worksheets → Selected Worksheets** → tick KPIs, OpEx by Department, Monthly OpEx Trend and Headcount (**not** Scenario Comparison).

### 5. Build the dashboard
1. Click the **New Dashboard** icon at the bottom. Size: **Fixed, 1200 × 800**.
2. Layout:
   - Top: a Text object with the title *"Brightwater Analytics — FY2027 Headcount & OpEx Plan"* and a one-line subtitle: *"Simulated company. Select a scenario to compare against the $15.0M ceiling."* Then the KPIs sheet and the Scenario dropdown.
   - Middle row: Scenario Comparison (left), OpEx by Department (right).
   - Bottom row: Monthly OpEx Trend (left), Headcount (right).
3. Make Scenario Comparison a filter: select it → click the funnel icon ("Use as filter"). Clicking a bar now updates the other charts too.
4. Add a small text box at the bottom: *"Recommendation: defer 8 priority-2 hires by one quarter and trim vendors 5% → $14.89M, $107K under the ceiling, keeping all 14 planned hires."*

### 6. Publish and link
1. **File → Save to Tableau Public**, title it *FY2027 Headcount & OpEx Plan*.
2. On your Tableau Public profile, open the viz → **Settings** (gear) → make sure "Show viz on profile" is on.
3. Copy the link and paste it at the top of this README where it says "add your Tableau Public link here".
4. Take a screenshot of the finished dashboard, save it as `dashboard.png` in this folder and add the line `![Dashboard](dashboard.png)` under the link.

### What to check before sharing
- Choosing **Plan** shows **$15.40M** and **+$0.40M** over the ceiling; **Deferred hiring** shows **$14.89M** and **($0.11M)**.
- Headcount under Plan goes from **60** in January to **71** in December.
- Scenario Comparison shows all four bars no matter which scenario is selected.
