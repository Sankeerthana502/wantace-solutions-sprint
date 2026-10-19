# PART B: Business Requirement Document (BRD)
## Client: Vardhan Steelware (Jagadhri, Haryana)
**Author:** Solutions Engineering | **Target Audience:** Engineering & Product Teams  
**Constraint:** Within 4 Pages | **Weight:** 45%

---

## 1. The Problem in the Client's Words (Executive Summary)

Vardhan Steelware manufactures stainless steel utensils (kadhais, patilas, thalis, glasses, dinner sets) in Jagadhri, Haryana, employing ~70 workers and supplying ~40 North Indian distributors alongside nascent online sales.

* **Suresh Vardhan (58, Owner & Cheque Signer):**  
  > *"Problem orders ki nahi hai. Problem yeh hai ki pata nahi chalta maal kahan atka hai. Customer phone karta hai toh mujhe Ramesh ko phone karna padta hai. Ramesh ki diary mein sab hai... Mujhe bas phone pe dekhna hai kaunsa order kahan hai. Pehle wale mein paisa doob gaya [₹1.2L ERP failed in 2023]... Diwali se pehle chahiye, season hai."*
* **Kunal Vardhan (30, Son, MBA, Order Coordinator):**  
  > *"What we really need is a proper modern system. Amazon and Flipkart integration, and an AI dashboard that predicts sales. Online is growing 30% every month... And it should look modern. I want to show it to our distributors."*

**Core Diagnosis:** Suresh lacks real-time mobile visibility into order stages, forcing constant phone friction with Ramesh. Meanwhile, Kunal is chasing high-complexity features (AI forecasting, marketplace APIs) while the fundamental order tracking, credit control, and contractor loop remain completely chaotic.

---

## 2. What the Evidence Shows: Forensic Analysis of Order Sheet & WhatsApp Data

An analysis of Kunal’s Google Sheet (1–7 September 2026, 25 order lines) and the "Vardhan Dispatch" WhatsApp export reveals stark discrepancies between client perceptions and operational reality:

### Agreement & Disagreement Matrix (What Client Said vs. What Data Proves)

| Topic | What the Client Said | What the Evidence Actually Proves | Classification |
|:---|:---|:---|:---:|
| **Order Volume** | Suresh: *"Mahine ke 60–70 order honge."* Kunal: *"Usse zyada hain."* | **25 orders logged in just 7 days** (1–7 Sep). At this run-rate, monthly volume is **100–110 orders**. Kunal was right; Suresh underestimates order velocity by 40–50%. | **[OBSERVED]** |
| **Payment & Credit** | Suresh: *"Payment koi problem nahi. Purani parties hain."* | **Severe credit leakage.** Sharma Bartan Bhandar (Karnal) owes **₹1.8 Lakh in old dues**. Two orders held on 3 Sep and 7 Sep. Sharma pressured on 05/09 (*"Diwali ka maal hai"*), causing internal confusion between Ramesh and Kunal (*"Papa se pucho"*). | **[OBSERVED]** & **[REPORTED]** |
| **Ramesh’s "All-Knowing Diary"** | Suresh: *"Ramesh ki diary mein sab hai."* | **Ramesh himself is confused.** On 07/09, Ramesh asks WhatsApp: *"Bhagona sheet mein do jagah likha hai? Gupta ka 40 aur New Delhi ka 60, dono polish mein hain na"*. Orders are not synchronized. | **[OBSERVED]** |
| **Steel Wastage & Polishing** | Suresh: *"Thoda idhar udhar hota hai... wastage hai."* (Kunal noted 3–4% annual steel gap). | **Material shrinkage is unmonitored leakage, not scrap.** Karigar Pappu took **420 kg**, returned **1,180 pcs**, with **6 kg shortage** flagged by Munshi ji (*"Pappu ka 6 kg short. Next lot mein adjust"*). At scale, 3–4% unaccounted steel is hundreds of kg bleeding monthly. | **[OBSERVED]** & **[INFERRED]** |
| **Online Channel Focus** | Kunal: *"Online is growing 30%... need Amazon/Flipkart sync & AI."* | **Online is only 3 out of 25 orders** (12% of order count, ~5% revenue). 88% of orders are from regional B2B wholesale distributors (Ambala, Panipat, Rohtak, Karnal, Sonipat, Jalandhar). | **[OBSERVED]** |

