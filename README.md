# 🚛 Vehicle LWH Audit Report — Non-Large & Large Network

> **End-to-end vehicle dimension compliance pipeline** — from field auditing to vendor penalisation.
> Automated LWH (Length × Width × Height) audit of 32FT vehicles at transporter and facility level for **National MH-MH** and **Zonal MH-MH** trips, covering both Non-Large and Large networks.
> Results feed directly into the **Vendor Scorecard** as a compliance metric.

---

## 🧭 Project Overview

Flipkart's Non-Large and Large networks rely on standardised 32FT vehicles to maintain volumetric efficiency. Vehicles that do not meet LWH specifications (high cube vs low cube) distort CFT calculations, misallocate load planning, and create unfair billing. This project automates the full audit-to-action cycle:

1. **Field data collection** — LWH measurements executed on-ground by the **Ground Operations & Quality teams**, coordinated and driven centrally
2. **Automated audit pipeline** — classifies each vehicle as Pass/Fail across L, W, H dimensions
3. **Facility-level reporting** — pass rates per Motherhub for National and Zonal legs
4. **Vendor-level reporting** — pass rates per transporter, enabling targeted accountability
5. **Scorecard integration** — audit pass rate published as a metric on the Vendor Scorecard
6. **Penalisation dataset** — non-compliant vendor data packaged and shared with management for action

> **Note:** Physical vehicle auditing is conducted by the Ground Operations and Quality teams at source facilities. This pipeline is designed, owned, and centrally driven by **Sri Ranganatha Perumal Venkatesan**, with field execution supported by those teams.

**No manual collation. One pipeline. Automatic escalation.**

---

## 📸 Dashboard Preview

### 🏭 National MH-MH — Facility Level LWH Audit
Per-facility LWH audit pass rates (Data Received %, No Data %, Overall Passed %) with L / W / H dimension-wise breakdown for National 32FT vehicles.

![National Facility LWH Audit](01_national_facility_lwh_audit.png)

---

### 🏭 Zonal MH-MH — Facility Level LWH Audit
Same audit view for Zonal 32FT vehicles — facility-wise compliance across the MH-MH trip leg.

![Zonal Facility LWH Audit](02_zonal_facility_lwh_audit.png)

---

### 🚚 Zonal MH-MH — Vendor Level LWH Audit
Transporter-level LWH compliance rates for Zonal legs — the primary input for vendor penalty decisions.

![Zonal Vendor LWH Audit](03_zonal_vendor_lwh_audit.png)

---

### 🚚 National MH-MH — Vendor Level LWH Audit
Transporter-level LWH compliance rates for National legs — feeds into Vendor Scorecard and penalisation dataset.

![National Vendor LWH Audit](04_national_vendor_lwh_audit.png)

---

## 🏗️ Pipeline Architecture

```
Field LWH Measurement (per vehicle, per trip)
        │
        ▼
┌─────────────────────────────────────┐
│  Raw Data Ingestion                 │
│  - Vehicle ID, Facility, Vendor     │
│  - Trip Type (National / Zonal)     │
│  - Measured L, W, H values          │
└─────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────┐
│  Audit Classification Engine        │
│  - Pass/Fail per dimension (L,W,H)  │
│  - High Cube vs Low Cube detection  │
│  - No-data flagging                 │
└─────────────────────────────────────┘
        │
        ▼
┌─────────────────────────────────────┐
│  Aggregation Layer                  │
│  - Facility-level pass rates        │
│  - Vendor-level pass rates          │
│  - Segment split: National / Zonal  │
└─────────────────────────────────────┘
        │
        ├──► Vendor Scorecard (compliance metric)
        ├──► Management Penalisation Dataset
        └──► Dashboard (this report)
```

---

## 📊 Report Structure

Each report view contains the following columns:

| Column | Description |
|---|---|
| **Facility / Vendor Name** | Motherhub or Transporter being audited |
| **Data Received %** | Share of trips where LWH measurement data was submitted |
| **No Data %** | Share of trips with missing measurement data |
| **Overall Passed %** | Share of trips where vehicle passed all three dimensions |
| **L (Passed %)** | Length dimension pass rate |
| **W (Passed %)** | Width dimension pass rate |
| **H (Passed %)** | Height dimension pass rate |

**Colour coding:**
- 🟩 Green — High pass rate (compliant)
- 🟨 Yellow — Moderate compliance, watch list
- 🟥 Red / Pink — Low pass rate, action required

---

## 🗂️ Report Segments

| Segment | Trip Type | Vehicle Type | Granularity |
|---|---|---|---|
| National-32FT Facility | MH-MH National | 32FT | Motherhub level |
| Zonal-32FT Facility | MH-MH Zonal | 32FT | Motherhub level |
| National-32FT Vendor | MH-MH National | 32FT | Transporter level |
| Zonal-32FT Vendor | MH-MH Zonal | 32FT | Transporter level |

> Covers both **Non-Large** and **Large** network vehicles on MH-MH legs.

---

## ⚖️ Penalisation Logic

Vendors falling below defined compliance thresholds are flagged for penalisation:

- **No Data %** above threshold → vendor responsible for missing audit submissions
- **Overall Passed % / L / W / H** below threshold → vehicle does not meet network standards
- Flagged vendors are compiled into a structured dataset shared with the **Central Ops & Finance teams** for penalty processing
- Repeat offenders are escalated to the **Vendor Scorecard** with permanent compliance history

---

## 📋 Vendor Scorecard Integration

The LWH audit pass rate is published as a **scored metric** on the Flipkart Vendor Scorecard:

- Scorecard updates on a defined cadence (weekly / monthly)
- Vendors are ranked within their segment (National / Zonal)
- Persistent non-compliance impacts vendor allocation and rate negotiations

---

## 📂 Repository Structure

```
vehicle-lwh-audit/
│
├── lwh_audit_pipeline.py               # Core audit & aggregation script
├── lwh_audit_pipeline.ipynb            # Colab notebook wrapper
├── README.md
│
├── 01_national_facility_lwh_audit.png  # Report screenshots
├── 02_zonal_facility_lwh_audit.png
├── 03_zonal_vendor_lwh_audit.png
├── 04_national_vendor_lwh_audit.png
│
└── sample_data/
    └── sample_lwh_input.csv            # Synthetic sample for local testing
```

---

## 🚀 Quick Start

### Prerequisites
- Google account with Google Drive access
- Raw LWH measurement CSV exported from the Flipkart audit tool
- Google Colab (free tier sufficient)

### Input CSV Format

```
trip_id, trip_date, trip_type, facility_name, vendor_name,
vehicle_number, measured_L, measured_W, measured_H,
standard_L, standard_W, standard_H
```

### Steps

**1. Place input CSV in Drive:**
```
MyDrive/Vehicle_LWH_Audit/input/
```

**2. Run the notebook in Colab:**
```
Runtime → Run All
```

**3. Outputs generated at:**
```
MyDrive/Vehicle_LWH_Audit/output/
  ├── national_facility_report.csv
  ├── zonal_facility_report.csv
  ├── national_vendor_report.csv
  ├── zonal_vendor_report.csv
  └── penalisation_dataset.csv
```

---

## 👤 Author

**Sri Ranganatha Perumal Venkatesan** — pipeline design, data engineering, reporting automation, scorecard integration, and penalisation framework.

Field auditing executed by the **Ground Operations & Quality teams** at source facilities, coordinated under this initiative.

For access, questions, or onboarding, raise an issue in this repository or connect via GitHub.

---

*Vehicle Compliance Intelligence © 2026 | Flipkart Supply Chain*
