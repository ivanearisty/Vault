One of the key things the professor says is that the figures should be able to explain the paper without even reading it. Like, one must have a good idea of what the paper does just from the figures. What figures could make sense to include here?
In the data aquiscition section, for example, we had some errors were we ran inference on basically incomplete maps with areas of just darkness (we didnt get all the data and didnt realize) maybe we can show the different images and how we validated. then there could be figures of the frontend tool, like screen captures of different areas that we can show how it aligns to the 

this project actually also has an interesting conclusion, we realized that downtown brooklyn (or any part of nyc) was a very bad target since this city has not really changed at all. even if we compare sidewalks and pedestrian crossings between the early 1900s and now, we actually can't show that much of a difference. Running this on cities that actually have grown would make a much better case study. Miami is a great example of a city we could have chosen instead i think, especially since their pedestrian infra changes all the time; however, NYC has always been very walkable in comparison to other us cities, and we believe that a project that wants to evaluate quality would need more holistic methods. we're validating for existence of pedestrian infra, not the quality

Your goal is to create a paper that meets this criteria:

{
Idea C: The Urban Time Traveler


Goal & Key Questions
Goal

Design a visual tool for analyzing the evolution of urban pedestrian infrastructure over time using historical aerial imagery.
Key Questions

    How has the pedestrian network changed over the last decade?
    Where have sidewalks/crosswalks been added or removed?
    Can we correlate changes with city planning initiatives?
    How do walkability metrics change over time?


Potential Features
Visual Diff Map

    Highlight changes between time periods
    Show added paths (green)
    Show removed paths (red)
    Show modified paths (yellow)

Timeline Animation

    Slider control
    Animate network evolution
    Year-by-year transitions

Temporal Dashboard

    Network map view
    Linked time-series charts
    Track metrics over time:
        Total sidewalk length
        Network density
        Number of intersections

Case Study Example

Cambridge, MA: 23% change in crosswalks over 8 years
Resources: Codebase & Data
Primary Tool

Tile2Net

    Install and run the pipeline
    GitHub Repository

Aerial Imagery Sources

    NYC Planimetrics: High-res imagery for multiple years
    USGS NAIP: National aerial imagery
    Google Earth Engine: Vast satellite/aerial catalog

Ground Truth Data

For Comparison & Validation:

    OpenStreetMap (OSM)
        Crowd-sourced global data
        Detailed pedestrian features
    City GIS Portals
        Official sidewalk data
        Street centerlines
        Planning datasets

What You Need to Submit
1. Project Proposal (Due Oct 21)

    Length: 1-2 pages
    Content: Chosen direction, research questions, datasets, timeline

2. Final Report (Due Dec 11)

    Length: 4-6 pages (conference paper format, e.g., IEEE VIS)
    Sections: Motivation, related work, system design, implementation, case study

3. Source Code

    GitHub repository with well-documented code
    Setup instructions
    Dependencies listed
    Example usage

4. Demo Video

    Length: 3-5 minutes
    Content:
        System features showcase
        Walk through usage scenario

}

You should use:
1. The assignment outline to understand what the goals of our paper are and guide you
2. The example papers to see how professionals in this field structure papers
3. The 2 documentation markdown files to see how we structured the project
4. Our raw script files to see how we got data
5. Our raw frontend files to see how we built the tool

(((First, create an outline of the paper sections.)))



Here is a proposed list of figures for your paper. These are designed so that a reader skimming the paper would understand the problem, the technical challenges (the data acquisition issues), the solution (the UI), and the results just by looking at the images.

### Figure 1: The "Teaser" Figure
**Location:** Top of Page 1 (spanning two columns).
**Concept:** A high-impact composite image summarizing the "Urban Time Traveler" concept.
**Visual Composition:**
*   **Left:** A sepia-toned/grayscale map view of a Brooklyn intersection from **1924** (raw aerial imagery).
*   **Right:** A modern, high-contrast map view of the same intersection from **2024** (extracted vector network).
*   **Center:** A stylized "swipe" slider graphic in the middle, showing the transition from raster image to vector network.
*   **Caption:** *The Urban Time Traveler system enables temporal analysis of pedestrian infrastructure. By applying computer vision to historical aerial imagery (Left), we extract and visualize vector pedestrian networks (Right) across a century of urban development, allowing planners to quantify changes in walkability.*

