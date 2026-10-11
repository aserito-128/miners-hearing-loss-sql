# Occupational Hearing Loss Analysis in Mining Industry

## Project Overview
This project simulates a data analytics pipeline for an occupational health department in a mining company. The goal is to monitor, track, and evaluate progressive hearing degradation among miners over time, isolating environmental factors (machinery noise exposure) and lifestyle compounding factors (smoking status).
To ensure high clinical precision, the analysis evaluates hearing degradation based on the **worst-performing ear** (`worst_ear_loss_dB`) for each individual exam session.

## Business & Clinical Logic
* **Worst Ear Approach:** In occupational health, tracking a single ear over time or averaging both ears can mask severe unilateral acoustic trauma. This query dynamically isolates the highest decibel loss between the left and right ear per test.
* **Window Functions for Progression:** Using `LAG() OVER (PARTITION BY...)`, the script calculates year-over-year hearing threshold shifts (`dB_worsening_delta`) without relying on expensive self-joins.

## Tech Stack & SQL Concepts Used
* **Platform:** Google BigQuery (Standard SQL)
* **Advanced CTEs (Common Table Expressions):** Layered for clean data isolation without permanent DML operations.
* **Window Functions:** `LAG()` for chronological delta calculations.
* **Conditional Logic:** `IF()` control flow for worst ear mapping.
* **Data Sanitization:** `COALESCE()` to elegantly handle baseline checks (first-time exams).

## Database Architecture (Logical Schema)
The architecture mimics a production relational database split into 4 core entities:
1. `miners`: Demographic profiles & baseline lifestyle habits.
2. `departments`: Workplace noise baselines mapped by site.
3. `work_history`: Temporal employment tracking (reconstructs career exposure).
4. `audiometric_exams`: Longitudinal pure-tone audiometry test records.

## How to Run the Query
Since DML queries are restricted under Google BigQuery's free sandbox tier, the dataset has been fully mocked inside the memory layer using a `WITH ... UNION ALL` architecture. 
1. Open the [Google BigQuery Console](https://google.com).
2. Copy the full contents of `query.sql` into the editor.
3. Click **Run**. No billing account or physical dataset creation is required.
