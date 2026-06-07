# Burn Scar Mapping with Sentinel-2

End-to-end burn scar mapping workflow using Sentinel-2 L2A imagery and the 
differenced Normalized Burn Ratio (dNBR) methodology. All processing is performed 
directly on the Copernicus Data Space Ecosystem (CDSE) S3 storage using windowed 
reads — no full scene downloads required.

> **Getting started: only edit Cell 1a (User Input). All other cells run automatically.**

---

## Examples

### Corbières Wildfire 2025
The workflow is demonstrated on the Corbières wildfire (August 2025), which burned 
approximately 16,000 ha in the Corbières massif, southern France — the largest 
wildfire in France since 1949.

![Corbières Overview](outputs/Corbi%C3%A8res%20Wildfire%202025/Corbi%C3%A8res%20Wildfire%202025_overview.png)

### Bulahdelah Wildfire 2025
Second example demonstrating the workflow on a wildfire in New South Wales, Australia — 
validating southern hemisphere UTM zone detection (EPSG:327XX).

![Bulahdelah Overview](outputs/Bulahdelah%20Wildfire%202025/Bulahdelah%20Wildfire%202025_overview.png)

---

## Outputs

Each run creates a subfolder under `outputs/{event_title}/` containing:

- `{event_title}_severity_classes.tif` — classified burn severity raster (GeoTIFF, uint8)
- `{event_title}_burn_perimeter.geojson` — vectorized burn perimeter (WGS84)
- `{event_title}_overview.png` — three-panel figure: pre/post RGB + severity map
- `{event_title}_static_map.png` — cartographic map with OSM basemap, scale bar, north arrow
- `{event_title}_severity_overlay.png` — severity PNG for interactive folium map
- Interactive folium map (displayed inline in notebook)

---

## Requirements

- Free [CDSE account](https://dataspace.copernicus.eu) with S3 credentials
- AOI GeoJSON exported from [Copernicus Browser](https://browser.dataspace.copernicus.eu)
- Two cloud-free Sentinel-2 L2A scenes (cloud cover < 10%) for pre- and post-event dates
- Sentinel-2 tile ID covering your AOI (verify in Copernicus Browser)

> **Note:** AOI must fall entirely within a single Sentinel-2 tile.

---

## Setup

1. Clone the repository:
```bash
git clone https://github.com/AlexVollstaedt/burn_scar_analysis.git
cd burn_scar_analysis
```

2. Create a conda environment and install dependencies:
```bash
conda create -n burnscar python=3.10
conda activate burnscar
pip install -r requirements.txt
```

3. Create a `.env` file in the project root:
CDSE_ACCESS_KEY=your_access_key
CDSE_SECRET_KEY=your_secret_key
4. Open the notebook in JupyterLab:
```bash
jupyter lab burn_scar_mapping.ipynb
```

5. Edit **Cell 1a** with your AOI, dates, tile ID and event title

6. Run all cells: **Kernel → Restart & Run All**

---

## Workflow

1. Define AOI and search timeframe
2. Search CDSE OData catalogue for matching Sentinel-2 L2A scenes
3. List S3 bucket structure to locate band file paths
4. Windowed read of bands directly from CDSE S3
5. Compute NBR and dNBR
6. Classify dNBR into burn severity classes (USGS schema)
7. Vectorize burn perimeter and save as GeoJSON
8. Save classified severity raster as GeoTIFF
9. Generate visualizations and save as PNG
10. Interactive burn scar map (folium, inline)

---

## Limitations

- AOI must fall entirely within a single Sentinel-2 tile — multi-tile support planned
- Southern hemisphere fully supported via automatic UTM zone detection (EPSG:327XX)

---

## Author

Alexander Vollstädt, 2026