### Specific Data Inconsistencies & Operational Anomalies (1–7 Sep 2026)
1. **Duplicate / Repeat Entries:**
   * *Gupta Traders Ambala (01-09-26):* 200 pcs Kadai No.10 dispatched; identical 200 pcs pending with remark `"repeat msg?"`.
   * *Jain Crockery Panipat (04-09-26):* 150 pcs Kadhai No 10 pending with remark `"again sent on whatsapp"` (duplicate of 02-09-26 entry).
2. **Missing Master Data:**
   * *Order with Blank Party (03-09-26):* `3 Sep,,Thali 12 inch,20,dozen,600,dispatched,` — No customer name recorded; goods dispatched untracked.
   * *Unit & Pricing Disconnect (06-09-26):* Verma Utensils Sonipat order entered as `180 kg` (instead of pieces), with rate blank and remark `"rate confirm karna hai"`.
3. **Improper Return Logging:**
   * *Negative Quantity Entry (05-09-26):* Sharma Bartan recorded with `-6 dozen` Gilas 300ml (`return, dent`). Returns are logged into the active sales sheet rather than an isolated credit/RMA log.
4. **Unrecorded Partial Shipments:**
   * *Mehra Kitchenware Jalandhar (04-09-26):* 300 pcs Kadai No.10 split: 180 pcs dispatched, 120 pending (`"polish ke baad"`), completed on 06-09-26 (`"baaki wala"`). Done manually via free-text remarks.

---

## 3. As-Is Process Map (Current Operational Journey)

```
[ Customer Order Intake ]
  • Phone call to Suresh OR WhatsApp message to Kunal.
  • Kunal manually types into Google Sheet (no validation, blank names, kg vs pcs errors).
         │
         ▼
[ Shop Floor & Ramesh's Diary ]
  • Ramesh checks sheet/diary. No credit check performed.
  • Production starts; items batched for outsourced polishing.
         │
         ▼
[ Outsourced Polishing (4-5 Karigars, e.g., Pappu) ]
  • Raw pressed utensils sent by weight (Gross kg on gate slip).
  • Finished utensils return counted by pieces (e.g., 420 kg out ──> 1,180 pcs in).
  • Munshi ji calculates weight variance the next day; shortages (e.g., 6 kg) pushed to "next lot".
         │
         ▼
[ Packing, Credit Hold Check & Dispatch ]
  • Ramesh packs goods. If Kunal remembers, he flags credit hold on WhatsApp (e.g., Sharma 1.8L dues).
  • Partial dispatches tracked via handwritten notes (e.g., Mehra 180 sent, 120 pending).
         │
         ▼
[ Delivery & Billing ]
  • Munshi ji enters final GST bill in Tally days after goods leave. Suresh calls Ramesh to verify status.
```

---

## 4. Phase 1 Scope vs. Strategic Exclusions

To satisfy Suresh's non-negotiable deadline—**"Diwali se pehle chahiye, season hai"** (October call, mid-November Diwali = 3–4 weeks execution window)—and prevent repeating the 2023 ERP failure, Phase 1 strictly focuses on core visibility:

```
+---------------------------------------------------------------------------------------+
| INCLUDED IN PHASE 1 (Must-Have for Diwali)   | EXCLUDED FROM PHASE 1 (Phase 2 Roadmap)|
+---------------------------------------------------------------------------------------+
| 1. Centralized Order Register: Auto unique   | 1. AI Sales Forecasting Engine (Kunal)  |
|    Order ID (VS-ORD-XXXX); mandatory party,  |    Rationale: Historical sheet data is  |
|    SKU dropdown, rate, locked UoM (Pieces).  |    dirty; AI will predict garbage.     |
| 2. Order Lifecycle Status: Intake, In-Prod,  | 2. Amazon & Flipkart API Sync (Kunal)   |
|    In-Polish, Ready, Dispatched, On-Hold.    |    Rationale: Online is only 5% volume; |
| 3. Credit Gate (Munshi ji): 1-click hold for |    distributors drive 95% of revenue.  |
|    overdue dues (blocks dispatch chit).      | 3. Automated Tally 2-Way Sync           |
| 4. Outsourced Polishing Reconciliation: Log  |    Rationale: Munshi ji continues Tally;|
|    kg sent, pcs received, auto-shortage flag.|    we avoid ERP integration delays.     |
| 5. Partial Dispatch & RMA Return Module:     | 4. Contractor Mobile App/Portal         |
|    Dedicated balance tracking & return logs. |    Rationale: Pappu & karigars won't use|
| 6. Suresh Mobile Status PWA: 1-click view    |    software; Ramesh logs gate scale.    |
|    by party, stage, or delay status.         | 5. Complex Business Intelligence Cubes |
+---------------------------------------------------------------------------------------+
```

