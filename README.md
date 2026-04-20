# AI-Powered Invoice Automation Engine (n8n + Google Sheets + Moneybird)

A robust automation system designed to handle complex, multi-line professional invoicing. This system eliminates manual data entry by synchronizing Google Sheets (acting as a CRM/Database) with the Moneybird API, supporting both one-off custom invoices and smart recurring billing previews.

## 🚀 Key Features

* **Multi-Line Splitting:** Automatically transforms comma-separated values in Google Sheets into separate, professional line items on a single invoice.
* **Dynamic Rate Lookup:** Fetches client-specific rates from a centralized database based on service categories (SEO, AI, Ads, etc.).
* **Smart Period Calculation:** Automatically calculates and appends the "Period" (e.g., *Period: March 2026*) to descriptions based on the previous month.
* **Recurrence Assistant:** A "Pre-fill" logic that fetches data from a client's last invoice to populate the Google Sheet for human review before finalization.
* **VAT/Tax Automation:** Integrates with tax-rate logic to apply the correct VAT rules (e.g., EU Reverse Charge) per invoice.

## 🛠 Tech Stack

* **Automation:** n8n (Agent-first workflows)
* **Data Source:** Google Sheets (Main Transaction Log & Client Rate Database)
* **Accounting:** Moneybird (API v2)
* **Logic:** JavaScript (n8n Code Nodes)

## 📋 System Architecture

The system operates through two distinct workflows:

### 1. The Execution Workflow (Main)
* **Trigger:** Google Sheets status set to `Ready for Invoice`.
* **Logic:** 1. Merges transaction data with client-specific rates.
    2. Splits `Service_Category`, `Hours_Breakdown`, and `Description_Breakdown` strings.
    3. Constructs a JSON array for the Moneybird API.
* **Output:** Creates a draft invoice in Moneybird and updates the Sheet with the `Invoice_URL`.

### 2. The Recurrence Assistant (Optional)
* **Trigger:** Status set to `Pull Recurring`.
* **Logic:** Fetches the most recent invoice for that client from Moneybird and "reverse-engineers" the data back into the Google Sheet.
* **Goal:** Allows for a "Human-in-the-loop" review/tweak of hours before the invoice is generated.

## 🗄 Google Sheets Schema

### Main Sheet Headers
| Column | Description |
| :--- | :--- |
| `Company_name` | Matches the Client Database. |
| `Is_Recurring` | Toggle (Yes/No). |
| `Service_Category` | Comma-separated (e.g., Omzet SEO, Omzet AI). |
| `Hours_Breakdown` | Matching comma-separated hours (e.g., 10, 5). |
| `Status` | Controls workflow trigger (Ready for Invoice, Invoiced, etc). |

### Client Database Headers
| Column | Description |
| :--- | :--- |
| `Company_name` | Primary Key. |
| `Omzet SEO Rate` | Hourly rate for SEO services. |
| `Omzet AI Rate` | Hourly rate for AI automation. |

## ⚙️ Configuration

1. **n8n Setup:** Import the provided JSON workflow files.
2. **API Credentials:** Add your Moneybird API token and Google Sheets OAuth2 credentials.
3. **Ledger Mapping:** Update the `categoryToLedger` object in the Code nodes to match your specific Moneybird ledger account IDs.

## 📝 License
MIT License - Feel free to use and modify for your own automation needs.
