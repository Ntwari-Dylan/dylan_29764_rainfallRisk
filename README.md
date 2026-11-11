# Rwanda Rainfall Monitoring and Flood Risk Assessment System

**Student:** Ishimwe Ntwari Dylan | **Student ID:** 29764
**Course:** INSY 8311 – Database Development with PL/SQL (AUCA, 2026–2027 Semester I)
**Instructor:** Eric Maniraguha

## Problem statement
Rainfall and flood information in Rwanda is not always easy to collect and analyse. When rainfall data is kept in separate files, it is hard to compare areas and dates, review past patterns, or notice when rainfall crosses a level that needs attention. This project builds an Oracle database that stores rainfall data by area and date, compares it with configurable thresholds, and records monitoring alerts.

This is an academic system. A threshold alert means a rule was crossed. It is not a validated flood prediction or an official warning.

## Objectives
**Main objective:** Design and implement an Oracle database with PL/SQL that stores Rwanda rainfall data and flags rainfall that crosses configured thresholds.

**Specific objectives:**
1. Import and document a real rainfall dataset (source, coverage, units, limitations).
2. Organise the data in normalised (3NF) tables by administrative area and date.
3. Use PL/SQL to compare rainfall with configurable thresholds and record alerts.
4. Keep an audit log of changes and restrict changes to protected data.
5. Present the results in an interactive dashboard connected to the Oracle database.

## Data source
- **Dataset:** Rwanda: Rainfall Indicators at Subnational Level
- **Publisher:** World Food Programme (WFP), via the Humanitarian Data Exchange (HDX)
- **Based on:** CHIRPS version 2 (satellite imagery with station data), aggregated by administrative unit, per 10-day period (dekad)
- **Licence:** CC BY 4.0
- **Size:** 58,680 rows (as shown on the Hugging Face dataset page, where a copy is hosted)
- **Main columns:** date, adm_level, adm_id, pcode, n_pixels, rfh, rfh_avg, r1h, r1h_avg (and others)
- **Limitation:** satellite-estimated rainfall, not rain-gauge readings; it does not prove that flooding occurred.

## Innovation
**Option:** Analytics dashboard. **Tool:** Power BI, connected to the Oracle database. It will show rainfall trends by area and period and the observations that crossed thresholds. Business rules stay in PL/SQL.

## Planned tables
DISTRICTS (or administrative areas), RAINFALL_RECORDS, RISK_THRESHOLDS, ALERTS, USERS/ROLES, AUDIT_LOG. FLOOD_INCIDENTS is optional, if reliable incident data is found.

## Planned PL/SQL features
- **Procedure:** validate and load rainfall records.
- **Function:** return the threshold status of an observation.
- **Cursor:** go through new observations and compare them with thresholds.
- **Package:** group the alert logic.
- **Trigger:** write changes to the audit log, and block edits to protected data.
- **Exceptions:** handle invalid, duplicate and missing records.

## References
- **Downloaded from:** https://huggingface.co/datasets/electricsheepafrica/africa-rwa-rainfall-subnational
