# Survivor: Data Island - Dataset Readme

Welcome to the **Survivor: Data Island** dataset, designed for the [KQL reality show blog post](https://rodtrent.substack.com/p/kql-reality-show-survivor-data-island)! This `IslandLogs.csv` dataset simulates the activities of contestants stranded on Data Island, where they must query to survive. This README explains how to use the dataset, its structure, and how to leverage it with Kusto Query Language (KQL) for survival challenges.

## Dataset Overview
The `IslandLogs.csv` file contains 100 rows of survival activity logs from Data Island, capturing contestant actions like searching for water, fishing, building shelters, and stirring drama. It’s your key to mastering the island’s resources using KQL.

### File Details
- **Name**: `IslandLogs.csv`
- **Format**: Comma-separated values (CSV)
- **Size**: 100 rows
- **Purpose**: To support KQL queries for survival tasks, such as finding resources or analyzing camp dynamics.

## Dataset Structure
The dataset has the following columns:

| Column      | Type        | Description                                      |
|-------------|-------------|--------------------------------------------------|
| Timestamp   | Datetime    | Time of the activity (e.g., `2025-07-01T08:00:00`)|
| Contestant  | String      | Name of the contestant (e.g., Alice, Bob, Null)  |
| Activity    | String      | Action taken (e.g., Searched, Fished, Argued)    |
| Resource    | String      | Resource involved (e.g., Water, Fish, Drama)     |
| Quantity    | Integer     | Amount of resource (e.g., 5 for water units)     |
| Location    | String      | Where the activity occurred (e.g., North, Camp)  |

### Sample Rows
```
Timestamp,Contestant,Activity,Resource,Quantity,Location
2025-07-01T08:00:00,Alice,Searched,Water,5,North
2025-07-01T08:15:00,Bob,Fished,Fish,2,South
2025-07-01T08:30:00,Charlie,Built,Shelter,1,Camp
2025-07-01T08:45:00,Null,Did Nothing,None,0,Unknown
```

## Getting Started
To use this dataset, you’ll need a KQL-compatible environment, such as **Azure Data Explorer** or **Microsoft Sentinel**. Follow these steps:

1. **Import the Dataset**:
   - Download `IslandLogs.csv`.
   - In Azure Data Explorer, create a table (e.g., `IslandLogs`) and ingest the CSV using the following command:
     ```kql
     .ingest into table IslandLogs @"path/to/IslandLogs.csv" with (format="csv", ignoreFirstRecord=true)
     ```
   - Alternatively, use the Azure Data Explorer web UI to upload the CSV and map columns.

2. **Verify the Data**:
   - Run a quick query to check the data:
     ```kql
     IslandLogs | take 10
     ```
   - This displays the first 10 rows to confirm successful ingestion.

3. **Explore the Dataset**:
   - Use KQL to query the dataset for survival tasks (see examples below).
   - Experiment with the challenges from the [blog post](https://rodtrent.substack.com/p/kql-reality-show-survivor-data-island) or create your own.

## Example KQL Challenges
Here are sample queries from the [**Survivor: Data Island** blog post](https://rodtrent.substack.com/p/kql-reality-show-survivor-data-island) to get you started. These align with the survival challenges described in the post.

### Challenge 1: Find the Water Source
Identify locations with the most water collected.
```kql
IslandLogs
| where Resource == "Water"
| summarize TotalWater = sum(Quantity) by Location
| order by TotalWater desc
```
**Expected Output**: Shows `North` as the top water-rich location.

### Challenge 2: Uncover Camp Drama
Find out who’s causing the most drama.
```kql
IslandLogs
| where Resource == "Drama"
| summarize DramaCount = count() by Contestant
| order by DramaCount desc
```
**Expected Output**: Highlights contestants like Charlie as drama instigators.

### Challenge 3: Optimize Resource Gathering
Combine water and fish collection for a survival strategy.
```kql
IslandLogs
| where Resource in ("Water", "Fish")
| summarize TotalResources = sum(Quantity) by Location, Resource
| order by TotalResources desc
```
**Expected Output**: Ranks locations by resource abundance, guiding you to North and South.

## Tips for Using the Dataset
- **Filter Null Values**: Contestant `Null` represents inactivity. Filter them out with `where Contestant != "Null"` for meaningful insights.
- **Time-Based Analysis**: Use `Timestamp` to analyze activity patterns, e.g., `summarize by bin(Timestamp, 1h)`.
- **Experiment Freely**: Try new queries, like finding the most active contestant or busiest location.
- **Join with Other Data**: The [blog](https://rodtrent.substack.com/p/kql-reality-show-survivor-data-island) mentions a `FishDetails` table. You can create a similar table and practice `join` operations.

## Environment Setup
- **Azure Data Explorer**: Free tier available at [learn.microsoft.com/azure/data-explorer](https://learn.microsoft.com/azure/data-explorer).
- **Demo Cluster**: Use the Azure Data Explorer demo environment for quick testing.
- **KQL Tools**: Install the KQL extension for VS Code for syntax highlighting and query testing.

## Notes
- The dataset is fictional and designed for educational fun, inspired by the reality show theme.
- Timestamps span July 1-2, 2025, covering a 24-hour period with 15-minute intervals.
- Contestants include Alice, Bob, Charlie, Denise, and Null, each with varied survival strategies.
- Share your KQL queries on X with `#SurvivorDataIsland` to join the community challenge!

## Troubleshooting
- **Ingestion Errors**: Ensure `ignoreFirstRecord=true` to skip the CSV header.
- **Query Failures**: Verify column names match the dataset (case-sensitive).
- **Missing Data**: Check that all rows were ingested correctly using `IslandLogs | count`.

Dive into Data Island, query like a survivor, and may your KQL skills keep you from being voted off at Tribal Council!
