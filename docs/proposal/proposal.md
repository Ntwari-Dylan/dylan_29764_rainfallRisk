# Rwanda Rainfall Monitoring and Flood Risk Assessment System

Ishimwe Ntwari Dylan · Student ID 29764 · INSY 8311 Database Development with PL/SQL · Instructor: Eric Maniraguha · GitHub: https://github.com/Ntwari-Dylan/dylan_29764_rainfallRisk

## Problem Statement

Heavy rainfall regularly causes floods and landslides in Rwanda. Between 1 and 6 May 2023, torrential rain affected 14 districts, killed 137 people and destroyed 5,472 houses (International Federation of Red Cross and Red Crescent Societies [IFRC], n.d.). The same report states that the national meteorological agency reported 110–130 mm of rainfall on 3 May 2023, while earlier flood events had maximum rainfall of roughly 36–61 mm.

Rainfall data is usually published as separate files. A file on its own does not compare areas and dates, show when rainfall crossed a level that needs attention, or keep a trace of who changed the data. Staff and analysts therefore need a structured, queryable record of rainfall by area and period that is linked to monitoring thresholds.

## Proposed Solution

The project will build an Oracle 21c database with PL/SQL that (1) stores dekadal (10-day) rainfall by administrative area, (2) stores configurable monitoring thresholds, (3) compares each observation with the thresholds and records alerts, (4) keeps an audit log of changes, and (5) provides summary views for reporting and a dashboard. This is an academic system. An alert means that a configured rule was crossed. It is not a validated flood prediction and not an official warning.

## Data Source

The project uses the dataset *Rwanda: Rainfall Indicators at Subnational Level*, published by the World Food Programme (WFP) on the Humanitarian Data Exchange. It is computed from CHIRPS version 2 satellite imagery combined with station data and aggregated by administrative unit for each 10-day period. The copy used was downloaded from Hugging Face (https://huggingface.co/datasets/electricsheepafrica/africa-rwa-rainfall-subnational). It contains 58,680 rows and is licensed CC BY 4.0. Main columns include date, adm_level, adm_id, pcode, n_pixels, rfh (10-day rainfall), rfh_avg, r1h and r1h_avg. Limitation: the values are satellite-based estimates, and rainfall alone does not prove that a flood occurred.

## Planned Tables

- **ADMIN_AREAS:** administrative areas with their codes and names.
- **RAINFALL_RECORDS:** one row per area and dekad, with rainfall, long-term average and source.
- **RISK_THRESHOLDS:** configurable threshold levels, measure and unit.
- **ALERTS:** the observation and rule that caused an alert, with status and review details.
- **APP_USERS:** users and roles.
- **AUDIT_LOG:** who changed what, and when.

The final design (3NF) will be confirmed after the dataset file has been inspected.

## Planned PL/SQL Features

- **Procedure:** a load procedure validates each rainfall row and inserts it into RAINFALL_RECORDS.
- **Function:** a function returns the threshold status (normal, watch or alert) of an observation.
- **Cursor:** a cursor goes through new observations, compares each with the thresholds and writes the alerts.
- **Package:** one package groups the load, status and alert-processing code.
- **Triggers:** an audit trigger writes changes to AUDIT_LOG, and a restriction trigger blocks changes to loaded rainfall records by users without the administrator role.
- **Exceptions:** handlers deal with missing areas, duplicate dekads and invalid (negative) rainfall values.

## Innovation

Option: analytics dashboard. Tool: Power BI, connected to the Oracle database through summary views. It will show rainfall trends by area and period, and the observations that crossed thresholds. The business rules stay in PL/SQL, and the dashboard only displays the results.

## References

International Federation of Red Cross and Red Crescent Societies. (n.d.). *Rwanda: Floods and landslides – DREF final report (MDRRW022)*. ReliefWeb. https://reliefweb.int/report/rwanda/rwanda-floods-and-landslides-dref-final-report-mdrrw022

World Food Programme. (n.d.). *Rwanda: Rainfall indicators at subnational level* [Data set]. Humanitarian Data Exchange. https://huggingface.co/datasets/electricsheepafrica/africa-rwa-rainfall-subnational
