# Paisa — Personal Expense Tracker

A mobile-friendly, installable website for INR expenses, cash/bank balances and udhari. No build tools, paid APIs or external JavaScript dependencies are required.

## Host on GitHub Pages

1. Create a GitHub repository, for example `paisa-expenses`.
2. Upload every file from this folder into the repository root. `index.html` must be at the root, not inside another folder.
3. Open the repository's **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Choose **main**, then **/(root)**, and save.
6. After deployment completes, open the website link shown by GitHub Pages. A project URL normally looks like `https://YOUR-USERNAME.github.io/paisa-expenses/`.
7. Open this URL on your phone. Use the browser's **Install app** or **Add to Home Screen** option. On iPhone, use Safari's Share menu → Add to Home Screen.

Reference: https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site

The website has not been uploaded to your GitHub account for you. This package contains the complete code to upload.

## Start using it

1. In **Balance & udhari**, add one opening balance for Bank and one for Cash. Use the money you have when you start tracking.
2. Add past loans using **Existing debt** or **Existing loan to someone**. These do not change current available money.
3. Record new expenses in **Overview**. Choose the category, account and Useful/Unnecessary label yourself.
4. Use **Income / add money** for salary or other newly received money.
5. For new loans, choose **Borrowed money** or **Lent money**. These affect available money.
6. Use **I repaid udhari** or **Someone repaid me** to reduce the person's outstanding amount. Reuse the same name. Names ignore case and leading/trailing spaces. Repayments cannot exceed the recorded outstanding amount.
7. Moving money between Bank and Cash uses **Transfer**. It does not affect total available money.
8. Correct expenses with Edit. Other entries can be deleted and re-entered. Deleting loan entries is blocked if it would leave repayments larger than the recorded loan.

Available money is the balance of cash and bank entries. It can become negative if expenses or outflows exceed recorded funds; check for missing opening balances or income. The app does not connect to your bank. Opening balance should not be added again as salary. Do not backfill expenses already subtracted from your opening balance.

Net position = available money + invested capital + loans to receive − debts owed. Loans, repayments and transfers are not counted as spending. The top spending total is for the current calendar month. The month selector changes the history and spending breakdown. Money is calculated in integer paise to avoid common rounding errors.

## Daily reminder and phone alarm

In **Reminders & backup**, choose a time such as 21:00.

- **Page reminder** displays a message only while the website is open and active. It does not ring, wake the device or reliably run in the background.
- **Add daily calendar reminder** downloads an `.ics` calendar event repeating every day at the selected local time. Import it into your calendar and check its notification settings. Calendar support, notification delivery and sound depend on your phone and app. If your mobile calendar cannot import the file directly, import it through your calendar's web interface. Changing the app's time does not update an already imported event. Delete or edit old calendar events to avoid duplicate reminders.
- For a dependable ringing reminder, set a repeating daily alarm in your phone's Clock app, with the label **Fill Paisa expense tracker**. This website cannot set a native Clock alarm automatically.

A closed-page push notification service would need a separate backend and push subscriptions. GitHub Pages alone is static hosting. No notification permission is requested by this version because it does not provide background push.

## Excel and backups

**Export Excel** downloads a real `.xlsx` workbook with four sheets:

- **Summary:** available money, bank/cash balances, udhari and current month's spending.
- **Entries:** every saved entry with date, type, numeric INR amount, account, category, purpose, person, note and ID.
- **Investments:** recorded capital for Share market, Gold, SIP and Bhisi.
- **Udhari:** amount owed and receivable for each person.

The file is a snapshot at export time. The app does not write automatically to a cloud Excel file or an existing local workbook. Use the built-in overview and month selector to view your records, and export whenever you want an Excel copy. User-entered text is exported as literal text, never executable formulas. Excel files cannot be imported into this version.

Use **Download full backup** regularly. The JSON backup contains all entries and reminder settings, and can be restored on another device with **Restore a backup**. Restore replaces existing entries after confirmation. Download a current backup before restoring. Backups and exports contain personal data: keep them private and do not upload them into your public repository.

## Storage and privacy

Entries are automatically saved to localStorage in the current browser for the website's origin. No server receives expense records. There is no login or cross-device sync. Anyone with access to that browser profile can access the data. Clearing site data, private browsing sessions, browser storage eviction, changing the hosting domain or switching browsers can make the records unavailable. Backups are essential. A service worker caches application files for offline use after the first successful online visit.

Different GitHub Pages projects under the same `username.github.io` hostname share an origin. Use a trusted account and avoid hosting untrusted scripts on that origin. The tracker is meant for one personal dataset per browser origin.

## Run locally

From this folder, run:

```sh
python -m http.server 8000
```

Open `http://localhost:8000`. Do not rely on double-clicking the HTML file: service workers and installation need HTTPS or localhost.

## Files

- `index.html`: screen layout and forms
- `style.css`: mobile and desktop styling
- `app.js`: ledger, calculations, backup, reminder and UI logic
- `excel.js`: dependency-free XLSX export
- `sw.js`: offline cache
- `manifest.webmanifest`: installable app metadata
- `icon.svg`, `icon-192.png`, `icon-512.png`: app icons
- `.nojekyll`: tells GitHub Pages to serve the files directly

When releasing updates, increment the cache version in `sw.js`. Close and reopen installed app windows after deployment so the new service worker can activate. New versions should preserve the `paisa-v1` data key or explicitly migrate it.

## Savings & investments update

The Savings & investments tab separately shows cash available, bank/online money available, invested capital and their combined total. The combined total is before udhari. The net position in Balance & udhari includes outstanding loans and debts.

Record Share market, Gold, SIP and Bhisi contributions here. A **New investment** reduces the selected Bank or Cash balance and increases invested capital by the same amount. It is not an expense. An **Existing investment** adds previously invested capital without reducing the available balance again. Investment amounts represent recorded contributions, not live market valuations. This version does not track redemptions, investment income, profit/loss or bhisi payouts. SIP and bhisi contributions are manually entered for each payment. Correct mistakes by deleting the original entry in Overview and entering it again.

A red warning at the top of Overview lists every person whose total outstanding debt **to you** exceeds ₹7,000. Separate loans to the same person are combined; names are case-insensitive. Exactly ₹7,000 does not trigger a warning. Repayments reduce the amount and remove the warning when it drops to ₹7,000 or less. This warning is based on all outstanding entries, regardless of the selected history month.

### Update an existing installation

Download a JSON backup first. Replace the website files in the **same GitHub repository and URL**, including app.js, index.html, style.css, excel.js and sw.js. Close all open Paisa tabs and installed windows, then reopen the site online after deployment. Existing entries stay under the same `paisa-v1` storage key and old backups can still be restored. Do not clear browser/site data to update. New backups containing investments require this updated version.