---

## 5. Relational Data Architecture

```
+--------------------------+          +--------------------------+
|         CUSTOMER         |          |       ORDER_HEADER       |
+--------------------------+          +--------------------------+
| customer_id (PK)         |1        N| order_id (PK) [VS-ORD-X] |
| company_name             +----------+ customer_id (FK)         |
| station (Ambala/Karnal)  |          | order_date               |
| credit_status (Clear/Hold|          | current_stage            |
| overdue_balance_inr      |          | created_by (Kunal)       |
+--------------------------+          +------------+-------------+
                                                   | 1
                                                   | N
+--------------------------+          +------------+-------------+
|     DISPATCH_RECORD      |          |     ORDER_LINE_ITEM      |
+--------------------------+          +--------------------------+
| dispatch_id (PK)         |1        N| line_item_id (PK)        |
| order_id (FK)            +----------+ order_id (FK)            |
| dispatch_date            |          | sku_id (Kadai/Patila/etc)|
| shipped_qty_pcs          |          | ordered_qty_pcs          |
| balance_qty_pcs          |          | dispatched_qty_pcs       |
| dispatched_by (Ramesh)   |          | balance_qty_pcs          |
+--------------------------+          | unit_rate_inr            |
                                      +------------+-------------+
                                                   | N
                                                   | 1
+--------------------------+          +------------+-------------+
|    SALES_RETURN_LOG      |          |     POLISHING_BATCH      |
+--------------------------+          +--------------------------+
| return_id (PK)           |          | batch_id (PK) [POL-XXX]  |
| order_id (FK)            |          | karigar_name (Pappu)     |
| sku_id                   |          | sent_date & gross_wt_kg  |
| returned_qty_pcs         |          | return_date & rec_pcs    |
| reason (Dent/Defect)     |          | expected_wt_kg           |
+--------------------------+          | shortage_kg & status     |
                                      +--------------------------+
```

---

## 6. Users: Role-Based Mobile & Desktop Interfaces

* **Suresh (Owner):**  
  *Device:* Smartphone (Mobile Web / PWA).  
  *Actions:* Opens app 5 times a day. Sees total active orders, orders delayed > 2 days, and orders on credit hold. Can tap any distributor (e.g., "Sharma Karnal") to see live status without calling Ramesh. Holds exclusive PIN to override credit holds.
* **Kunal (Order Intake Lead):**  
  *Device:* Laptop / Tablet in Office.  
  *Actions:* Enters new orders via structured form. Mandatory fields prevent blank customer names and UoM errors. Flags online orders (Amazon/Flipkart) with channel tags.
* **Ramesh (Dispatch In-Charge, Floor):**  
  *Device:* Rugged 10-inch Android tablet at the dispatch desk.  
  *Actions:* Simplified Hindi/Hinglish UI with large buttons:
    * `[ Karigar ko Maal Bheja ]` $\rightarrow$ Select karigar (Pappu), select SKU, enter gross weight (kg).
    * `[ Karigar se Maal Aaya ]` $\rightarrow$ Enter piece count and net weight; system flags shortage.
    * `[ Dispatch Slip Banao ]` $\rightarrow$ Select order, enter pieces loaded, print slip. Blocked if on "Payment Hold".
