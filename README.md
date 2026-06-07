# Sylhet-Watershed-Analysis
ArcGIS Pro project mapping out the stream networks and watershed boundaries of the Sylhet Basin. Used USGS DEM data to calculate flow pathways and drainage density, helping pinpoint exactly which areas face the highest risk of flash floods.
markdown
# Mapping Flash Flood Risks in the Sylhet Basin using ArcGIS Pro

This project maps out the stream networks and watershed boundaries of the Sylhet Basin. By processing elevation data, calculating flow pathways, and analyzing drainage density, this model pinpoints exactly which areas face the highest risk of sudden flash floods from across transboundary borders.


## 🛠️ Step-by-Step Project Workflow

### 1. Data Processing & Delineation
* **DEM Acquisition:** Downloaded the digital elevation model (DEM) tiles covering northeast Bangladesh natively from the [USGS EarthExplorer](https://earthexplorer.usgs.gov/).
* **Surface Correction:** Merged the separate elevation tiles into a single continuous layer and ran the **Fill** tool to smooth out artificial sinks or depressions.
* **Flow Mapping:** Generated **Flow Direction** and **Flow Accumulation** grids to model exactly how rainwater routes across the terrain.



[Raw 30m DEM] ──► Fill ──► Flow Direction ──► Flow Accumulation


* **Basin Extraction:** Applied the **Basin** tool and converted the raster outputs into vector formats using **Raster to Polygon** to identify all regional catchments.

![Regional Basin Map](Basins.png)

* **Isolating the Study Area:** Clipped out the largest continuous catchment boundary to focus the study specifically on the primary Sylhet Basin hydrologic system.

![Isolated Watershed Footprint](Watershed.png)

---

### 2. Stream Network Extraction
To cleanly separate permanent river channels from minor surface puddles or temporary rainwater sheet-flow, the accumulated cell layer was filtered through the **Raster Calculator** using a conditional 600-pixel cutoff:

spatial_analysis
"Flow_Accumulation" > 600


> **Why 600 pixels?** This threshold filters out small surface runoff drainage lines, ensuring that only core stream channels with a solid upstream drainage footprint are extracted.

* **Vectorizing Streams:** Converted the cell pathways into crisp vector lines using the **Raster to Polyline** tool to finalize the river network map.

---

## 📊 Geometric Calculations & Key Results

Using the finalized vector layers, I accessed the layer attribute tables and applied the **Calculate Field** tool to run real-world geometric calculations.

### Attribute Table Data Extraction

1. **Stream Lengths:** Calculated individual stream segment lengths inside the polyline attribute table, converting raw map units into standard kilometers (**km**).

2. **Watershed Footprint:** Calculated the total surface area and total stream density of the isolated catchment polygon using the spatial metrics.

### Final Catchment Metrics

Using the total sum of network values extracted from the attribute tables, the baseline drainage density ($D_d$) for the catchment was calculated using the standard engineering formula:

$$D_d = \frac{\text{Total Stream Length (km)}}{\text{Total Basin Area (km²)}} = \frac{4849.20\text{ km}}{3862.46\text{ km}^2} = \mathbf{1.25\text{ km/km}^2}$$

| Core Catchment Parameter | Calculated Metric Value |
| --- | --- |
| **Total Stream Length ($L$)** | ~4,849.20 km |
| **Total Basin Surface Area ($A$)** | 3,862.46 km² |
| **Average Drainage Density ($D_d$)** | **1.25 km/km²** |

---

## 🌍 Drainage Density & Flood Risk Insights

Running the **Line Density** tool split the watershed into 5 distinct drainage zones. This spatial distribution explains exactly how water behaves across the different regions of Sylhet.

To see the breakdown of these zones clearly, I extracted the area percentages into a presentation pie chart:

### What the Data Tells Us:

* **The Lowland Sponge ({0.15 - 0.8} km/km²):** Makes up the biggest chunk of the basin (**53.4%**). These flat plains and haor wetlands act like a natural sponge, safely soaking up and holding massive amounts of monsoonal rainwater.
* **The Flash Flood Funnels ({2.21 - 3.55} km/km²):** Located right along the steep northern border. These highly crowded stream channels show exactly where heavy transboundary rains rush down from the mountains, driving the sudden, dangerous flash floods that hit the Surma River system down-gradient.

```

```
