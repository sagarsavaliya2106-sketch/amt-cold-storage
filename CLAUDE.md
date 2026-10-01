# AMT Cold Storage — Product Architecture, Operational Blueprint & Client Dossier

**Entity:** AMT Cold Storage ("The Best Choice for Storage Services")  
**Location:** Plot No 30/2/1, Near New Betwa Pull, Village Bhojpura, Dist Niwari (M.P. — Bundelkhand / Jhansi corridor)  
**Owners / Key Personnel:** 
- **Sadik Thekedar** (`+91 73768 31985`, `+91 8299215827`)
- **Shanu Thekedar** (`+91 9450076566`, `+91 7007352167`)
- **Email:** `amtcoldstorage@gmail.com`

---

## 1. Live Deployment & Repository Links

- **GitHub Repository:** [sagarsavaliya2106-sketch/amt-cold-storage](https://github.com/sagarsavaliya2106-sketch/amt-cold-storage)
- **Live Interactive Web Demo (No Login Required):** [https://sagarsavaliya2106-sketch.github.io/amt-cold-storage/](https://sagarsavaliya2106-sketch.github.io/amt-cold-storage/)
- **Live Illustrated Visual Brochure Guide:** [https://sagarsavaliya2106-sketch.github.io/amt-cold-storage/guide.html](https://sagarsavaliya2106-sketch.github.io/amt-cold-storage/guide.html)
- **Desktop PDF Artifacts:**
  - `/Users/sagarsavaliya/Desktop/AMT_Cold_Storage_Visual_Brochure.pdf` (8-Page illustrated brochure with visual callouts and QR codes)
  - `/Users/sagarsavaliya/Desktop/AMT_Cold_Storage_Demo_Guide_and_Proposal.pdf` (Summary proposal and walkthrough)

---

## 2. Core Business Context & Regional Agricultural Dynamics

### Geographic & Mandi Reality (Bundelkhand / Betwa Belt)
1. **Capacity:** ~5,000 Metric Tonnes (MT) = ~1,20,000 standard agricultural bags (bori) across 4 Chambers (Chambers 1 to 4, each with 4 floors and rack subdivisions).
2. **Commodities Stored:**
   - Table Potatoes (*Lal Aloo* / *Safed Aloo*)
   - Seed Potatoes (*Beej Aloo*)
   - Garlic (*Lahsun*) & seasonal spices
3. **Three Operational Cycles:**
   - **Aamad / Inward Rush (Feb – April):** 24/7 non-stop inflow during harvest. Over 1,00,000 bags enter within 45 days. The gate entry system must generate slips in under 30 seconds per vehicle.
   - **Bhandaran & Kisaan Financing (May – Sept):** Preservation at 2°C–4°C and 85%–90% humidity. Cold storage disburses cash advances to farmers against stored bags with automated monthly interest (typically 1.5% per month).
   - **Nikasi & Settlement (Oct – Dec):** Mandi prices rise. Farmers liquidate stock. System auto-calculates Storage Rent (₹120–₹140/bag) + Hamali/Palladari (₹10/bag) + Bardana (₹25/bag) minus Advance & Interest for net cash collection and gate-pass clearance.

---

## 3. Product Architecture (Web-Only, Zero APK)

The system is engineered as a **unified, responsive Web Application**:
- **On Desktop (> 768px) for Munimji / Office:** Wide sidebar, high-speed keyboard shortcuts (`Alt+1/2/3/4`, `Enter` to submit), fast thermal/laser printing.
- **On Mobile Chrome (< 768px) for Owners (Sadik & Shanu Bhai):** Disables desktop sidebar; displays fixed bottom navigation bar (`Dashboard`, `Inward`, `Chamber`, `Khata & Bill`), large 48px touch targets, bottom-sheet slide-up modals, and 2x2 KPI grid.

---

## 4. The 4 Core Operational Modules

```
[ Step 1: Gate Inward ] ──► [ Step 2: Chamber Slotting ] ──► [ Step 3: Kisan Khata ] ──► [ Step 4: Nikasi & Settlement ]
         │                                   │                               │                            │
   Tractor / Truck Entry            Chamber 1-4 & Floor Bori        Advance Loan & 1.5% Byaj      Bhada + Hamali + Bardana
   Digital Lot Parchi Print         Visual Slot Grid Map            Carry-forward tracking        Gate-pass & WhatsApp Bill
```

### Module 1: Gate Inward & Automated Lot Numbering
- Captures: Farmer Name, Mobile, Village, Vehicle No, Bag Count, Variety, Assigned Chamber/Floor.
- Generates unique sequential tokens (`LOT-1001`, `LOT-1002`, etc.) with QR/barcode for stack tagging.
- Instant printable slip modal + WhatsApp receipt dispatch.

### Module 2: Multi-Chamber & Floor (Gulla) Spatial Mapping
- Visual interactive grid for Chambers 1 to 4 with live temperatures (2.8°C–3.1°C) and humidity (88%–92%).
- Real-time chamber capacity progress bars.
- Clicking any floor/gulla reveals farmer ownership, bag quantity, and deposit timestamp.

### Module 3: Kisan Advance Loan & Interest Ledger
- Searchable farmer directory with bag balance.
- Automated daily/monthly interest accrual (1.5%/month) calculated from deposit date.
- Real-time market liability overview ("Market mein baaki Advance + Byaj").

### Module 4: Gate Outward, Hamali/Bardana & Settlement
- Supports two settlement modes:
  1. *Cash Payment Mode:* Farmer brings cash: `Rent + Hamali + Bardana + Advance + Interest`.
  2. *Produce Sale Mode:* Cold storage acts as commission agent / liquidator: `Sale Value - (Rent + Hamali + Bardana + Advance + Interest) = Net Payable to Farmer`.
- Emits authenticated Gate Pass with "PAKKA" seal. No vehicle leaves gate without digital gate pass.

---

## 5. Ten Tested Mandi Edge Cases

| # | Edge Case Scenario | System Technical Handling |
|---|---|---|
| **1** | **Tukde Mein Nikasi (Partial Release)** | Farmer releases 150 out of 450 bags. Rent/Hamali charged strictly on 150 bags. Remaining 300 bags stay active in chamber with original lot priority. |
| **2** | **Zero Advance Loan** | For farmers with no advance loan, calculation shows no NaN or negative numbers: `Bill = Bhada + Hamali + Bardana`. |
| **3** | **Advance Exceeds Single Release Value** | When a farmer with ₹50,000 advance releases only 20 bags, interest is deducted first, then principal. Residual advance carries forward against remaining stored stock. |
| **4** | **Chamber Full / Over-Capacity Alert** | If Chamber 2 has only 1,000 bag capacity remaining and user enters 2,000 bags, dynamic red warning fires: *"Chamber 2 mein sirf 1,000 bori ki jagah bachi hai! Kripya doosre chamber me dalein."* |
| **5** | **Empty or Corrupted Input** | Submitting with empty farmer name, zero bags, or negative numbers triggers immediate validation toast without creating corrupted database records. |
| **6** | **Strict Unique Lot Numbering** | Tokens auto-increment and survive browser refreshes without duplicate key collisions. |
| **7** | **Case-Insensitive Multi-Field Search** | Farmers can be found by name ("ramesh", "PATEL"), village ("Mauranipur"), mobile number fragments, or lot numbers. |
| **8** | **Mobile Touch-Scroll Protection** | Thumb scrolling gestures on Android Chrome do not trigger unintended button clicks or modal closures. |
| **9** | **Client-Side Persistence & Safe Reset** | Demo runs on `localStorage`. A protected [Reset] button allows restoring default sample state after client testing. |
| **10** | **Pro-Rata Daily Interest Computation** | Interest computed precisely on days elapsed at 1.5%/month with standard Indian currency formatting (`₹ XX,XXX`). |

---

## 6. Commercial Scope & Delivery Schedule

- **Production Timeline:** 2 to 3 Weeks from contract execution.
  - *Week 1:* Plant parameter calibration (chamber dimensions, custom tariff rates, hardware printer pairing).
  - *Week 2:* Historical data migration, staff & munimji on-site/remote training.
  - *Week 3:* Full pilot run before peak harvest inward rush.
- **Estimated Commercial Contract Value:**
  - One-time implementation, custom branding & setup: **₹85,000 – ₹1,10,000 INR**
  - Annual Cloud Hosting, Daily Multi-Zone Backup & 24/7 Season Support Retainer: **₹12,000 – ₹18,000 / Year**
