# Digital Detectives of Environment (DiDE) - Part 3 Analysis

## 🎯 Goal of This Part
[cite_start]The main goal of this part was to identify the user who acts most similarly to me within the dataset[cite: 33]. More specifically, I aimed to find the person whose events occur *close to my events in both space and time*.

To achieve this, I applied a *K-Nearest Neighbour (KNN)* logic using PostGIS for spatial analysis and QGIS for visual verification.

In this context, KNN means:
> [cite_start]The user who creates events that are spatially close and temporally synchronized to my events can be considered my nearest neighbour (DiDE Partner)[cite: 34].

---

## 1️⃣ Data Preparation (PostGIS)
[cite_start]All events were stored in the following PostgreSQL/PostGIS table imported from the collected GeoJSON data[cite: 32]:

public.olaylar_1765259089240

Each event record contains detailed attributes essential for analysis:
* *geometry (geom):* The spatial location of the event.
* *timestamp (created_at):* The exact time the data was collected.
* *user information (created_by, created_by_id):* To distinguish between my data and others.
* *photo links (photo_urls):* Evidence of the observation.

---

## 2️⃣ Spatio-Temporal Analysis (Finding My Partner)
[cite_start]To find my partner, I executed a Spatial SQL query[cite: 33]. I filtered the dataset to compare my User ID (*207*) against all other users, calculating the distance between points.

### 🧩 The SQL Query
I used ST_Distance to calculate proximity and ordered the results to find the closest match (Nearest Neighbor).

```sql
SELECT 
    b.created_by as PARTNER_NAME,
    a.created_at as MY_TIME,
    b.created_at as PARTNER_TIME,
    ST_Distance(a.geom, b.geom) as distance
FROM 
    public.olaylar_1765259089240 as a, 
    public.olaylar_1765259089240 as b
WHERE 
    a.created_by = '207'       -- My User ID
    AND b.created_by <> '207'  -- Other Users
ORDER BY 
    distance ASC
LIMIT 1;