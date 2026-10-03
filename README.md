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

