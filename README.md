# Financial Transaction Analytics System

A Python-based personal finance management system built with object-oriented programming principles. Tracks multi-account finances, processes real bank CSV exports, detects anomalies, and generates spending insights.

**Course:** INST326 — Object-Oriented Programming for Information Science  
**Institution:** University of Maryland, College Park  
**Semester:** Fall 2024  
**Team:** Uzzam Tariq · Keven Day · Kevin Miele · Angelo Montagnino

---

## The Problem

People struggle to understand where their money is going. Small recurring charges accumulate unnoticed, and bank statements are hard to analyze manually.

## Our Solution

A Python application that:
- Tracks transactions across Checking, Savings, and Credit accounts
- Imports real bank CSV exports and cleans messy data automatically
- Flags suspicious transactions and spending anomalies
- Generates categorized spending breakdowns and account summaries
- Enforces banking rules (overdraft limits, savings withdrawal caps, credit limits)

---

## Key Features

### Multi-Account Management
| Account Type | Key Rules |
|---|---|
| **Checking** | Overdraft protection, monthly fee if below minimum balance, check writing |
| **Savings** | Monthly interest earnings, minimum balance enforcement, 6 withdrawal/month limit |
| **Credit** | Credit limit enforcement, monthly APR interest on outstanding balance |

### Data Pipeline
- **Import:** Accepts CSV exports from any bank (handles `MM/DD/YYYY` and `YYYY-MM-DD` formats)
- **Clean:** Normalizes dates, standardizes categories, strips transaction codes, removes duplicates
- **Analyze:** Categorized spending totals, inflow/outflow breakdown, recent transaction history
- **Alert:** Flags large transactions, category overspending, and suspicious merchants

### OOP Design Patterns
- **Inheritance:** `Account` ABC → `CheckingAccount`, `SavingsAccount`, `CreditAccount`
- **Polymorphism:** All accounts implement `calculate_available_funds()`, `can_withdraw()`, `apply_monthly_fees()` with type-specific logic
- **Composition:** `StatementMonitor` owns a `TransactionCleaner` and a collection of `AlertRule` objects
- **Encapsulation:** Private attributes with validated property setters

---

## Project Structure

```
financial-tracker/
│
├── src/
│   ├── validation.py                        # 4 data validation functions (Uzzam)
│   ├── transaction.py                       # Transaction class with full validation (Uzzam)
│   ├── account.py                           # Abstract Account base class (Uzzam)
│   ├── checking_account.py                  # CheckingAccount implementation (Uzzam)
│   ├── savings_account.py                   # SavingsAccount implementation (Angelo)
│   ├── credit_account.py                    # CreditAccount implementation (Keven)
│   ├── datacleaner.py                       # TransactionCleaner + AlertRules (Kevin + Kevin)
│   ├── Project4_Financial_Tracker_Final_Version.py  # Full CSV pipeline runner
│   └── statement.csv                        # Sample bank statement
│
├── tests/
│   ├── test_checking_account.py             # CheckingAccount unit tests (Uzzam)
│   └── test_financial_tracker.py            # Full test suite (38 tests)
│
├── examples/
│   └── demo_script.py                       # End-to-end demonstration
│
├── docs/
│   ├── architecture.md
│   ├── function_reference.md
│   └── usage_example.md
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Getting Started

### Prerequisites
- Python 3.8+
- No external libraries required (standard library only)

### Installation

```bash
git clone https://github.com/uzit001/INST326-Financial-Transaction-Analytics-System.git
cd INST326-Financial-Transaction-Analytics-System
```

### Run the Demo

```bash
python examples/demo_script.py
```

### Run on Your Own Bank Statement

Place your CSV in `src/` with columns: `Date, Amount, Description, Category, Account`

```bash
python src/Project4_Financial_Tracker_Final_Version.py
```

The script auto-detects `statement.csv` in the working directory.

### Run Tests

```bash
python -m pytest tests/ -v
```

---

## Sample Output

```
=== ACCOUNT BALANCES ===
- Checking : balance=$1,308.60, available=$1,808.60
- Savings  : balance=$300.00,   available=$0.00
- Credit   : balance=-$320.50,  available=$4,679.50

=== WHERE IS MY MONEY GOING? ===
- Housing        : $1,200.00
- Shopping       :   $195.50
- Healthcare     :   $125.00
- Groceries      :    $85.42
- Transportation :    $45.99
- Subscription   :     $9.99
```

---

## Data Cleaning Pipeline

The `TransactionCleaner` normalizes raw bank data in four steps:

1. **Normalize dates** — converts `MM/DD/YYYY` → `YYYY-MM-DD`
2. **Clean descriptions** — strips trailing transaction codes (`TRN0001`, `#123`)
3. **Standardize categories** — maps `"subscr"`, `"dining"`, `"subs"` → canonical names
4. **Remove duplicates** — deduplicates by (date, amount, description, category, account)

---

## Alert Rules

`StatementMonitor` runs three alert rules out of the box:

| Rule | Default Threshold |
|---|---|
| `LargeTransactionRule` | Flags any transaction ≥ $500 |
| `CategoryLimitRule` | Flags Dining transactions > $120 |
| `SuspiciousMerchantRule` | Flags keywords: "unknown", "cash app", "money transfer" |

Custom rules can be added by subclassing `AlertRule` and implementing `check(tx)`.

---

## Team Contributions

| Team Member | Role | Key Contributions |
|---|---|---|
| **Uzzam Tariq** | Data Validation & Architecture Lead | `validation.py`, `transaction.py`, `account.py`, `checking_account.py`, test suite |
| **Angelo Montagnino** | Account Management & Analytics | `savings_account.py`, data transformation functions, category analysis |
| **Keven Day** | Credit & Analysis | `credit_account.py`, date-range filtering, account comparison utilities |
| **Kevin Miele** | Data Quality & Monitoring | `datacleaner.py`, `AlertRule` hierarchy, `StatementMonitor`, deduplication |

---

## Technologies

- **Language:** Python 3.8+
- **OOP:** Abstract classes (`abc`), inheritance, polymorphism, encapsulation
- **Data:** `csv`, `json`, `datetime`, `collections`, `re`
- **Testing:** `pytest` (38 tests, covering unit, integration, and system scenarios)
- **Style:** PEP 8, full docstrings with type hints and usage examples
