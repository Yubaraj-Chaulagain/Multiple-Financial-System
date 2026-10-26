# Yubaraj Finance & Stock — Multi-Branch Phase 2

## What is included
- Admin-only Branch Management: create a branch with its own Google Spreadsheet; disable, re-enable, or archive a branch. Archive preserves financial history.
- Admin Panel user-to-branch assignment for approved users.
- Branch-aware business data: assigned Manager / Accountant / Staff users read and write records in their assigned branch spreadsheet rather than copying all transactions into the Master spreadsheet.
- Main Master Dashboard reads branch spreadsheet summaries without copying all transaction rows into the Master spreadsheet.
- Inter-branch transfer endpoint and form: permitted Admin / Manager / Accountant can transfer savings from a member in the source branch to a member in another branch; source balance is checked and a withdrawal/deposit ledger entry is created.
- Members are blocked from directly editing financial records through the record mutation endpoint; member access to data is scoped to their own records.

## Important architecture notes
- The Master spreadsheet keeps the branch registry, user accounts, and configuration. Each branch spreadsheet stores that branch's Members, Products, Transactions, Savings, Loans, LoanPayments, Transfers, and related logs.
- Main Dashboard summarizes the branch spreadsheets at request time; it does not copy every branch transaction into the Master spreadsheet.
- Branch assignment is required for non-admin users to load branch business data. After Admin assigns a branch, the user should log out and log back in.
- Branch Delete / Archive is deliberately non-destructive: the branch is archived so historical financial records remain available.
- Inter-branch transfer currently uses sender/receiver Member IDs. The source member must have enough recorded Savings ledger balance. It does not include the `Members.opening` field as available balance.
- This is not audited accounting software. Test with sample data and take backups before live use. Cross-spreadsheet Google Sheets writes cannot provide the same transaction guarantees as a dedicated database; reconcile transfers and keep audit records. For very large datasets, use a database backend instead of relying on Sheets quotas and execution time.

## Deploy Apps Script updates
1. Open the Apps Script project currently used by the app.
2. Replace the project code with the supplied `Code.gs` from this ZIP (or copy the full file into the existing `Code.gs`).
3. Save, then **Deploy → Manage deployments → Edit** the existing Web App deployment and select **New version**. Keep the existing deployment if you want the URL already embedded in `www/index.html` to continue working.
4. Confirm execution as the script owner and authorize Spreadsheet access. The first time the script creates a branch spreadsheet, Google may ask for additional authorization.
5. Open the app and log in as Admin. Create branches from **Branch Management**.
6. Open **Approval Panel → Users & Branch Assignment** and assign each approved Manager, Accountant, Staff, User, or Member to the correct active branch. Have them log out and log back in.
7. Test branch data and inter-branch transfer using test members and small sample amounts before using real financial data.

## Build APK
1. Upload all files and folders to GitHub (keep the hidden `.github/workflows` folder).
2. Open Actions and run **Build Yubaraj Finance & Stock APK**.
3. Download the workflow artifact and install the APK on Android.

## Credentials
Never share the Apps Script deployment URL with unauthorized people. Change the initial Admin password and follow secure password practices. Plain-text password storage is inherited from the earlier project and is not recommended for production.
