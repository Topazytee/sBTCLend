
---

# sBTCLend - Bitcoin-Backed Lending Protocol

**sBTCLend** is a decentralized finance (DeFi) lending platform built on the Stacks blockchain. It enables users to create, fund, repay, and liquidate Bitcoin-backed loans in a trust-minimized and transparent manner.

---

## 📜 Overview

sBTCLend allows users to:

* **Create** loan requests backed by Bitcoin collateral.
* **Fund** existing loan requests as a lender.
* **Repay** loans with interest.
* **Withdraw** excess collateral if the loan is safely overcollateralized.
* **Liquidate** undercollateralized loans to protect lenders.

Collateral ratios, penalties, and other protocol rules are enforced on-chain using Clarity smart contracts.

---

## 🚀 Features

* 📌 **Bitcoin-backed loans** using SATs as collateral.
* 🔒 **Overcollateralization enforcement** (150% minimum, 125% liquidation threshold).
* 💸 **Dynamic interest calculation** based on duration.
* ⚖️ **Secure liquidation** mechanisms with penalties.
* 🔁 **Partial collateral withdrawal** for safe positions.
* 👤 **Loan tracking** per user.

---

## ⚙️ Constants & Parameters

| Constant                | Value     | Description                             |
| ----------------------- | --------- | --------------------------------------- |
| `COLLATERAL-RATIO`      | `u150`    | Minimum collateral ratio (150%)         |
| `LIQUIDATION-THRESHOLD` | `u125`    | Minimum ratio before liquidation (125%) |
| `LIQUIDATION-PENALTY`   | `u110`    | 10% penalty on liquidation              |
| `MAX-LOAN-DURATION`     | `u2880`   | \~20 days (144 blocks/day)              |
| `MAX-INTEREST-RATE`     | `u1000`   | 10% annual interest                     |
| `minimum-collateral`    | `u100000` | Minimum allowed collateral in SATs      |
| `protocol-fee`          | `u100`    | 1% platform fee in basis points         |

---

## 🧩 Data Structures

### Loan Structure (`loans` map)

```clarity
{
  borrower: principal,
  lender: principal,
  amount: uint,
  collateral: uint,
  interest-rate: uint,
  start-height: uint,
  end-height: uint,
  status: (string-ascii 20) ;; "PENDING", "ACTIVE", "REPAID", "LIQUIDATED"
}
```

### Liquidation Structure (`liquidations` map)

```clarity
{
  liquidator: principal,
  liquidation-height: uint,
  liquidation-amount: uint
}
```

### User Loans (`user-loans` map)

A list of up to 10 loan IDs per user:

```clarity
principal => (list 10 uint)
```

---

## 📘 Public Functions

### ✅ `create-loan(amount, collateral, interest-rate, loan-duration)`

Creates a loan request.

### ✅ `fund-loan(loan-id)`

Funds a `PENDING` loan and activates it.

### ✅ `repay-loan(loan-id)`

Repays the loan and changes the status to `REPAID`.

### ✅ `check-and-liquidate(loan-id)`

Allows anyone to liquidate undercollateralized loans.

### ✅ `withdraw-excess-collateral(loan-id, withdrawal-amount)`

Allows borrowers to withdraw excess collateral if safely above the required ratio.

---

## 🧮 Helper Functions

* 🔢 `calculate-interest(principal, rate, blocks)` – calculates interest accrued over time.
* 🧠 `is-valid-input(...)` – validates loan creation input.
* 📊 `calculate-current-collateral-ratio(...)` – determines real-time collateralization.
* 📉 `calculate-max-withdrawable-collateral(...)` – determines safe amount of collateral to withdraw.

---

## 📖 Read-Only Functions

* `get-loan(loan-id)` – Returns the full loan info.
* `get-liquidation(loan-id)` – Returns liquidation info if any.
* `get-user-loans(user)` – Returns a list of loan IDs for a user.

---

## 🛡️ Error Codes

| Code | Name                                 | Description                           |
| ---- | ------------------------------------ | ------------------------------------- |
| `u1` | `ERR-NOT-AUTHORIZED`                 | Caller not authorized for this action |
| `u2` | `ERR-INSUFFICIENT-COLLATERAL`        | Collateral below required threshold   |
| `u3` | `ERR-LOAN-NOT-FOUND`                 | Loan does not exist or wrong status   |
| `u4` | `ERR-LOAN-ALREADY-ACTIVE`            | Loan has already been funded          |
| `u5` | `ERR-INVALID-INPUT`                  | Invalid input values                  |
| `u6` | `ERR-INSUFFICIENT-EXCESS-COLLATERAL` | Withdrawal exceeds safe threshold     |
| `u7` | `ERR-LOAN-ALREADY-LIQUIDATED`        | Loan already liquidated               |

---

## 📦 Deployment Checklist

* [ ] Define token contract integrations for actual SAT collateral handling.
* [ ] Integrate protocol fees to a fee collector address.
* [ ] Implement collateral transfer logic (currently placeholder).
* [ ] Security audit and formal verification.

---

## 🧪 Testing Suggestions

Use Clarinet to write tests that cover:

* Loan creation with invalid and valid parameters.
* Loan funding and status transitions.
* Accurate interest calculation across durations.
* Safe and unsafe collateral withdrawal.
* Liquidation behavior at various collateral ratios.

---

## 📚 License

This project is open-source under the MIT License.

---
