# Agency Executive Dashboard

## Overview

The Agency Executive Dashboard is a Power BI solution designed for Agency Leaders, Regional Managers, and Sales Executives to monitor agency performance, agent productivity, and policy persistency.

## Dashboard Pages

### 1. Executive Summary
- YTD APE
- Active Agents
- Persistency Rate
- Actual vs Target %
- Monthly APE Trend
- Top 10 Agents by APE

### 2. Agent Performance Grid
- Agent Name
- Actual APE
- Target APE
- Achievement %
- Performance Status

### 3. Agent Profile Card
- Agent Details
- YTD APE
- Rank
- MoM Growth
- Product Mix
- Persistency

### 4. Persistency Cohort
- Cohort Analysis by Issue Year
- Renewal Tracking
- Persistency Heatmap

## Data Model

### Fact Tables
- Fact_APE
- Fact_Persistency
-Fact_Target

### Dimensions
- Dim_Date
- Dim_Agent
- Dim_Region
- Dim_Product


## Key DAX Measures

```DAX
Total APE = SUM(Fact_APE[APE Amount])
```

```DAX
Actual vs Target % = DIVIDE([Total APE],[Target APE])
```

```DAX
Persistency_Rate = 
VAR __Renewed =
    CALCULATE (
        COUNTROWS ( fact_persistency ),
        fact_persistency[renewal_status] = "renewed",USERELATIONSHIP(dim_date[date_key],fact_persistency[renewal_due_date_key])
    )
VAR __Due =
    CALCULATE (
        COUNTROWS ( fact_persistency ),
        fact_persistency[renewal_status] IN { "renewed", "lapsed", "grace" },USERELATIONSHIP(dim_date[date_key],fact_persistency[renewal_due_date_key])
    )
RETURN
DIVIDE ( __Renewed, __Due )
```

## Recommended Relationships

Active Relationship:
- Calendar -> Issue Date Key

Inactive Relationship:
- Calendar -> Renewal Date Key

Used USERELATIONSHIP() for renewal analysis. 

## RLS

RLS is implemented at Region level considering as regional managers access

## Technology Stack

- Power BI Desktop
- Power BI Service
- DAX
- Star Schema


## Business Benefits

- Performance Monitoring
- Target Tracking
- Agent Productivity Analysis
- Persistency Monitoring
- Renewal Management
- Executive Reporting

Version: 1.0
