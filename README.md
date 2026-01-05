# Digital Detectives of Environment (DiDE) - Part 3 Analysis

## 🎯 Goal of This Part
The main goal of this part was to identify the user who acts most similarly to me within the dataset. More specifically, I aimed to find the person whose events occur *close to my events in both space and time*.

To achieve this, I applied a *K-Nearest Neighbour (KNN)* logic using PostGIS for spatial analysis and QGIS for visual verification.

In this context, KNN means:
>The user who creates events that are spatially close and temporally synchronized to my events can be considered my nearest neighbour (DiDE Partner).

---

## 1  Data Preparation (PostGIS)
All events were stored in the following PostgreSQL/PostGIS table imported from the collected GeoJSON data:

public.olaylar_1765259089240

Each event record contains detailed attributes essential for analysis:
* *geometry (geom):* The spatial location of the event.
* *timestamp (created_at):* The exact time the data was collected.
* *user information (created_by, created_by_id):* To distinguish between my data and others.
* *photo links (photo_urls):* Evidence of the observation.

---

## 2  Spatio-Temporal Analysis (Finding My Partner)
To find my partner, I executed a Spatial SQL query. I filtered the dataset to compare my User ID (*207*) against all other users, calculating the distance between points.

### 🧩 The SQL Query
I used ST_Distance to calculate proximity and ordered the results to find the closest match (Nearest Neighbor).

![](WhatsApp%20Image%202026-01-05%20at%2018.31.55.jpeg)



 ## 3 Performance Analysis: Spatial Indexing (Bonus)
To demonstrate the efficiency of database indexing, I tested the query performance with and without a Spatial Index (GIST).

🚀 Query WITH Spatial Index (Fast)
The query executed significantly faster when the spatial index was active.
![](WhatsApp%20Image%202026-01-05%20at%2018.19.39.jpeg)


Execution Time: 0.007s (approx)

🐌 Query WITHOUT Spatial Index (Slow)
I dropped the index and restarted the session (cold start) to simulate a non-indexed search. The query took significantly longer to process because the database had to scan the entire table sequentially.
![](WhatsApp%20Image%202026-01-05%20at%2018.19.34.jpeg)

Execution Time: 0.016s (approx)

Conclusion: Using a spatial index significantly improves query performance  by allowing the database to locate geometries efficiently without scanning the entire table.

## 4 Accessing Event Details & Photos
By clicking on a point in QGIS, I inspected detailed event information using the Identify Features panel. This allowed me to verify:

Event Type (Cigarette Butt vs. Trash Bin)

User ID matching

Timestamps

📸 Opening Photos
In QGIS, I could also access the photo URLs directly from the attribute table to verify the field observations visually.


![](WhatsApp%20Image%202026-01-05%20at%2018.19.47.jpeg)
![](WhatsApp%20Image%202026-01-05%20at%2018.19.57.jpeg)