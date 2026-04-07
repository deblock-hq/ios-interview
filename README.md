# iOS Home Task

## AI-Assisted Development

At Deblock we value effective use of AI tooling. For this task, we encourage you to use **Claude Code** (or any AI assistant) as part of your workflow.
We provide the folder `.claude/` with useful ready made assets, but you can translate those into codex or gemini too.

**What we score:**
- We will review your **git history** to assess progress and workflow. We score descriptive commits that reflect meaningful, incremental pieces of work. Bonus if we see the provided `/commit` under `.claude/commands` (or one of your own) used as you progress through the implementation. The commit history shows your structure when building a feature, and we pay attention to it.
- We have included a `.claude/rules/architecture.md` file in this repo describing the architecture and coding standards we follow at Deblock. You can feed this to your AI assistant for context, or simply use it as a manual reference: what matters is the outcome.
- We score positively if the architecture described in that file is followed.
- We pay the most attention to the graph.

---

## Getting Started

1. Create a new private repo and invite: @mario-deblock.
2. Start or upload your project.
3. We take into account:
  - Xcode SwiftUI full-lifecycle project/workspace (Bonus for workspace setup)
  - Bonus for iOS workspace with multiple targets to separate Core (platform and framework agnostic) module from SwiftUI (views and components) and Main modules
  - SOLID principles hands-on.
  - Model-View (MV) should be the natural way in SwiftUI.
  - Unit testing (core module).
  - Bonus: understanding of Composition Root concept.

  For full reference, see `.claude/rules` files.

## Requirements

### Design
<img width="1185" height="814" alt="Screens" src="https://github.com/user-attachments/assets/a2f9c31c-38d0-4e56-856b-c162bfa6ff55" />

🎨 => [Figma file to follow colors, spacing, icons, etc here](https://www.figma.com/design/62KFvBRvs44chtjkW43z0D/iOS-tests---Mar-2026?node-id=0-1&t=yXjfpNi5UNdPllu0-1)

### Features

A Deblock user has a crypto wallet containing multiple assets (e.g. ETH, BTC, USDT).
Assume the current user has the following balances:
- **ETH**: 0.051
- **BTC**: 0.051
- **USDT**: 230

The task consists of a **Buy Crypto flow** with two steps: **Set Amount** and **Set Limit**.

---

#### Step 1 — Set Amount

**Screen: "Set amount"** — _subtitle: "Your {asset} purchase"_ (e.g. "Your BTC purchase")

- The subtitle dynamically reflects the currently selected asset (e.g. selecting ETH changes it to "Your ETH purchase").
- As a user I see a numeric input field to enter the amount I want to spend in fiat (e.g. EUR).
- As a user I can select which crypto asset to buy by tapping a dropdown (e.g. "BTC"). This opens a **"Choose asset" modal**.
- The **"Choose asset" modal** displays a list of available assets with their current balances (e.g. ETH 0.051, BTC 0.051, USDT 230). Each currency shows the current conversion to EUR value below the amount. See `Data points` section to obtain values from a free API.
- A custom numeric keypad is displayed at the bottom for amount entry.
- The **"Next"** button is disabled when the amount is 0 or empty, and becomes enabled once a valid amount is entered (e.g. €50).
- Tapping "Next" navigates to the **Set Limit** screen.

---

#### Step 2 — Set Limit

**Screen: "Set limit"** — _subtitle: "Current Price: € {currentPrice}"_

- As a user I see the current market price of the selected asset displayed in the subtitle (e.g. "Current Price: € 1,460").
- As a user I see a fiat input field to set my desired limit price (the price at which I want to buy).
- Below the input, a helper text reads: _"is the price you will buy at"_.
- A **price chart** is displayed showing recent price history for the selected asset.
- The chart includes a **draggable horizontal line** (slider) that allows the user to visually set the limit price. Dragging the slider updates the input field and vice versa.
- When a limit price is set, a percentage badge is shown indicating the difference from the current price (e.g. "+0.86%").
- The **"Confirm {amount} limit"** button at the bottom reflects the entered limit price (e.g. "Confirm 1,200 limit"). It is disabled when the limit is 0 and enabled once a valid limit price is entered.

---

#### Step 3 — Order Summary

**Screen: Order summary**

- After confirming the limit, the user is taken to a summary screen.
- The screen displays the purchase amount from Step 1 (e.g. 50 €) and the limit price from Step 2 (e.g. 1,200 €).
- Additional order details are listed (asset, amounts, limit price).
- This screen serves as a final review before the user confirms the order.

---

### Data Points

You will need to consume 2 data points:
1. Exchange rate (crypto to EUR)
2. Price chart data

#### 1. Obtaining Exchange Rate

For the exchange rate, you can consume any free API. Here is a working example:

**Request**
```
https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&vs_currencies=eur
```

**Response**
```
{
  "bitcoin": {
    "eur": 1460.0
  }
}
```

You can replace `bitcoin` with `ethereum` and `vs_currencies` accepts `usd`, `eur` and `gbp`.

#### 2. Price Chart Data

For the price chart, you can use a free API to obtain historical price data. Here is a working example:

**Request**
```
https://api.coingecko.com/api/v3/coins/bitcoin/market_chart?vs_currency=eur&days=30
```

**Response**
```
{
  "prices": [
    [1648684800000, 1420.5],
    [1648771200000, 1435.2],
    ...
  ]
}
```

Each entry is a `[timestamp, price]` pair.

## What we look for
- Be pixel perfect
- Clean code (networking, UI, simplicity)
- Interactive chart with draggable limit line
- Use of AI, without the AI slop :)

---

> **Note:** Ignore the `_outdated/` folder — it contains a deprecated exercise and is not part of this task.
