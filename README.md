# payment-fraud-and-risk-framework

# Fraud Triage Matrix

This matrix guides initial risk assessments for fiat-to-crypto transactions flagged by our automated systems.

## Level 1: Low Risk (Monitor)
- **Flag:** First-time purchase from a new IP, but matching the billing country.
- **Action:** Allow transaction to process. Log IP and device fingerprint for future baseline.

## Level 2: Medium Risk (Manual Review Required)
- **Flag:** High-velocity attempts (3+ attempts in 10 minutes) with differing card numbers but the same KYC profile.
- **Flag:** IP Address (e.g. proxy/VPN) does not match Card BIN country.
- **Action:** Halt automatic crypto settlement. Request a manual 3D-Secure verification or Liveness Check (selfie with ID). 

## Level 3: High Risk (Decline & Escalate)
- **Flag:** Known compromised Card BIN combined with Tor exit node IP.
- **Flag:** Name on KYC profile severely mismatches the name on the credit card statement.
- **Action:** Hard decline the fiat capture. Flag the user profile for AML/Compliance review. Do not initiate on-chain transfer.

- # 🔍 On-Chain Address Screening SOP

Before processing high-value fiat on-ramps, destination addresses must be screened for compliance.

## Pre-Flight Checklist
1. **Sanctions Check:** Query the destination wallet address against the current OFAC SDN list.
2. **Smart Contract Verification:** If the user is sending to a smart contract rather than an EOA (Externally Owned Account), verify the contract is not a known mixer (e.g., Tornado Cash).
3. **Phishing/Scam DBs:** Check the address on Etherscan/Solscan for community warnings (red flag banners indicating involvement in phishing or exploits).

## Handling Flagged Addresses
# ⚖️ Chargeback & Dispute Defense Workflow

When a user initiates a chargeback via their bank (claiming fraud or non-delivery), follow this workflow to build a representment case.

## 1. Gather KYC Evidence
- Export the user's verified Identity Document (ID/Passport).
- Export the timestamped Liveness Check / Selfie.
- Export IP logs matching the location of the cardholder.

## 2. Gather On-Chain Evidence
Since we are a non-custodial provider, the blockchain acts as the receipt of delivery.
- Locate the On-Chain TX Hash.
- Generate a PDF of the block explorer page showing:
  - Status: `Success`
  - Destination Address: Matching the user's inputted address.
  - Timestamp: Matching the time of fiat capture.

## 3. Submit Representment
Combine the KYC data, IP logs, and On-Chain delivery proof into a single PDF packet and upload it to the payment processor's dispute portal within the 14-day window.


# ⚖️ Chargeback & Dispute Defense Workflow

When a user initiates a chargeback via their bank (claiming fraud or non-delivery), follow this workflow to build a representment case.

## 1. Gather KYC Evidence
- Export the user's verified Identity Document (ID/Passport).
- Export the timestamped Liveness Check / Selfie.
- Export IP logs matching the location of the cardholder.

## 2. Gather On-Chain Evidence
Since we are a non-custodial provider, the blockchain acts as the receipt of delivery.
- Locate the On-Chain TX Hash.
- Generate a PDF of the block explorer page showing:
  - Status: `Success`
  - Destination Address: Matching the user's inputted address.
  - Timestamp: Matching the time of fiat capture.

## 3. Submit Representment
Combine the KYC data, IP logs, and On-Chain delivery proof into a single PDF packet and upload it to the payment processor's dispute portal within the 14-day window.

# 🗺️ Block Explorer Quick Reference

A guide for Support Agents on how to read block explorers to answer customer tickets quickly.

## Etherscan (Ethereum / EVM Chains)
- **Status: Success (Green):** Funds were delivered. The issue is with the user's wallet UI.
- **Status: Reverted (Red):** The transaction failed on-chain (often due to Out of Gas or Slippage). The fiat must be refunded or the transaction rebroadcast.
- **Internal Transactions Tab:** Use this tab if the crypto was sent from a smart contract. The transfer will NOT show up on the main "Transactions" list.

## Solscan (Solana)
- **Finalized vs. Confirmed:** A transaction is only fully immutable once it says `Finalized`. `Confirmed` means it is still being voted on by validators.
- **Token Accounts:** Solana creates specific sub-accounts for tokens (SPL). Ensure the user is looking at their SPL token balance, not just their native SOL balance.

## Mempool.space (Bitcoin)
- **Unconfirmed / ETA:** Use the mempool block visualization to see where the user's transaction is sitting. If their sat/vB fee is lower than the current median, tell them the ETA could be several hours.
- If an address triggers a high-risk score on Chainalysis/TRM Labs, **DO NOT PROCESS**.
- Escalate the Order ID and Destination Address to the Compliance Officer.
- Inform the user: *"Your transaction is undergoing a routine manual security review in accordance with regulatory requirements."*
