Real-Time Fraud Investigation: Optimising Device Tracking & Audit Efficiency

**1. Executive Summary:**
- The Project: Redesigned a business-critical fraud investigation report by migrating to a real-time event log framework and building regex-driven device mapping.
- The Impact: Eradicated a 24-hour data latency bottleneck, eliminated manual cross-system hardware lookups for frontline agents, and enabled instant parameter-driven lookups in Metabase.

**2. Background & Objective:**
The Member Service team relies heavily on account login logs to trace suspicious activity, identify account takeovers, and verify user identity. However, the pre-existing data pipeline and report suffered from severe operational inefficiencies:
- Stale Data / 24-Hour Latency: The legacy query pulled from an aggregate table that updated only once every 24 hours. During active fraud escalations, agents lacked visibility into real-time login activity.
- Manual Lookup Bottlenecks: Raw mobile user-agent strings display obscure Apple hardware strings (e.g., iPhone18,2) rather than commercial market names (iPhone 17 Pro Max). Frontline agents had to manually pause investigations to cross-reference these codes against an external database to determine the device model.
- User Friction: The query was not optimised for quick, ad-hoc searches by non-technical team members, causing delays in resolving high-priority user complaints.

**3. The Technical Solution:**
I entirely refactored the underlying script to establish a direct connection to live transactional logs, standardising timestamps to local context and automating text parsing directly within the query.
Key Technical Improvements:
- Zero Data Latency: Shifted the data source to the live core.fact_user_log table, providing immediate data availability for high-urgency fraud investigations.
- Regex Hardware Extraction: Built a regex-based device mapping layer that translated raw Apple hardware identifiers into analyst-friendly device names. (up to the newest device architectures like the iPhone 17 Pro Max and iPad mini 7th Gen).
- Dual-Timezone Transparency: Implemented FORMAT_DATETIME and timezone evaluation strings to simultaneously surface UTC timestamps alongside the user's localised time, ensuring exact chronological alignment during cross-border security reviews. This is especially useful for international travel and our US users, with the company being UK-based.
- Metabase Parameterisation: Wrapped the filter logic in a dynamic variable ({{user_id}}) so customer-facing teams could instantly execute reports without writing or reading raw SQL.

**4. Architecture**

User Login Event
       ↓
core.fact_user_log
       ↓
SQL Transformation Layer
       ↓
Device Mapping Logic
       ↓
Metabase Report
       ↓
Fraud Investigator

**5. The Production Code:**

- Real-Time Log Source
</> SQL
  From core.fact_user_log
- Device Mapping: 
</> SQL
  CASE
  WHEN REGEXP_CONTAINS(...)
  ...
  END
- Timezone Handling
</> SQL
  FORMAT_DATETIME(...)

**6. Impact**

- Removed 24-hour reporting latency
- Eliminated manual device lookups
- Reduced fraud investigation time by an estimated 2–5 minutes per case
- Enabled self-service reporting for customer support teams

**7. Skills Demonstrated**

- SQL (BigQuery)
- Data Transformation
- Regex Pattern Matching
- Fraud Analytics
- Event Log Analysis
- Business Intelligence (Metabase)
- Timezone Normalisation
- Stakeholder-Focused Reporting
