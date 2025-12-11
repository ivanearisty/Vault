
## Urban Time Traveler
### Tracking the Evolution of Pedestrian Infrastructure

1. Backend
2. Frontend
3. Demo

---


A "Time Machine" for urban planners and researchers to visualize how pedestrian infrastructure (sidewalks, crosswalks) has evolved.
<small>

**Scope:**
- **Focus Area:** Central Brooklyn
- **Time Range:** Historical analysis of as many years we can find (1924, 1951, 1996, 2004, 2010, 2012, 2014, 2016)
- **Output:** A web dashboard for comparing/analyzing infrastructure.

</small>

---

### Key Questions
<small>

1. **Quantifying Change**: How has the total length and area of pedestrian space changed over the last decade?

2.  **Walkability Evolution:** Have specific neighborhoods seen measurable improvements in walkability metrics?

3.  **Detection Accuracy:** Can computer vision models (`tile2net`) accurately extract infrastructure from historical aerial imagery compared to official datasets?


</small>


---

### Data Acquisition Sources

<small>

1.  **NYC Planimetrics:** Official vector data (Shapefiles/GDB) used for validation (Years: 1924, 1951, 1996, 2004, 2010, 2012, 2014, 2016).
2.  **OpenStreetMap (OSM):** Contextual street network data.
3.  **NYCSidewalks:** Additional reference datasets.
4.  **NYC Map Tiles:** Historical aerial imagery tiles (XYZ service).

</small>

---

### Data Acquisition Issues


<small>

*   Rate Limits
*   Bandwidth
*   File Sizes
*   Missing Tiles

</small>

---

<grid >
![[Screenshot 2025-12-01 at 4.00.30 PM.png | 300]]
![[Screenshot 2025-12-01 at 4.01.12 PM.png | 70]]
![[Screenshot 2025-12-01 at 4.03.18 PM.png | 50]]
</grid>

 

<small>

We allowed our inference VM to fetch tiles dynamically. When the external server 404'd, 403'd, or timed out, the model ran inference on **black/empty tiles**.

*   Never let expensive GPUs run on unvalidated data.
*   Ensure all assets are valid on disk before spinning up inference.
*   We burned compute credits processing "nothingness."

</small>

---

### Remedies & Scaling Strategy

<small>

To overcome these bottlenecks, we pivoted our approach:

1.  **Proof of Concept:** Successfully ran the model on a single block in Brooklyn to validate the pipeline.
2.  **Controlled Expansion:** Expanded to a specific bounding box in Brooklyn, capping raw file sizes to ~1GB.
3.  **Cloud Orchestration:** Created batch jobs to process large areas in parallel.

</small>

---

#### Current Status Backend

We have a working pipeline, finished inference on all of our data, and a functional frontend.

---

### Architecture

```
[Historical Aerial Tiles] →  [Tile2Net Inference]  →  [Postprocessing Pipeline]
                                     ↓                            ↓
                          [GeoJSON Infra + Metrics]      [Diff Engine: Added/Removed/Unchanged]
                                     ↓                            ↓
                              --------------------------------------
	                              |           API Layer        |
                              --------------------------------------
											   ↓
					   ------------------------------------------------
					   | React Frontend (MapboxGL Renderer + UI Logic) |
					   ------------------------------------------------

```


---

![[Screenshot 2025-12-01 at 7.26.41 PM.png]]

---

## Planned Changes for Final Report

<small>

1.  **Real Data:** Find a way to get validation data that is not just 2 years.
2.  **Control:** Add user defined controls for spatial tolerance parameters in the UI.
3.  **Control pt2:** Drag and drop shapefiles that take stuff from user.

</small>

---

## Demo

---

## Conclusion

We built the machine.
Now we need to tweak it.
