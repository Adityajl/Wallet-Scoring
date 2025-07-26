# 🧠 Wallet Risk Scoring — Compound V2/V3 On-Chain Analysis

This project scores Ethereum wallet addresses based on their historical activity with the Compound protocol, producing a risk score between **0 and 1000** for each wallet.

---

## 📌 Objective

To build a risk profiling pipeline from scratch using on-chain transaction data, extracting Compound-specific behaviors to generate a custom risk score per wallet address.

---

## 📥 Data Collection Method

- **Source**: [Etherscan API](https://etherscan.io/apis)
- **Method**:
  - Fetched historical transaction data using the `txlist` endpoint for each wallet.
  - Filtered transactions targeting **Compound V2** smart contracts (`cETH`, `cDAI`, `cUSDC`, etc.).
  - Decoded function selectors from input data to classify actions (`borrow`, `repay`, `supply`, etc.).

---

## 🧪 Feature Selection Rationale

We extracted the following activity-based features for each wallet:

| Feature | Description |
|--------|-------------|
| `num_borrow` | Total number of borrow transactions |
| `num_repay`  | Total number of repay transactions |
| `num_supply` | Total number of supply transactions |
| `total_actions` | Sum of the above (Compound protocol usage count) |

These features reflect a wallet’s **engagement level**, which correlates with exposure to credit or protocol-based risk.

---

## 🧮 Scoring Method

Each wallet’s score is computed based on interaction frequency:
score = min(1000, total_actions * 100)


- **Why this scale?**\
  To prioritize wallets with more frequent borrowing/supplying/repayment activity.
- **Max score cap**: 1000
- **Default score**: Wallets with zero Compound interaction receive a score of `0`.

---

## 📊 Risk Indicators Justification

The scoring reflects the **behavioral risk** of wallets:
- High-frequency interactions with lending protocols often indicate larger or more sophisticated users.
- Wallets that have only borrowed but not repaid would trigger risk flags in further refinement.
- This model focuses on **usage pattern**, not financial health (e.g., debt ratio or liquidation risk), but it’s extensible to that.

---

## ✅ Outputs

- `wallet_scores.csv` — A clean CSV with:

```csv
wallet_id,score
0xabc123...,740
...


