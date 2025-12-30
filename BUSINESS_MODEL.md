# SWIFTLINE Business Model & Configuration

This document defines all business logic numbers, fees, limits, and configurations that need to be set in the backend for realistic testing and production deployment.

**Last Updated:** $(date)

---

## 💰 REVENUE MODEL

### Platform Fee Structure

#### Transaction Fees (Revenue from Sales)
- **Platform Fee Percentage:** `5%` (default)
- **Environment Variable:** `PLATFORM_FEE_PERCENT=5`
- **Calculation:** `platformFee = transactionAmount × 0.05`
- **Seller Receives:** `sellerPayout = transactionAmount - platformFee`
- **Example:**
  - Transaction: KES 10,000
  - Platform Fee: KES 500 (5%)
  - Seller Payout: KES 9,500

#### Fee Tier Structure (Optional - Future)
```
Transaction Amount Range    | Platform Fee
---------------------------|-------------
KES 0 - 1,000              | 5% (KES 50 max)
KES 1,001 - 10,000         | 5%
KES 10,001 - 50,000        | 4.5%
KES 50,001 - 100,000       | 4%
KES 100,000+               | 3.5%
```

---

## 💸 WITHDRAWAL FEES

### Platform Withdrawal Fee
- **Platform Fee:** `2%` of withdrawal amount
- **Minimum Platform Fee:** KES 10
- **Maximum Platform Fee:** KES 500

### Payment Method Fees

#### M-Pesa Withdrawal Fees
- **Fee Type:** Fixed
- **Fee Amount:** KES 27 (M-Pesa B2C fee)
- **Processing Time:** Instant (within 10 seconds)
- **Minimum Withdrawal:** KES 100
- **Maximum Per Transaction:** KES 150,000
- **Daily Limit:** KES 300,000

#### Airtel Money Withdrawal Fees
- **Fee Type:** Fixed
- **Fee Amount:** KES 25
- **Processing Time:** Instant (within 10 seconds)
- **Minimum Withdrawal:** KES 100
- **Maximum Per Transaction:** KES 150,000
- **Daily Limit:** KES 300,000

#### Bank Account Withdrawal Fees
- **Fee Type:** Fixed
- **Fee Amount:** KES 50
- **Processing Time:** 1-3 business days
- **Minimum Withdrawal:** KES 500
- **Maximum Per Transaction:** KES 500,000
- **Daily Limit:** KES 1,000,000

### Withdrawal Fee Calculation Example
```
Withdrawal Amount: KES 10,000
Platform Fee (2%): KES 200
M-Pesa Fee: KES 27
Total Fees: KES 227
Net Amount to Seller: KES 9,773
```

---

## 🔒 ESCROW & TRANSACTION LIMITS

### Transaction Limits
- **Minimum Transaction Amount:** KES 100
- **Maximum Transaction Amount:** KES 500,000
- **Transaction Expiry:** 7 days (from creation)
- **Auto-refund After Expiry:** Yes (if not paid)

