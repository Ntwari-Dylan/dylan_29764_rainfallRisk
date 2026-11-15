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

- 
## Phase I – Business Process Modeling

**Selected business process:** Rainfall monitoring process. WFP publishes the rainfall dataset, the Data Administrator imports it into a staging area, and the Oracle database validates the records, stores the valid ones, writes an audit log and compares rainfall with configured thresholds. When a threshold is crossed, an alert is generated for the Rainfall Analyst to review. The Decision-maker uses the dashboard and reports to support planning. An alert means a configured rule was crossed. It is not a flood prediction or an official warning.

**Modeling tool:** diagrams.net (draw.io)

**Published diagram link:** https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=rainfall_business_process.drawio&dark=auto#R%3Cmxfile%3E%3Cdiagram%20id%3D%22prtHgNgQTEPvFCAcTncT%22%20name%3D%22Page-1%22%3E3VzbUuM4EP2aVDEPsxXfcnmEBIbZBYZNhgEehSVsLYqVkhWSzNev5FviC4nxRLbLT0RtuS139%2BkjtWR6xmSx%2BcbA0r2lEJGe3oebnjHt6bpmDMbij5RsQ8mgHwkchmHUaSeY498oEvYj6QpD5Kc6ckoJx8u00Kaeh2yekgHG6Drd7ZWS9FOXwEE5wdwGJC99xJC7oXSkD3fya4QdN36yFr%2FwAsSdozfxXQDpek9kXPaMCaOUh78Wmwki0nixXcL7rj64mgyMIY%2BXuQHebbZ3%2Fwzt4ejlZv3uQ3R7bX3Vxnk1kWafb2Mj%2BGu8IMATrQuXL4gQauKn7WICb8CWruSNPgf2W9y6YMgXrryPtWoZ0S2QAdKX%2BijDv6nHAYkEQg%2FjURzo6R5z%2BYioWzjGd0BW0RhnAHuvIHjjW%2BphLu7yHNG4Z9RGvh%2F1R4yjzd5rRqb6hugCcbYVXdw9bw76ke%2FWO9fr49ihsZ64vc20QRR6TqJ85x3xI3LQJ5wVK9531kGvHnXhAWN%2FZOfHq3shOLuePn2pZNPEPCmb9tM2PYUJZ2v279Pj3wvn7XpDTH30NH59%2FDoI9SKYQ%2F0RkyYopitmowNPMHKmlw%2BbR03KuEsd6gFyuZNeMLryIIKRyXd9bihdRp76D3G%2BjTwFVpym%2FSjGzrZP0f1B41k2%2FrLi5nSzf3G6jVoQvYIV4TfgBZELAS4nGMmEEspEjzA3XhB59R5AKAEl79P7ZqAJnssEK0Q2Ab6P7VB4hUk8rChfA%2BYgfsBkZnHUMEQAx%2B9pR508IozPBsCeYwkRNCRNtHYxR%2FMlCCJjLZgwAzMXLNF3zxdUlrLMHqzmP89nPysBapTH0yiNJiudoJSBy6xuyh0CtOPmjDrfI4bFwBH7yKb3qxeCfVeG0o4cIODAF%2BFYxdTDvKm1DBvoGTbQLVVkUDZzqSSDqTCmkJzDBfawzxkQtKueFU5JtIWBPK7MEvHc6RhLWE2wxJ9k%2B5KpfJQLkSZzu%2FVZtylOSFO69ggFcD8N9XvWxaxnTSvBZvD5hGSoAs2oZcb%2BvlgKjMhRewII8nnACZcEwuIThRY3RjVZXGubySeUMbkIlzYXYgwDpNuUQT80%2B4NCs1uD4cnNXmxNvQXM%2B4MBWyjV%2B5KCX0QqqY12dVNZPJuVeVcvybtawSy%2BE8Sr6a1i3tjO5T2nODX9kukIcJTNSCqptz4iqF7WKA2cvJ86AhwrFyvPl%2FNGsVOWX4qw49LFy8o%2FSUliluAk5HLjqhJQCuoTmpal7poqFPFqXCVQurq00812MUzpxV1NDDPnlKEELBmaUbneGJo10Uy86aUQPXGQdQ89g1ahRy9dq60JPROCAMuu0qcKUTOuqy4SK1aJmkFXUdOuemJs59ag5pHhYEkDVhDLsguhNdS3tP64Lujo6qEz6ih0DK1d0Cldq6yLcOhCjiUIdS73CLnLkO9SoqQyYGUgpA%2FqgpD6mpqRry53BEJGLmoaLg0Yn3aemtLAzxgrQmwz6vtIZXlAM0c11QeM0qdBaspS35CHWFi2BAQxroDds6lpUFdqsv4kmFPp44ixC4hwQOQpRYjfxU9H%2FrxBDvJgfEG8z961gu4T6Qipqz9hKPTPmYfWybL%2FS2lNs0TTDAU7w2dMWBejd6lTnqbo2y6y38orfEgUPiyjiveZ7QLPCYKowginicIpIogn4wLMdmXG%2B0BPhfDUCs5baoNMtSNzCERZTb1kcKrc5Ns703ouuHHrVzu2VGWXzxgoy7GjEjOSQy45PiHpagnWGBaksgZnIwUl2IOOU0yWM%2FSOgxwcUGUd0%2FjauDIu8SoEzbCroBm3CzTDTzpOMWiSSUI8vxQ8xVcqzutkwTOqbQ1c8JFLkyb%2FFWYpCHz3hQImJ31Azj1FBMojazUkrt0SS%2FmeUQumUVNkYx9T7%2BsCvAmv1DWFSlatp2eDMgeUD7njKBuYXd1RMNu1o2AW7CgcdFxdU6hdKgpTExQYgqhLmcksqEiXNP0pP%2Fy5vKtmy%2BOf%2FehGXR%2F%2BVD4XkGT9o%2FmoiXS0%2B6pu9yHdc2%2F%2FO7rTf1VXMo1ZTSYtTVPu767uysXJrSX0o6v3ZHcPjuZnEnc%2FGnWmrt6ZjeByLw8PU4lYaz4Ra%2F0mXW5UPvhd2uVGIx%2B011EXyh%2F8bnQloB69zWzUt3gWVVAabDaFm%2Bpn0ka%2B0NINPJt1nbAVzd1%2F7Qmu7f3vI%2BPyfw%3D%3D%3C%2Fdiagram%3E%3C%2Fmxfile%3E

**Supporting documents:**
- [Business process diagram (PDF)](docs/business_process/business_process_diagram.pdf)
- [Business process explanation (PDF)](docs/business_process/business_process_explanation.pdf)
