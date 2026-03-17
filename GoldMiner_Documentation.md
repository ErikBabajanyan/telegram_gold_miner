# ⛏ Gold Miner
### Telegram Mini App — TON Blockchain Integration
**Complete Game Design & Economy Documentation · Version 1.1 · March 2026**

---

## Table of Contents

1. [Game Overview & Vision](#1-game-overview--vision)
2. [Core Gameplay Loop](#2-core-gameplay-loop)
3. [Miner System](#3-miner-system)
4. [Upgrade System](#4-upgrade-system)
5. [Merge System](#5-merge-system)
6. [Gold Economy & Mining Rewards](#6-gold-economy--mining-rewards)
7. [Marketplace](#7-marketplace)
8. [TON Wallet Integration](#8-ton-wallet-integration)
9. [Pricing & Economic Balance](#9-pricing--economic-balance)
10. [Player Progression](#10-player-progression)
11. [MongoDB Database Schema](#11-mongodb-database-schema)
12. [API Architecture](#12-api-architecture)
13. [Security & Anti-Cheat](#13-security--anti-cheat)
14. [UI/UX Design Guidelines](#14-uiux-design-guidelines)
15. [Roadmap & Future Features](#15-roadmap--future-features)
16. [WebSocket Integration — Real-Time Data](#16-websocket-integration--real-time-data)

---

## 1. Game Overview & Vision

Gold Miner is a Telegram Mini App idle mining game with real crypto integration via the TON blockchain. Players own virtual miners that passively produce gold over time. Gold can be used to upgrade miners, which in turn produce more gold. High-rarity miners (Rare and above) can be sold on the in-game marketplace for real TON cryptocurrency, creating a genuine play-to-earn loop.

### Core Value Proposition

- **No clicking required** — fully idle, passive income
- **Real crypto earnings** via TON marketplace
- **Long-term progression** — merge system creates months of engagement
- **Fair economy** — 2-miner cap prevents whale domination
- **Telegram-native** — zero install, instant access, viral via referrals

### Target Audience

| Segment | Profile | Hook |
|---|---|---|
| Casual gamers | Play 5–10 min/day, check progress passively | Easy idle loop, no skill required |
| Crypto enthusiasts | Already use TON/Telegram wallet | Real withdrawal to wallet, earn while idle |
| Competitive players | Want to top leaderboards, show rare miners | Rare+ miners as status symbols on marketplace |
| Investors/traders | Buy low, sell high on marketplace | Miner prices fluctuate, arbitrage opportunities |

---

## 2. Core Gameplay Loop

The game loop is designed to be simple to understand but deep to master. Every action feeds back into the core cycle of **produce → upgrade → merge → earn**.

### The Core Loop (6 steps)

1. Register → receive 1 free Common L1 miner
2. Miner produces gold passively (no tapping needed)
3. Collect gold → spend gold to upgrade miner
4. Buy a 2nd Common miner from the Starter Shop (paid in Gold, unlimited)
5. Upgrade both miners to L20 — takes ~33 days total
6. Merge both L20 Common miners → Uncommon L1. Continue merging through rarities → sell Rare+ on marketplace for TON

### Daily Player Actions

- Open app → gold auto-collected from active miners
- Check upgrade status — spend gold to level up miners
- Browse marketplace — buy/sell Rare+ miners
- Monitor TON balance — withdraw when ready
- Invite friends for referral bonuses

### Session Design

Sessions are designed to be short (2–5 minutes) with a reason to return every few hours. The idle accumulation cap of 48 hours ensures players check in at least once every 2 days.

| Timing | Action | Reward |
|---|---|---|
| Every 2–6 hrs | Collect gold | Gold added to balance |
| Daily | Upgrade miner level | Increased gold/hr |
| Weekly | Complete a merge | New higher-rarity miner |
| Monthly | Sell Rare miner / withdraw | Real TON earnings |

---

## 3. Miner System

### 3.1 Rarity Tiers

There are 5 rarity tiers. Each tier is significantly more powerful than the previous, but requires substantial investment to obtain. Common and Uncommon miners are not tradeable — only obtainable via the starter shop or merging. Rare, Epic, and Legendary miners can be listed on the marketplace.

| Rarity | Gold/hr L1 | Gold/hr L20 | Max Level | Marketplace | How to Get | Commons Needed |
|---|---|---|---|---|---|---|
| Common | 20 | 395 | 20 | ❌ No | Starter shop (Gold only) | 1 |
| Uncommon | 80 | 1,580 | 20 | ❌ No | Merge 2× Common L20 | 2 |
| Rare | 320 | 6,319 | 20 | ✅ Yes | Merge 2× Uncommon L20 | 4 |
| Epic | 1,280 | 25,278 | 20 | ✅ Yes | Merge 2× Rare L20 | 8 |
| Legendary | 5,120 | 101,112 | 20 | ✅ Yes | Merge 2× Epic L20 | 16 |

### 3.2 Active Miner Slots

> **2-Miner Cap Rule**
> - Each player can have exactly **2 miners active** at the same time
> - This is a **permanent cap** — it cannot be increased
> - On registration: 1 active slot is filled (free Common L1)
> - The 2nd slot is empty until the player buys from the Starter Shop (with Gold) or completes a merge
> - Miners listed for sale on the marketplace are **removed** from active slots

**Why a fixed 2-miner cap?**
- Prevents whales from running 10+ miners and flooding the gold market
- Every player has the same production ceiling per miner slot
- Creates genuine strategic choices: which 2 miners to keep active?
- Keeps gold inflation under control at all player levels

### 3.3 Miner States

| State | Description | Effect |
|---|---|---|
| **Active** | Placed in a mining slot | Produces gold passively every second |
| **Inventory** | Owned but not in a slot | Does NOT produce gold — waiting to be activated or merged |
| **Listed** | Posted on marketplace | Removed from active slot, no gold production until sold or delisted |
| **Merging** | Being consumed in a merge | Permanently destroyed — gold invested is lost |

---

## 4. Upgrade System

### 4.1 How Upgrades Work

Upgrading a miner increases its gold production rate. Upgrades are paid exclusively in **Gold** (the in-game currency). There is no TON payment path for upgrades — this ensures gold has a permanent use and cannot lose value to zero.

- Maximum level: **20** for all rarities
- Upgrade currency: **Gold only**
- Upgrade is instant — no wait time
- Levels are not transferable — upgrading a miner and then merging consumes that gold
- Both active miners can be upgraded independently

### 4.2 Upgrade Cost Formula

Costs scale exponentially to create a meaningful mid-game challenge:

```
upgradeCost(rarity, level) = BASE_COST[rarity] × 1.35^(level - 1)
```

| Rarity | L1→L2 | L5→L6 | L10→L11 | L15→L16 | L19→L20 | **Total L1→L20** |
|---|---|---|---|---|---|---|
| Common | 150 | 498 | 2,234 | 10,018 | 33,274 | **127,914** |
| Uncommon | 600 | 1,993 | 8,936 | 40,070 | 133,094 | **511,647** |
| Rare | 2,400 | 7,972 | 35,745 | 160,282 | 532,377 | **2,046,596** |
| Epic | 9,600 | 31,886 | 142,980 | 641,127 | 2,129,507 | **8,186,385** |
| Legendary | 38,400 | 127,546 | 571,920 | 2,564,508 | 8,518,028 | **32,745,536** |

### 4.3 Gold Production vs Upgrade Cost (Common)

| Level | Gold/hr | Daily gain | Upgrade cost | Cumulative cost |
|---|---|---|---|---|
| L1 | 20 | 480 | 150 | 0 |
| L3 | 27 | 650 | 360 | 383 |
| L5 | 37 | 889 | 866 | 1,343 |
| L8 | 55 | 1,320 | 3,224 | 5,590 |
| L10 | 82 | 1,968 | 7,746 | 13,811 |
| L13 | 112 | 2,688 | 28,845 | 52,173 |
| L15 | 148 | 3,552 | 69,300 | 102,640 |
| L18 | 215 | 5,160 | 258,066 | — |
| L20 | 395 | 9,480 | MAX | **127,914** |

---

## 5. Merge System

### 5.1 Merge Rules

Merging is the core progression mechanic. It is the only way to obtain higher-rarity miners and is **irreversible**. Both input miners are permanently destroyed.

> **Merge Requirements**
> - Exactly **2 miners** of the same rarity, both at **Level 20**
> - Both miners must be in **inventory** (not active, not listed)
> - The merge is **permanent** — input miners are destroyed
> - Result: 1 miner of the **next rarity at Level 1**
> - All gold invested in upgrades is permanently consumed (this is the gold sink)
> - During merge: player is temporarily reduced to 1 active miner

### 5.2 Merge Chain

| Result | Input Required | Commons Needed | Gold Invested (Cumul.) | Gold/hr Gained |
|---|---|---|---|---|
| Uncommon L1 | 2× Common L20 | 2 | 255,828 | +60 g/hr |
| Rare L1 | 2× Uncommon L20 | 4 | 9,337,592 | +240 g/hr |
| Epic L1 | 2× Rare L20 | 8 | ~74.8M | +960 g/hr |
| Legendary L1 | 2× Epic L20 | 16 | ~598M | +3,840 g/hr |

### 5.3 The Path to Legendary

Getting one Legendary miner requires consuming **16 Common miners** worth of upgrade gold:

| Step | Action | Miners consumed | Gold consumed |
|---|---|---|---|
| Step 1 | Max 2 Common → Uncommon | 2× Common L20 | 255,828 |
| Step 2 | Repeat ×4 to get 4 Uncommon L20 | 8× Common L20 | ~2M+ |
| Step 3 | Max 2 Uncommon → Rare | 4× Uncommon L20 | 2,046,596 |
| Step 4 | Repeat ×2 to get 2 Rare L20 | 8× Uncommon L20 | ~4M |
| Step 5 | Merge 2 Rare → Epic, repeat for 2nd | 4× Rare L20 | ~8.2M |
| Step 6 | Max 2 Epic → Legendary | 2× Epic L20 | ~16.4M |
| **TOTAL** | **1 Legendary L1** | **16 Common miners** | **~598M gold** |

> **Economy Design Note:** Getting one Legendary miner requires destroying 16 Common miners worth of upgrade gold. This is intentional — Legendary miners are the gold sink that prevents inflation. A Legendary L20 produces 101,112 gold/hr. The merge system ensures gold always has a huge destination sink, keeping it valuable at all stages.

---

## 6. Gold Economy & Mining Rewards

### 6.1 Gold Production Formula

```
goldPerHour(rarity, level) = BASE_RATE[rarity] × 1.17^(level - 1)
```

| Rarity | L1 | L5 | L10 | L15 | L20 | L20 Daily |
|---|---|---|---|---|---|---|
| Common | 20 | 37 | 82 | 180 | 395 | 9,480 |
| Uncommon | 80 | 150 | 329 | 721 | 1,580 | 37,920 |
| Rare | 320 | 600 | 1,315 | 2,882 | 6,319 | 151,656 |
| Epic | 1,280 | 2,399 | 5,259 | 11,530 | 25,278 | 606,672 |
| Legendary | 5,120 | 9,594 | 21,035 | 46,118 | 101,112 | 2,426,688 |

### 6.2 Gold Collection Mechanics

- Gold accumulates automatically while the app is closed (idle/offline)
- **Accumulation cap: 48 hours** of production maximum per miner
- No manual tapping required — fully idle
- Gold is collected instantly when player opens the app
- Both active miners accumulate simultaneously

### 6.3 Gold Sinks

A healthy economy needs sinks — ways for gold to permanently leave circulation:

| Sink | How it works | Volume | Impact |
|---|---|---|---|
| Upgrades | Gold spent to level up miners | ~128K per Common miner | Primary daily sink |
| Merge cost | All upgrade gold destroyed when merging | Hundreds of thousands per merge | Major — irregular but massive |
| Idle cap | Gold above 48hr cap is lost | Small, occasional | Minor |

---

## 7. Marketplace

### 7.1 Overview

The marketplace is a player-to-player exchange where **Rare, Epic, and Legendary miners** can be bought and sold using TON. The game acts as an escrow — TON is held until the transaction completes, then transferred minus the platform fee.

> **Marketplace Rules**
> - Only **Rare, Epic, and Legendary** miners can be listed
> - Common and Uncommon miners are **NOT tradeable** — merge or keep
> - **Sellers set their own price freely** — any TON amount they choose
> - Platform fee: **5%** of sale price (deducted from seller's proceeds)
> - Listing duration: **7 days** — auto-expires if unsold
> - Miners listed on the marketplace stop producing gold
> - Buyer pays in TON — transfer is atomic and instant upon payment

### 7.2 Free Market Pricing — Seller Sets the Price

There are no fixed or suggested prices. Every seller sets their own asking price when creating a listing. The market determines value through supply and demand.

**How pricing works:**
- Seller enters any TON amount they want when listing a miner
- Example: User A lists Rare L20 for **50 TON**. User B lists Rare L20 for **100 TON**
- Buyers browse all listings and choose which price they accept
- No minimum or maximum price is enforced by the game
- Sellers can cancel and relist at a different price at any time

This creates a real open market where:
- Prices reflect actual supply and demand, not game-set values
- Early sellers of a new rarity can command premium prices
- Competition between sellers naturally drives prices toward fair value
- Rare L20 will naturally sell for more than Rare L1 — seller decides the premium

### 7.3 Starter Shop (Game-Sold Items)

The game sells Common L1 miners directly in exchange for **Gold**. There is no TON involved. Purchases are unlimited.

| Item | Price | Currency | Limit |
|---|---|---|---|
| Common L1 Miner | 5,000 Gold | Gold only — no TON | Unlimited |

> **Why Gold, Not TON?**
> Paying gold creates a meaningful early-game decision: upgrade my miner vs buy a 2nd one. Common miners are NOT on the TON marketplace — the only ways to get them are the free starter miner or buying with gold from this shop.

### 7.4 Transaction Flow

1. Seller lists miner → sets TON price → miner removed from active slot
2. Buyer browses listings → selects miner → initiates purchase via TonConnect
3. TON sent to game escrow smart contract
4. Smart contract verifies payment → triggers backend webhook
5. Backend transfers miner ownership atomically in MongoDB
6. Seller receives **95%** of TON to their in-app balance
7. Buyer receives miner in inventory → can activate immediately

---

## 8. TON Wallet Integration

### 8.1 TonConnect 2.0

Players connect their TON wallet using the TonConnect 2.0 protocol. This is a non-custodial connection — the game never holds private keys.

**Supported wallets:**
- Telegram Wallet (built-in, primary)
- Tonkeeper
- MyTonWallet
- Any TonConnect 2.0 compatible wallet

### 8.2 Top-Up (Deposit) Flow

1. Player taps "Top Up" in wallet screen
2. Selects amount or enters custom TON amount
3. TonConnect opens native Telegram Wallet / external wallet
4. Player confirms transaction — sends TON to game treasury address
5. TON blockchain confirms (3 block confirmations required)
6. Backend credits player's in-app TON balance
7. Player can now spend TON on marketplace purchases or withdraw

### 8.3 Withdrawal Flow

1. Player taps "Withdraw" → enters amount
2. System validates: balance ≥ amount + network fee
3. Withdrawal request created with status: **pending**
4. Player balance debited immediately (prevents double-spend)
5. Automated processor signs and broadcasts TON transaction
6. On-chain confirmation → status: **completed**, tx hash stored
7. On failure → balance refunded, status: **failed**

### 8.4 Fee & Limit Reference

| Parameter | Deposit | Withdrawal |
|---|---|---|
| Minimum amount | 0.1 TON | 0.5 TON |
| Network fee | Paid by player (normal TON gas) | Deducted from amount |
| Platform fee | None | 0.05 TON flat |
| Processing time | ~30 seconds (3 confirmations) | ~60 seconds |
| Frequency limit | No limit | 1 per minute per user |

---

## 9. Pricing & Economic Balance

### 9.1 Economy Design Principles

- **Bounded production:** 2-miner cap means max output is always calculable and finite
- **Gold is always useful:** upgrades provide permanent value — gold never becomes worthless
- **TON is the exit currency:** gold cannot be directly converted to TON; it must go through miners → marketplace
- **Scarcity through merging:** Rare+ miners require destroying many lower-tier miners, limiting supply
- **Free-to-play viable:** patient players can reach Rare without spending TON (via merging)

### 9.2 Economy Rebalance Log

> **⚠️ Critical fix applied in v1.1**
>
> The original parameters (`UPG_MULT = 1.55`, `Common L1 = 10 g/hr`) resulted in **~400 days to reach Uncommon L1** — a completely broken new-player experience. Root cause: at ×1.55, the L19→L20 upgrade alone cost 400,002 gold (57 days of a single L19 miner's production).

| Parameter | Old value | New value | Effect |
|---|---|---|---|
| Common L1 gold/hr | 10 | **20** | First week feels rewarding |
| Upgrade cost growth | ×1.55/level | **×1.35/level** | L19→L20 costs 33K not 400K |
| Time to Uncommon L1 | ~400 days | **~33 days** | Healthy 1-month milestone |
| Common total upg cost | 1,127,005 gold | **127,914 gold** | Achievable, not a wall |

### 9.3 Gold-to-TON Soft Peg

```
Reference rate: 1,000 gold ≈ 0.1 TON  (soft peg, market-driven)
```

Gold has no direct exchange rate to TON. It is valued indirectly through miner production value. This peg is not enforced on-chain — it is a design guideline.

### 9.4 Full Economy Numbers Reference

| Parameter | Value |
|---|---|
| Active miner slots | 2 (fixed, permanent) |
| Starting miners | 1× Common L1 (free) |
| Gold/hr growth per level | ×1.17 (17% per level) |
| Upgrade cost growth per level | ×1.35 (35% per level) |
| Common L1 gold/hr | 20 |
| Common L20 gold/hr | 395 |
| Rare L1 gold/hr | 320 |
| Legendary L20 gold/hr | 101,112 |
| Common L1→L20 total cost | 127,914 gold |
| Legendary L1→L20 total cost | 32,745,536 gold |
| Max 2-miner gold/hr (2× Legendary L20) | 202,224 g/hr |
| Starter shop: Common L1 price | 5,000 Gold (no TON) |
| Marketplace pricing | Fully user-defined — seller sets any price |
| Marketplace fee | 5% of sale |
| Idle accumulation cap | 48 hours per miner |
| Withdrawal minimum | 0.5 TON |
| Withdrawal platform fee | 0.05 TON flat |
| Gold-to-TON soft peg | 1,000 gold ≈ 0.1 TON |
| **Time to Uncommon L1 (F2P)** | **~33 days from new player** |
| **Time to first Rare L1 (F2P)** | **~99 days (~3 months)** |
| 2nd Common miner available | ~Day 11 (after saving 5,000 gold) |

### 9.5 Free-to-Play vs Pay-to-Skip

| Goal | F2P time | Pay shortcut | Cost |
|---|---|---|---|
| Get 2nd miner | Save 5,000 gold (~day 11) | Buy from Starter Shop | 5,000 Gold |
| Get Rare miner | ~3 months (merge path) | Buy on marketplace | Seller's chosen price |
| Get Epic miner | ~8 months (merge path) | Buy on marketplace | Seller's chosen price |
| Get Legendary | ~18 months (merge path) | Buy on marketplace | Seller's chosen price |

---

## 10. Player Progression

### 10.1 Progression Timeline

| Timeline | Milestone | State | Earnings |
|---|---|---|---|
| Day 1 | Register | 1× Common L1 | 0 |
| Day 4 | Common L5 | 37 g/hr | 0 |
| Day 11 | Buy 2nd miner | 2× Common mining | 0 |
| Day 20 | Both miners ~L12 | ~300 g/hr combined | 0 |
| **Day 33** | **First merge → Uncommon L1** | 80 g/hr | 0 |
| Month 2–3 | Building toward Rare | Multiple Uncommons | 0 |
| **~Day 99** | **First Rare L1** | 320 g/hr | **List on marketplace for TON** |
| Month 6 | Multiple Rares | Rare + other active | 6–15 TON/month |
| Month 12+ | Epic/Legendary | High-rarity active | 30–100+ TON/month |

### 10.2 Referral System

- Each player gets a unique referral code on registration
- Share code via Telegram → friend clicks link → opens Mini App with referral pre-filled
- **Referrer reward:** 500 gold + 10% of referred player's first Common miner shop purchase (in gold)
- **Referred player reward:** 200 gold bonus on registration
- Referral chain: tracked 1 level deep (not multi-level)
- Referral rewards credited after referred player's first login + 24 hours

### 10.3 Leaderboards

- **Global leaderboard:** ranked by total gold ever mined (lifetime)
- **Weekly leaderboard:** gold mined in the last 7 days
- **Marketplace leaderboard:** highest TON earned from sales

---

## 11. MongoDB Database Schema

### 11.1 Collection Overview

| Collection | Purpose | Key fields | Volume |
|---|---|---|---|
| `users` | Player accounts | telegramId, tonBalance, goldBalance | 1 per player |
| `miner_templates` | Miner type catalog | rarity, baseGoldPerHour, levelMultiplier | 5 docs (static) |
| `user_miners` | Individual miner instances | ownerId, level, isActive, isListed | ~5–10 per player |
| `marketplace_listings` | Active & historical listings | itemType, priceInTon, status, sellerId | High volume |
| `transactions` | Financial audit trail | userId, type, amount, status | High volume |
| `ton_deposits` | Inbound TON payments | txHash, amountNanoTon, status | Per deposit |
| `withdrawals` | Outbound TON requests | userId, netAmountNanoTon, status | Per withdrawal |
| `referrals` | Invite tracking | referrerId, referredUserId, rewardStatus | 1 per new user |
| `starter_shop_purchases` | Common miner gold purchases | userId, goldSpent, minerId | Per purchase |

### 11.2 Schema: `users`

```js
{
  _id:               ObjectId,
  telegramId:        Number,       // unique index
  username:          String,       // nullable
  firstName:         String,
  lastName:          String,
  avatarUrl:         String,
  tonWalletAddress:  String,       // TonConnect, sparse unique
  goldBalance:       Decimal128,
  tonBalance:        String,       // nanoTON string
  lastGoldCollected: Date,
  totalGoldEarned:   Decimal128,   // for leaderboard
  totalTonEarned:    String,
  referredBy:        ObjectId,
  referralCode:      String,       // unique, 6-char
  level:             Number,
  isPremium:         Boolean,
  isBanned:          Boolean,
  createdAt:         Date,
  updatedAt:         Date
}
// Indexes:
// { telegramId: 1 }           — unique
// { tonWalletAddress: 1 }     — sparse unique
// { referralCode: 1 }         — unique
// { totalGoldEarned: -1 }     — leaderboard
```

### 11.3 Schema: `user_miners`

```js
{
  _id:               ObjectId,
  ownerId:           ObjectId,     // ref users._id
  templateId:        ObjectId,     // ref miner_templates._id
  rarity:            String,       // Common|Uncommon|Rare|Epic|Legendary
  level:             Number,       // 1–20
  isActive:          Boolean,      // in active slot (max 2 per user)
  slotIndex:         Number,       // 0 or 1
  currentGoldPerHour:Number,       // denormalized
  accumulatedGold:   Decimal128,   // capped at 48hr
  lastMiningTick:    Date,
  isListed:          Boolean,      // on marketplace (Rare+ only)
  listingId:         ObjectId,
  canBeListed:       Boolean,      // rarity >= Rare
  acquiredFrom:      String,       // starter|starter_shop|marketplace|merge
  upgradeHistory:    Array,        // [{fromLevel,toLevel,goldSpent,timestamp}]
  mergedIntoId:      ObjectId,     // if destroyed in merge
  createdAt:         Date
}
// Indexes:
// { ownerId: 1, isActive: 1 }
// { ownerId: 1, isListed: 1 }
// { lastMiningTick: 1 }        — mining cron batch
```

### 11.4 Schema: `marketplace_listings`

```js
{
  _id:             ObjectId,
  sellerId:        ObjectId,
  itemType:        String,    // "miner" only (no gold bundles)
  minerId:         ObjectId,
  minerRarity:     String,    // denormalized, always Rare|Epic|Legendary
  minerLevel:      Number,
  goldPerHour:     Number,    // at time of listing
  priceInTon:      String,    // nanoTON — seller-defined, any value
  status:          String,    // active|sold|cancelled|expired
  buyerId:         ObjectId,
  soldAt:          Date,
  transactionId:   ObjectId,
  tonTransactionHash: String,
  platformFeeTon:  String,    // 5% of priceInTon
  expiresAt:       Date,      // 7 days from listing
  viewCount:       Number,
  createdAt:       Date,
  updatedAt:       Date
}
// Indexes:
// { status: 1, minerRarity: 1, priceInTon: 1 }
// { sellerId: 1, status: 1 }
// { expiresAt: 1, status: 1 }  — TTL cleanup job
```

### 11.5 Schema: `transactions`

```js
{
  _id:           ObjectId,
  userId:        ObjectId,
  type:          String,  // deposit|withdraw|marketplace_buy|marketplace_sell
                          // |upgrade_gold|referral_reward|shop_purchase
  direction:     String,  // credit|debit
  currency:      String,  // TON|GOLD
  amount:        String,
  balanceBefore: String,
  balanceAfter:  String,
  status:        String,  // pending|completed|failed|refunded
  tonTxHash:     String,
  listingId:     ObjectId,
  minerId:       ObjectId,
  relatedUserId: ObjectId,
  metadata:      Object,  // {fromLevel, toLevel, minerName, ...}
  failureReason: String,
  processedAt:   Date,
  createdAt:     Date
}
// Indexes:
// { userId: 1, createdAt: -1 }
// { tonTxHash: 1 }  — unique sparse
```

### 11.6 Schema: `ton_deposits`

```js
{
  _id:             ObjectId,
  userId:          ObjectId,
  fromWallet:      String,
  toWallet:        String,
  amountNanoTon:   String,
  txHash:          String,  // unique
  blockId:         String,
  memo:            String,
  status:          String,  // detected|confirmed|credited|failed
  confirmations:   Number,
  creditedAt:      Date,
  detectedAt:      Date,
  createdAt:       Date
}
```

### 11.7 Schema: `withdrawals`

```js
{
  _id:                   ObjectId,
  userId:                ObjectId,
  toWallet:              String,
  requestedAmountNanoTon:String,
  feeNanoTon:            String,
  netAmountNanoTon:      String,
  status:                String,  // pending|approved|processing|completed|failed|cancelled
  txHash:                String,
  failureReason:         String,
  completedAt:           Date,
  createdAt:             Date,
  updatedAt:             Date
}
```

---

## 12. API Architecture

### 12.1 Authentication

```
POST   /api/auth/telegram          — Validate Telegram initData → return JWT
POST   /api/auth/wallet/connect    — Link TON wallet (TonConnect signature)
DELETE /api/auth/wallet/disconnect — Unlink TON wallet
```

### 12.2 Miners

```
GET    /api/miners                 — All user miners + pending gold per miner
GET    /api/miners/active          — Only active slot miners
POST   /api/miners/:id/collect     — Collect accumulated gold from miner
POST   /api/miners/:id/upgrade     — Upgrade miner level (costs gold)
POST   /api/miners/:id/activate    — Move miner to active slot (max 2)
POST   /api/miners/:id/deactivate  — Move miner to inventory
POST   /api/miners/merge           — Merge 2 miners {miner1Id, miner2Id}
GET    /api/miners/templates       — Miner type catalog
```

### 12.3 Marketplace

```
GET    /api/market/listings        — Browse (filter: rarity, minLevel, maxPrice, sort)
POST   /api/market/list            — List a Rare+ miner {minerId, priceInTon} — any price
DELETE /api/market/listings/:id    — Cancel listing (returns miner to inventory)
POST   /api/market/buy/:id         — Purchase a listing (initiates TonConnect flow)
GET    /api/market/my-listings     — Seller's active and historical listings
```

### 12.4 Starter Shop

```
GET    /api/shop/items             — Available shop items (Common L1, price in Gold)
POST   /api/shop/buy               — Purchase Common miner (deducts Gold from balance)
```

### 12.5 Wallet

```
GET    /api/wallet/balance         — TON + gold balances
POST   /api/wallet/topup/init      — Create deposit intent → payment address + memo
POST   /api/wallet/withdraw        — Submit withdrawal request
GET    /api/wallet/transactions    — Paginated transaction history
```

### 12.6 User

```
GET    /api/user/me                — Profile + stats
GET    /api/user/leaderboard       — Global + weekly rankings
GET    /api/user/referrals         — Referral list + total earned
```

### 12.7 Webhooks (Internal)

```
POST   /webhooks/ton/deposit       — TON Center deposit notification
```

---

## 13. Security & Anti-Cheat

### 13.1 Anti-Cheat Rules

- Gold calculation is **100% server-side** — clients never submit gold amounts
- Client only sends "collect" trigger — server calculates how much is due
- 48-hour accumulation cap prevents exploit via time manipulation
- Miner slot enforcement: server validates active count ≤ 2 on every request
- Marketplace listing validation: server checks rarity ≥ Rare before allowing listing
- TON deposits verified on-chain before crediting — never trust client-reported amounts
- Withdrawal deduplication: `txHash` unique index prevents double-spend
- Rate limiting: 60 req/min per user on standard routes, 1/min on withdrawals

### 13.2 TON Transaction Security

- Deposits: verified via TON Center webhook + on-chain confirmation (3 blocks)
- Withdrawals: signed server-side — private key never exposed to client
- Escrow: marketplace TON held in smart contract until both parties confirm
- Wallet validation: TON address checksum verified before any payment
- Duplicate deposit protection: `txHash` indexed as unique — webhook idempotent

---

## 14. UI/UX Design Guidelines

### 14.1 Screen Map

| Screen | Tab | Key elements |
|---|---|---|
| Main / Mine | MINE | Mine background, 2 active miners animating, gold accumulation bar, collect button, menu buttons (Task, Miner, Recruit, Ranking, Storage, Setting) |
| Market | MARKET | Listings grid (Rare+ only), filter by rarity/price, buy button with TON price, seller info, live online count |
| Invite | INVITE | Referral link + share button, referral count, total gold/TON earned |
| Miner Detail | Modal | Miner stats, current level, upgrade button with gold cost |
| Wallet | Modal | TON balance, gold balance, Top Up + Withdraw buttons, transaction history |
| Starter Shop | Modal | Common L1 miner card, 5,000 gold price, buy button (no TonConnect needed) |
| Merge Screen | Modal | Select 2 same-rarity L20 miners, preview result rarity, confirm merge |

### 14.2 Visual Style

- **Theme:** Low-poly 3D isometric outdoor mine environment
- **Color palette:** Warm golds, sandy terrain, lush greenery accents
- **Miners:** Cute animal characters (e.g. Shiba Inu) with mining hats and pickaxes
- **Animations:** Miners swing pickaxe in idle loop, gold coins float up when collected
- **Bottom navigation:** 3 tabs — MINE, MARKET, INVITE
- **Top bar:** Gold balance, gem/diamond balance, reload button, G-token balance

---

## 15. Roadmap & Future Features

### Phase 1 — MVP Launch
- Core idle mining with Common miners
- Upgrade system (L1–L20)
- Starter Shop (Common L1 miners purchasable with Gold)
- Merge system (Common → Uncommon → Rare)
- TON deposit and withdrawal
- Marketplace (Rare+ miners only, user-set prices)
- Basic leaderboard
- Telegram referral system

### Phase 2 — Engagement Layer
- Daily tasks and quests
- Seasonal events with limited-edition miners
- Push notifications via Telegram bot (gold ready, upgrade complete)
- Friends list — see friends' active miners
- Epic and Legendary merge paths enabled

### Phase 3 — Economy Expansion
- Mining expeditions — send inactive miners on timed missions for bonus gold
- Guild/clan system — pooled gold for group upgrades
- In-game tournaments — compete for prize pool in TON
- Miner skins and cosmetics (cosmetic NFTs on TON chain)

### Phase 4 — Full Web3
- Miners as on-chain NFTs (TON NFT standard)
- Decentralized marketplace (smart contract escrow fully on-chain)
- DAO governance for economy parameters
- Cross-game miner portability

---

## 16. WebSocket Integration — Real-Time Data

### 16.1 Why WebSockets

Gold Miner requires live data for several core features. Without real-time updates, players would see stale marketplace listings, missed gold accumulation ticks, and delayed balance changes after TON transactions.

| Feature | Without WebSocket | With WebSocket |
|---|---|---|
| Marketplace listings | Stale — player sees sold items | Listing sold/added in real time |
| Gold balance | Only updates on page refresh | Live counter — ticks up every second |
| TON deposit | Player must manually refresh | Balance credited instantly on-screen |
| Withdrawal status | Unknown until next session | Status updates live: pending → completed |
| Miner sale notification | Seller never knows in-session | Instant toast: "Your Rare L15 sold for 12 TON" |
| Online player count | Not available | Live count shown in marketplace |

### 16.2 Technology Stack

- **Server:** Socket.IO 4.x on Node.js — layered on top of the existing Express server
- **Client:** Socket.IO client in the React Telegram Mini App
- **Auth:** JWT token passed as handshake auth — same token as REST API
- **Rooms:** Each user joins a private room `user:{userId}` + shared rooms (`marketplace`, `leaderboard`)
- **Scaling:** Socket.IO Redis adapter — allows horizontal scaling across multiple Node.js instances
- **Reconnect:** Automatic reconnect with exponential backoff — Telegram Mini Apps frequently background/foreground

### 16.3 Connection Lifecycle

1. Client opens Telegram Mini App → React app initializes
2. Socket.IO client connects: `socket.connect()` with `auth: { token: jwt }`
3. Server validates JWT → identifies user → joins user to private room `user:{userId}`
4. Server joins user to shared rooms: `marketplace`, `leaderboard`
5. Server emits initial state snapshot: gold balance, active miners, online count
6. Client receives live events while app is open (foreground)
7. Telegram backgrounds the app → socket auto-disconnects after 30s timeout
8. Player reopens app → reconnects → server sends fresh state snapshot

> **Telegram Mini App Note**
>
> Telegram may suspend the WebView when the app is backgrounded. Always implement reconnect logic — assume the connection is lost whenever the app returns to foreground. Use `document.addEventListener("visibilitychange")` to trigger reconnect. On reconnect, always request a full state snapshot — do not rely on the last received state.

### 16.4 Rooms & Namespaces

| Room | Who joins | Purpose |
|---|---|---|
| `user:{userId}` | That user only | Private events: gold tick, balance change, withdrawal status, miner sold, deposit confirmed |
| `marketplace` | All online users | Live listing feed: new listing added, sold, cancelled |
| `leaderboard` | All online users | Live rank updates — top 100 refresh every 60 seconds |
| `admin` | Admin users only | System alerts, pending withdrawals count, error monitoring |

### 16.5 Full Event Reference

#### Server → Client events

| Event | Room | Payload |
|---|---|---|
| `gold:tick` | `user:{id}` | `{ minerId, goldAdded, newBalance, accumulatedGold }` |
| `balance:update` | `user:{id}` | `{ goldBalance, tonBalance, reason }` — reason: `deposit\|withdrawal\|purchase\|sale\|upgrade\|merge` |
| `miner:upgraded` | `user:{id}` | `{ minerId, fromLevel, toLevel, newGoldPerHour, goldBalance }` |
| `miner:sold` | `user:{id}` | `{ listingId, minerId, soldFor, tonBalance, buyerName }` |
| `miner:received` | `user:{id}` | `{ minerId, rarity, level, goldPerHour, fromSeller }` |
| `merge:complete` | `user:{id}` | `{ newMinerId, newRarity, consumedMiner1Id, consumedMiner2Id }` |
| `deposit:confirmed` | `user:{id}` | `{ amountNanoTon, tonBalance, txHash }` |
| `withdrawal:update` | `user:{id}` | `{ withdrawalId, status, txHash, failureReason }` |
| `market:listing_added` | `marketplace` | `{ listingId, rarity, level, priceInTon, sellerName, goldPerHour }` |
| `market:listing_sold` | `marketplace` | `{ listingId }` |
| `market:listing_cancelled` | `marketplace` | `{ listingId }` |
| `leaderboard:update` | `leaderboard` | `{ top100: [{ rank, userId, username, totalGold }] }` |
| `online:count` | `marketplace` | `{ count }` |
| `state:snapshot` | `user:{id}` | `{ goldBalance, tonBalance, activeMiners[], pendingWithdrawals[] }` |

#### Client → Server events

| Event | Payload | Description |
|---|---|---|
| `gold:collect` | `{ minerId }` | Player taps collect. Server calculates gold, credits balance, emits `balance:update` back |
| `market:subscribe` | `{ filters: { rarity, minLevel, maxPrice } }` | Client tells server which filters are active — only matching `listing_added` events are pushed |
| `ping` | `{}` | Keepalive — sent every 25 seconds by client |

### 16.6 Gold Tick Architecture

Gold accumulation is calculated lazily (on demand) for REST endpoints, but for online players the server pushes a live tick every **60 seconds** so the UI can animate the gold counter smoothly.

**Tick flow:**
1. Server has a BullMQ repeating job that runs every 60 seconds
2. Job queries all `user_miners` where `isActive: true`
3. For each miner, calculates gold earned since `lastMiningTick`
4. Updates `accumulatedGold` and `lastMiningTick` in MongoDB
5. Checks if owner is currently connected (via Socket.IO room presence)
6. **If online:** emits `gold:tick` to `user:{userId}` room
7. **If offline:** skips emit — gold is stored in DB, user gets it on next session
8. Client-side: animates gold counter smoothly using linear interpolation between ticks

### 16.7 Marketplace Real-Time Flow

1. **Seller lists miner:** REST `POST /api/market/list` → creates listing in DB → server emits `market:listing_added` to all `marketplace` room members
2. **Buyer browses:** receives `market:listing_added` events in real time — no polling needed
3. **Buyer purchases:** REST `POST /api/market/buy/:id` → atomic DB transaction → emits `market:listing_sold` to `marketplace` room
4. **All other browsers:** receive `market:listing_sold` → card disappears from grid instantly
5. **Seller:** receives `miner:sold` in private room → toast notification
6. **Buyer:** receives `miner:received` in private room → miner appears in inventory

> **Race Condition Prevention**
>
> Two users clicking "Buy" on the same listing simultaneously is handled at the DB level. MongoDB `findOneAndUpdate` with `{ status: "active" }` filter — only one write can succeed. The second buyer receives HTTP `409 Conflict`. The marketplace room then receives `market:listing_sold` regardless of which buyer won. **Never rely on WebSocket ordering alone for purchase atomicity — always use DB transactions.**

### 16.8 Server-Side Implementation

```typescript
import { Server } from 'socket.io';
import { createAdapter } from '@socket.io/redis-adapter';
import { createClient } from 'redis';

const io = new Server(httpServer, {
  cors: { origin: 'https://your-telegram-mini-app.com' },
  transports: ['websocket'],    // WebSocket only — no long-polling
  pingInterval: 25000,          // 25s keepalive
  pingTimeout: 10000,           // 10s timeout
});

// Redis adapter for multi-instance scaling
const pubClient = createClient({ url: process.env.REDIS_URL });
const subClient = pubClient.duplicate();
io.adapter(createAdapter(pubClient, subClient));

// Auth middleware — validate JWT on every handshake
io.use(async (socket, next) => {
  const token = socket.handshake.auth.token;
  const user = await verifyJWT(token);
  if (!user) return next(new Error('Unauthorized'));
  socket.userId = user.id;
  next();
});

io.on('connection', async (socket) => {
  // Join private user room + shared rooms
  socket.join(`user:${socket.userId}`);
  socket.join('marketplace');
  socket.join('leaderboard');

  // Send initial state snapshot
  const snapshot = await buildStateSnapshot(socket.userId);
  socket.emit('state:snapshot', snapshot);

  // Handle gold collect
  socket.on('gold:collect', async ({ minerId }) => {
    const result = await collectGold(socket.userId, minerId);
    socket.emit('balance:update', {
      goldBalance: result.newBalance,
      reason: 'collect'
    });
  });

  // Handle marketplace filter subscription
  socket.on('market:subscribe', ({ filters }) => {
    socket.data.marketFilters = filters;
  });

  socket.on('disconnect', () => { /* cleanup */ });
});
```

#### Emitting from REST controllers

```typescript
// Inside POST /api/market/list controller:
const listing = await createListing(userId, minerId, priceInTon);
io.to('marketplace').emit('market:listing_added', {
  listingId: listing._id,
  rarity:    listing.rarity,
  level:     listing.level,
  priceInTon:listing.priceInTon,
  sellerName:listing.sellerName,
  goldPerHour: listing.goldPerHour,
});

// Inside POST /api/market/buy/:id controller:
io.to('marketplace').emit('market:listing_sold', { listingId });
io.to(`user:${sellerId}`).emit('miner:sold', { listingId, soldFor, tonBalance });
io.to(`user:${buyerId}`).emit('miner:received', { minerId, rarity, level });
```

### 16.9 Client-Side Implementation

```typescript
import { useEffect, useRef } from 'react';
import { io, Socket } from 'socket.io-client';
import { useGameStore } from '@/store';

export function useGameSocket(jwt: string) {
  const socket = useRef<Socket | null>(null);
  const store = useGameStore();

  useEffect(() => {
    socket.current = io(process.env.NEXT_PUBLIC_WS_URL, {
      auth: { token: jwt },
      transports: ['websocket'],
      reconnectionDelay: 1000,
      reconnectionDelayMax: 10000,
    });

    const s = socket.current;

    s.on('state:snapshot',          store.applySnapshot);
    s.on('gold:tick',               store.applyGoldTick);
    s.on('balance:update',          store.applyBalanceUpdate);
    s.on('miner:upgraded',          store.applyMinerUpgrade);
    s.on('miner:sold',              store.applyMinerSold);
    s.on('miner:received',          store.applyMinerReceived);
    s.on('merge:complete',          store.applyMergeComplete);
    s.on('deposit:confirmed',       store.applyDepositConfirmed);
    s.on('withdrawal:update',       store.applyWithdrawalUpdate);
    s.on('market:listing_added',    store.applyListingAdded);
    s.on('market:listing_sold',     store.applyListingSold);
    s.on('market:listing_cancelled',store.applyListingCancelled);
    s.on('online:count',            store.applyOnlineCount);

    // Reconnect when Telegram brings app back to foreground
    document.addEventListener('visibilitychange', () => {
      if (!document.hidden && !s.connected) s.connect();
    });

    return () => { s.disconnect(); };
  }, [jwt]);

  return socket;
}
```

### 16.10 UI Components Using Real-Time Data

| UI Component | Event(s) consumed | Behaviour |
|---|---|---|
| Gold counter (top bar) | `gold:tick` | Animates counter upward every tick using linear interpolation |
| TON balance (top bar) | `balance:update` | Flashes green and increments on deposit or sale |
| Miner card | `gold:tick`, `miner:upgraded` | Shows accumulated gold bar filling. Level badge updates on upgrade |
| Marketplace grid | `market:listing_added`, `market:listing_sold`, `market:listing_cancelled` | Cards slide in/out in real time. "SOLD" stamp overlays for 2s before removal |
| Online players badge | `online:count` | "247 online" shown in marketplace header, updates every 30s |
| Withdrawal row | `withdrawal:update` | Status pill: Pending → Processing → Completed ✅ or Failed ❌ |
| Toast notifications | `miner:sold`, `deposit:confirmed`, `miner:received` | "Miner sold for 12 TON", "1.5 TON deposited", "Rare L10 added to inventory" |
| Leaderboard list | `leaderboard:update` | Ranks refresh every 60s. Row highlights when your rank changes |

### 16.11 Security Considerations for WebSockets

- JWT validated on every handshake — no anonymous connections allowed
- Users can only join their own private room — server enforces this server-side
- All game mutations (upgrade, merge, buy) go through REST API — WebSocket is **read-only** from the client side, except for `gold:collect` and `market:subscribe`
- Rate limiting on `gold:collect`: max 1 per miner per 30 seconds server-side
- Socket.IO rooms are server-managed — client cannot emit to `marketplace` room directly
- Redis pub/sub used for cross-instance event propagation
- WebSocket traffic monitored via Socket.IO admin UI (admin room) — detects abnormal event frequency

---

*Gold Miner · Complete Game Design Documentation · Version 1.1 · March 2026*
*This document is confidential and intended for the development team only.*
