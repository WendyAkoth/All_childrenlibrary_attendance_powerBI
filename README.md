# All Children  Library Event Attendance & Program Analysis Dashboard

An end-to-end Power BI analytics project exploring event turnout, program performance, and branch utilization across public library systems. Designed with a clean two-page architecture following modern UI/UX principles (F-pattern layout, custom canvas dimensions, and interactive cross-filtering).


## Executive Summary

* **Total Event Attendance:** 5,892 attendees across all tracked events.
* **Total Events Hosted:** 169 scheduled sessions.
* **Average Attendance Rate:** **60.7 attendees per event** for top-performing programs like *Storytime* at Silver Spring—representing a **~55% increase** over the system-wide baseline (38.8).
* **Primary Growth Drivers:** *Silver Spring Branch* and family-oriented programming (*Storytime*) serve as the core anchors of public engagement.

---

## Project Architecture & Data Model

### Data Pipeline & Modeling
1. **Data Cleaning & Transformation:** Cleaned raw event logs, handled missing/null values without distorting turnout metrics, and unified date-time structures.
2. **Calendar & Sorting Mapping:** Established custom date sort keys (`Month1`) to enforce chronological sorting across charts and tables (Jan–Dec).
3. **DAX Governance:** Standardized calculations into a central `_Key Measures` table to keep the model clean, structured, and easy to maintain.

### Core DAX Measures
```dax
// Total Attendance Volume
Total Attendance = SUM(All_Children_Excel_Program_Attendance_20240525[Attendance_Count])

// Total Events Hosted
Total Events = DISTINCTCOUNT(All_Children_Excel_Program_Attendance_20240525[Event_ID])

// Average Turnout per Event (handling blank values cleanly)
Average Attendance = AVERAGE(All_Children_Excel_Program_Attendance_20240525[Attendance_Count])

// Deep Dive Page DAX Metrics
Total Categories = DISTINCTCOUNT(All_Children_Excel_Program_Attendance_20240525[Primary_Event_Type])

Top Category = 
TOPN(
    1, 
    VALUES(All_Children_Excel_Program_Attendance_20240525[Primary_Event_Type]), 
    [Total Attendance], 
    DESC
)

Storytime Avg Turnout = 
CALCULATE(
    [Average Attendance], 
    VALUES(All_Children_Excel_Program_Attendance_20240525[Primary_Event_Type]), 
    [Primary_Event_Type] = "Storytime"
)