### Escrow Period
- **Escrow Duration:** Until buyer confirms delivery
- **Maximum Escrow Period:** 30 days (auto-release if no dispute)
- **Minimum Escrow Period:** Until delivery confirmation
- **Auto-release After:** 7 days post-delivery (if buyer doesn't confirm)

### Dispute Window
- **Dispute Window:** 7 days after delivery confirmation
- **After Dispute Window:** Transaction auto-completes
- **Dispute Resolution Time:** 48 hours (admin review)

---

## 📱 PAYMENT METHOD FEES (Buyer Side)

### M-Pesa STK Push
- **Buyer Fee:** None (buyer pays exact amount)
- **Platform Pays:** M-Pesa charges (~KES 0.50 per transaction)
- **Processing Time:** Instant

### Mobile Money (Airtel, T-Kash)
- **Buyer Fee:** None (buyer pays exact amount)
- **Platform Pays:** Provider charges (~KES 0.50 per transaction)
- **Processing Time:** Instant

### Card Payments
- **Buyer Fee:** None (buyer pays exact amount)
- **Platform Pays:** Card processor fees (2.9% + KES 10)
- **Processing Time:** Instant

---

## 🏪 STORE & PRODUCT LIMITS

### Store Limits
- **Stores Per Seller:** 1 (one store per seller)
- **Store Slug:** Unique, URL-friendly
- **Store Visibility:** PRIVATE (default) or PUBLIC (opt-in)
- **Store Activation Requirements:**
  - At least 1 connected social page (Instagram/Facebook)
  - At least 1 active payout method
  - Store name and slug configured

### Product Limits
- **Products Per Store:** Unlimited
- **Product Images:** Maximum 10 images per product
- **Product Price Range:** KES 100 - KES 500,000
- **Product Currency:** KES (default), supports multi-currency

### AI Sync Limits
- **Initial Scan:** Full page scan (all posts)
- **Auto-Sync Frequency:** Every 6 hours
- **Manual Rescan:** Unlimited
- **Sync Retry Limit:** 3 attempts before disabling
- **Rate Limit:** 100 API calls per hour per social account

---

## 💼 WALLET & BALANCE LIMITS

### Wallet Limits
- **Minimum Balance:** KES 0
- **Maximum Balance:** KES 10,000,000 (per seller)
- **Pending Escrow Limit:** KES 5,000,000 (at any time)
- **Currency:** KES (Kenyan Shilling)

### Payout Limits
- **Minimum Withdrawal:** KES 100
- **Maximum Single Withdrawal:** KES 500,000
- **Daily Withdrawal Limit:** KES 1,000,000
- **Monthly Withdrawal Limit:** KES 10,000,000
- **Withdrawal Processing:** Instant (M-Pesa/Mobile Money) or 1-3 days (Bank)

---

## 🤖 AI FEATURE LIMITS

### AI Product Extraction
- **Confidence Score Threshold:** 0.5 (50%) minimum
- **Auto-Publish:** Never (always requires seller review)
- **Extraction Fields:**
  - Product Name (required)
  - Price (optional, can be missing)
  - Description (optional)
  - Images (minimum 1, maximum 10)

### AI Sync Rules
- **New Posts:** Created as DRAFT products
- **Updated Posts:** Suggest updates (requires review)
- **Deleted Posts:** Archive corresponding products
- **Failed Extractions:** Saved as DRAFT with warnings

---

## 🛡️ SECURITY & RISK LIMITS

### Transaction Risk Limits
- **Maximum Transactions Per Day (Buyer):** 10 transactions
- **Maximum Transaction Value Per Day (Buyer):** KES 100,000
- **Velocity Check:** Flag if >5 transactions in 1 hour
- **IP Reputation:** Block known fraud IPs

### Seller Risk Limits
- **New Seller Limit:** KES 10,000 per transaction (first 10 transactions)
- **Verified Seller Limit:** KES 500,000 per transaction
- **Dispute Rate Threshold:** >10% disputes = account review
- **Auto-Freeze:** If dispute rate >20%

---

## 📊 FEE BREAKDOWN EXAMPLES

### Example 1: Small Transaction
```
Product Price: KES 1,000
Platform Fee (5%): KES 50
Seller Receives: KES 950

Seller Withdraws KES 950:
- Platform Withdrawal Fee (2%): KES 19
- M-Pesa Fee: KES 27
- Total Fees: KES 46
- Net to Seller: KES 904
```

### Example 2: Medium Transaction
```
Product Price: KES 10,000
Platform Fee (5%): KES 500
Seller Receives: KES 9,500

Seller Withdraws KES 9,500:
- Platform Withdrawal Fee (2%): KES 190
- M-Pesa Fee: KES 27
- Total Fees: KES 217
- Net to Seller: KES 9,283
```

### Example 3: Large Transaction
```
Product Price: KES 50,000
Platform Fee (5%): KES 2,500
Seller Receives: KES 47,500

Seller Withdraws KES 47,500:
- Platform Withdrawal Fee (2%): KES 950
- M-Pesa Fee: KES 27
- Total Fees: KES 977
- Net to Seller: KES 46,523
```

---

## 🔧 BACKEND ENVIRONMENT VARIABLES

### Required Configuration
```env
# Platform Fees
PLATFORM_FEE_PERCENT=5                    # Platform fee percentage (5%)
WITHDRAWAL_FEE_PERCENT=2                  # Withdrawal fee percentage (2%)
MIN_WITHDRAWAL_FEE=10                     # Minimum withdrawal fee (KES)
MAX_WITHDRAWAL_FEE=500                    # Maximum withdrawal fee (KES)

# Transaction Limits
MIN_TRANSACTION_AMOUNT=100                # Minimum transaction (KES)
MAX_TRANSACTION_AMOUNT=500000             # Maximum transaction (KES)
TRANSACTION_EXPIRY_DAYS=7                 # Transaction expiry (days)

# Escrow Limits
MAX_ESCROW_DAYS=30                        # Maximum escrow period (days)
AUTO_RELEASE_DAYS=7                       # Auto-release after delivery (days)

# Wallet Limits
MIN_WITHDRAWAL=100                        # Minimum withdrawal (KES)
MAX_SINGLE_WITHDRAWAL=500000              # Max single withdrawal (KES)
DAILY_WITHDRAWAL_LIMIT=1000000            # Daily withdrawal limit (KES)
MAX_WALLET_BALANCE=10000000               # Maximum wallet balance (KES)

# Payment Method Fees (KES)
MPESA_B2C_FEE=27                          # M-Pesa B2C fee
AIRTEL_MONEY_FEE=25                       # Airtel Money fee
BANK_TRANSFER_FEE=50                      # Bank transfer fee

# Payment Limits
MPESA_MAX_PER_TRANSACTION=150000          # M-Pesa max per transaction
MPESA_DAILY_LIMIT=300000                  # M-Pesa daily limit
BANK_MAX_PER_TRANSACTION=500000            # Bank max per transaction
BANK_DAILY_LIMIT=1000000                  # Bank daily limit

# AI Sync Limits
AI_SYNC_FREQUENCY_HOURS=6                 # Auto-sync frequency (hours)
AI_SYNC_RETRY_LIMIT=3                     # Max retry attempts
AI_CONFIDENCE_THRESHOLD=0.5               # Minimum confidence score (0-1)
AI_RATE_LIMIT_PER_HOUR=100                # API calls per hour

# Risk Limits
MAX_TRANSACTIONS_PER_DAY=10               # Per buyer
MAX_TRANSACTION_VALUE_PER_DAY=100000      # Per buyer (KES)
VELOCITY_CHECK_THRESHOLD=5                # Transactions per hour
NEW_SELLER_LIMIT=10000                    # First 10 transactions (KES)
VERIFIED_SELLER_LIMIT=500000              # After verification (KES)
DISPUTE_RATE_THRESHOLD=0.1                # 10% dispute rate
AUTO_FREEZE_THRESHOLD=0.2                 # 20% dispute rate

# Product Limits
MAX_PRODUCT_IMAGES=10                     # Maximum images per product
MIN_PRODUCT_PRICE=100                     # Minimum product price (KES)
MAX_PRODUCT_PRICE=500000                  # Maximum product price (KES)
```

---

## 📈 REVENUE PROJECTIONS (Example)

### Scenario: 1,000 Transactions/Month
```
Average Transaction: KES 5,000
Total Volume: KES 5,000,000
Platform Fees (5%): KES 250,000
Withdrawal Fees (2%): ~KES 50,000
Total Revenue: ~KES 300,000/month
```

### Scenario: 10,000 Transactions/Month
```
Average Transaction: KES 5,000
Total Volume: KES 50,000,000
Platform Fees (5%): KES 2,500,000
Withdrawal Fees (2%): ~KES 500,000
Total Revenue: ~KES 3,000,000/month
```

---

## 🎯 KEY BUSINESS RULES

### Fee Collection
1. **Platform fee deducted at payment confirmation** (when buyer pays)
2. **Fee stored in transaction record** (`platformFee` field)
3. **Seller payout calculated** (`sellerPayout = amount - platformFee`)
4. **Funds held in escrow** until delivery confirmation
5. **Net amount released** to seller's available balance

### Withdrawal Process
1. **Seller requests withdrawal** from available balance
2. **Platform fee calculated** (2% of withdrawal amount)
3. **Payment method fee added** (M-Pesa/Bank specific)
4. **Total fees deducted** from withdrawal amount
5. **Net amount sent** to seller's payout method

### Refund Policy
- **Buyer cancels before payment:** No fees
- **Buyer cancels after payment:** Full refund (platform fee refunded)
- **Seller cancels:** Full refund + seller penalty (optional)
- **Dispute resolved for buyer:** Full refund
- **Dispute resolved for seller:** Funds released normally

---

## 🔄 TRANSACTION FLOW WITH FEES

### Complete Flow Example
```
1. Buyer selects product: KES 10,000
2. Buyer pays: KES 10,000 (no buyer fees)
3. Platform receives: KES 10,000
4. Platform fee deducted: KES 500 (5%)
5. Escrow amount: KES 10,000 (full amount held)
6. Seller sees: KES 9,500 pending (after fee)
7. Delivery confirmed
8. Seller receives: KES 9,500 in available balance
9. Seller withdraws KES 9,500:
   - Platform fee: KES 190 (2%)
   - M-Pesa fee: KES 27
   - Net payout: KES 9,283
```

---

## 📝 LEDGER RECORDS

### Transaction Ledger Entry
```json
{
  "transactionId": "TXN-...",
  "grossAmount": 10000,
  "platformFee": 500,
  "netAmount": 9500,
  "currency": "KES",
  "timestamp": "2025-01-XX..."
}
```

### Withdrawal Ledger Entry
```json
{
  "withdrawalId": "WD-...",
  "requestedAmount": 9500,
  "platformFee": 190,
  "methodFee": 27,
  "totalFees": 217,
  "netPayout": 9283,
  "currency": "KES",
  "timestamp": "2025-01-XX..."
}
```

---

## 🎛️ ADMIN CONTROLS

### Adjustable Parameters (Admin Panel)
- Platform fee percentage (per transaction)
- Withdrawal fee percentage
- Transaction limits (min/max)
- Escrow periods
- Risk thresholds
- AI sync frequency

### Fixed Parameters (System)
- Payment provider fees (M-Pesa, Bank rates)
- Currency conversion rates
- Regulatory compliance limits

---

## 📊 REPORTING METRICS

### Revenue Metrics
- Total platform fees collected
- Total withdrawal fees collected
- Average transaction value
- Average platform fee per transaction
- Revenue per seller
- Revenue per buyer

### Operational Metrics
- Transaction volume (KES)
- Number of transactions
- Average escrow duration
- Dispute rate
- Refund rate
- Withdrawal success rate

---

## 🔐 COMPLIANCE & REGULATIONS

### Kenya-Specific Limits
- **M-Pesa Limits:** As per Safaricom regulations
- **Bank Transfer Limits:** As per bank policies
- **Tax Compliance:** Platform fees subject to VAT (16%)
- **Reporting:** Monthly transaction reports to Central Bank

### Data Retention
- **Transaction Records:** 7 years
- **User Data:** 5 years after account closure
- **Audit Logs:** 3 years

---

## 🚀 TESTING CONFIGURATIONS

### Development/Testing Environment
```env
PLATFORM_FEE_PERCENT=5
WITHDRAWAL_FEE_PERCENT=2
MIN_TRANSACTION_AMOUNT=100
MAX_TRANSACTION_AMOUNT=500000
MPESA_B2C_FEE=27
```

### Staging Environment
```env
PLATFORM_FEE_PERCENT=5
WITHDRAWAL_FEE_PERCENT=2
MIN_TRANSACTION_AMOUNT=100
MAX_TRANSACTION_AMOUNT=500000
MPESA_B2C_FEE=27
```

### Production Environment
```env
PLATFORM_FEE_PERCENT=5
WITHDRAWAL_FEE_PERCENT=2
MIN_TRANSACTION_AMOUNT=100
MAX_TRANSACTION_AMOUNT=500000
MPESA_B2C_FEE=27
```

---

## 📋 QUICK REFERENCE

### Fee Summary
- **Platform Fee:** 5% of transaction
- **Withdrawal Fee:** 2% of withdrawal amount
- **M-Pesa Fee:** KES 27 per withdrawal
- **Airtel Money Fee:** KES 25 per withdrawal
- **Bank Transfer Fee:** KES 50 per withdrawal

### Limits Summary
- **Min Transaction:** KES 100
- **Max Transaction:** KES 500,000
- **Min Withdrawal:** KES 100
- **Max Withdrawal:** KES 500,000 per transaction
- **Daily Withdrawal Limit:** KES 1,000,000

---

**Note:** All amounts are in Kenyan Shillings (KES). Update environment variables in `.env` file for backend configuration.