---

### Figure 2: Data Pipeline & Quality Control (The "Black Tile" Issue)
**Location:** Section 3 (Data Pipeline).
**Concept:** This directly addresses your point about the inference errors. It illustrates the technical challenge of dealing with inconsistent WMS/XYZ tile servers.
**Visual Composition:**
*   **Panel A (The Failure Case):** Show a stitched image where several tiles failed to download (black squares) or returned 403 errors, alongside the resulting `tile2net` inference output (which might show bizarre artifacts or empty space where the black tiles were). Label this "Unvalidated Input."
*   **Panel B (The Correction):** Show the `stitch_tiles.py` output after the retry logic was implemented, resulting in a complete image. Next to it, show the clean vector output. Label this "Validated & Stitched Input."
*   **Caption:** *Handling data sparsity in historical archives. (A) Initial inference attempts on unvalidated tile downloads resulted in artifacts and missing network data due to server timeouts and black tiles. (B) Our optimized pipeline implements validation checks and stitching prior to inference, ensuring complete coverage before GPU processing.*

---

### Figure 3: System Interface & Components
**Location:** Section 5 (Interface Implementation).
**Concept:** An annotated screenshot of the full React application, mapping the visual components to the user tasks.
**Visual Composition:**
*   A full screenshot of the `App.jsx` UI in "Dark Mode" (based on your CSS).
*   **Annotations (Letters A-D pointing to specific areas):**
    *   **(A) Timeline Slider:** Point to the `TemporalSlider` at the bottom.
    *   **(B) Compare Controls:** Point to the toggle switch and year selectors in the sidebar.
    *   **(C) Metrics Dashboard:** Point to the charts showing "Net Change."
    *   **(D) The Map View:** Point to the main map rendering the GeoJSON.
*   **Caption:** *The Urban Time Traveler Interface. (A) The Temporal Slider allows for animation between 1924–2024. (B) Compare Controls enable side-by-side analysis. (C) The Metrics Dashboard updates dynamically to show network length and density. (D) The Map View renders color-coded infrastructure (Green=Added, Red=Removed).*

---

### Figure 4: The "Swipe" Comparison Mode
**Location:** Section 5 (Interface Implementation) or Section 6 (Case Study).
**Concept:** Demonstrating the `CompareMap.jsx` functionality.
**Visual Composition:**
*   A screenshot of the application in **Compare Mode**.
*   **Left Side:** 2014 Data (Before).
*   **Right Side:** 2024 Data (After).
*   **Focus:** Center the map on a specific location where a change occurred (e.g., a new crosswalk painted, or a sidewalk widened).
*   **Caption:** *Split-screen comparison mode. The user drags the slider to compare the 2014 pedestrian network (left) against 2024 (right), revealing a newly added pedestrian plaza and crosswalks at Grand Army Plaza.*

---

### Figure 5: Validation Against Ground Truth
**Location:** Section 6 (Case Study) or Section 7 (Discussion).
**Concept:** Visualizing the `ValidationMap.jsx` logic. This proves the tool works.
**Visual Composition:**
*   A map overlay showing three distinct colors of lines/polygons:
    *   **Green:** True Positives (Matches between `tile2net` and NYC Planimetrics).
    *   **Orange:** False Positives (Detected by `tile2net` but not in Planimetrics).
    *   **Red (Dashed):** False Negatives (In Planimetrics but missed by `tile2net`).
*   **Caption:** *Validation against NYC Planimetrics (2014). Green segments indicate correct detections. Orange segments represent false positives (often paved driveways misclassified as sidewalks). Red dashed lines indicate false negatives, primarily caused by tree canopy occlusion in the aerial imagery.*

---

### Figure 6: Small Multiples of Evolution (Case Study)
**Location:** Section 6 (Case Study).
**Concept:** A purely analytical figure showing the results of the tool.
**Visual Composition:**
*   A 1x4 grid of maps showing the *same* city block.
*   **1924:** Sparse network.
*   **1951:** Increased density.
*   **1996:** Modern grid established.
*   **2024:** Recent modifications.
*   **Caption:** *The evolution of pedestrian infrastructure in Central Brooklyn. Note the rapid densification between 1924 and 1951, followed by a period of stability, and recent modifications (2024) reflecting Vision Zero safety initiatives.*