* **Munshi ji (Accounts):**  
  *Device:* Desktop PC in Accounts Office.  
  *Actions:* Toggles Customer Credit Status (`Approved` / `Hold`); enters payment receipts against overdue balances (e.g., Sharma's 1.8L); audits daily contractor shortage ledger.
* **External Karigars (Pappu & 4 others):**  
  *Access:* *Zero direct software login.* Handled via physical gate pass slips weighed and logged by Ramesh.

---

## 7. The Three Morning Reports Suresh Opens Every Morning (08:00 AM)

### Report 1: Order Stage & Aging Tracker
* **Purpose:** Eliminates Suresh's need to call Ramesh to answer distributor queries.
* **Columns:** `Order ID` | `Distributor & City` | `Item & Qty` | `Current Stage` | `Days at Stage` | `SLA Status`
* **Trigger:** Flags any order stuck in Production > 3 days, Polishing > 2 days, or Dispatch > 24 hours.

### Report 2: Payment & Commercial Block Queue
* **Purpose:** Stops goods from leaving the factory for delinquent accounts while protecting customer relations before Diwali.
* **Columns:** `Order ID` | `Party Name` | `Item & Value` | `Overdue Dues (INR)` | `Hold Reason` | `Suresh Override?`
* **Real Case:** Instantly surfaces Sharma Karnal (₹1.8L dues) so Suresh can decide whether to accept partial payment before releasing Diwali stock.

### Report 3: Outsourced Polishing & Karigar Reconciliation Ledger
* **Purpose:** Solves the 3–4% annual steel leakage by tracking metal sent vs. pieces returned.
* **Columns:** `Batch ID` | `Karigar Name` | `Sent Wt (kg)` | `Recd (pcs)` | `Std Wt (kg)` | `Shortage (kg)` | `Status`
* **Formula:** $$\text{Shortage (kg)} = \text{Gross Sent} - (\text{Pcs Received} \times \text{SKU Std Weight} + \text{Scrap})$$

---

## 8. Ten Discovery Questions for the Next Call (Categorized by Stakeholder)

### For Suresh (Owner)
1. *"Since Diwali is 4 weeks away, are you willing to freeze Phase 1 strictly to Order Tracking and Polishing, launching with 5 core SKUs in 14 days?"*
2. *"Sharma Bartan owes ₹1.8L but wants goods for Diwali—what exact payment percentage (e.g., 50% advance) should unlock a credit-held order?"*
3. *"Do you want Ramesh to have authority to dispatch partial stock (like Mehra's 180 pcs out of 300), or must you approve all partial shipments?"*

### For Kunal (Coordinator)
4. *"In your sheet, Gupta Traders' 200 Kadai order was marked both dispatched and pending with 'repeat msg?'—did Gupta send two orders, or was WhatsApp checked twice?"*
5. *"Can we agree to defer the Amazon/Flipkart automated API integration to January, keeping manual order entry for the 3 online orders a week?"*
6. *"Do you have an exact gram-weight master for each SKU (e.g., Kadai No.10 = 350g, Patila 5pc set = 1,400g) to automate Pappu's shortage math?"*

### For Ramesh (Dispatch Lead)
7. *"When Pappu returned 1,180 pieces from 420 kg with 6 kg missing, does he usually claim burning/buffing loss, or does he deliver rejected pieces later?"*
8. *"When you dispatch goods on mini-tempos, do you create paper challans in duplicate, and can you tap 2 buttons on a tablet screen at the gate scale?"*

### For Munshi ji (Accounts)
9. *"How many total distributors currently have overdue payments over 45 days, and how often do you update payment clearances in Tally?"*
10. *"When Sharma returned 6 dozen dented glasses on 5/9, did you issue a credit note, or just subtract it verbally from their 1.8L balance?"*

---

## 9. The Biggest Project Risk & Actionable Mitigation Plan

### The #1 Risk: Floor Abandonment (Repeating the 2023 ERP Failure)
In 2023, Vardhan spent ₹1.2 lakh on an ERP that collapsed within 90 days because Ramesh (22 years at the company) and the shop-floor staff found desktop forms too complicated and returned to paper diaries. With Diwali season approaching, any software that slows Ramesh down will be discarded immediately.

### Actionable Mitigation Plan: The "Zero-Resistance" Rollout
1. **Ultra-Simple Hindi/Hinglish Tablet Interface:** Ramesh never types free text. The UI consists of large, high-contrast touch tiles: `[ Maal Bheja ]`, `[ Maal Aaya ]`, `[ Dispatch Slip ]`.
2. **Automated Conversion Logic:** Ramesh enters piece counts; the system auto-calculates weight expectations using SKU master tables, eliminating manual math.
3. **Incentive Alignment:** Frame the system to Ramesh as his personal protection shield: logging contractor weights at the gate officially proves he is not responsible for the 6 kg steel shortages.
4. **14-Day Pilot on 3 High-Volume SKUs:** Launch exclusively on Kadai No. 10, Patila 5pc set, and Tope No. 6 to stabilize operations before the Diwali peak.
