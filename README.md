# Burn Scar Mapping with Sentinel-2

End-to-end burn scar mapping workflow using Sentinel-2 L2A imagery and the 
differenced Normalized Burn Ratio (dNBR) methodology. All processing is performed 
directly on the Copernicus Data Space Ecosystem (CDSE) S3 storage using windowed 
reads — no full scene downloads required.

---

## Example: Corbières Wildfire 2025

The workflow is demonstrated on the Corbières wildfire (August 2025), which burned 
approximately 16,000 ha in the Corbières massif, southern France — the largest 
wildfire in France since 1949.

---

## Outputs

- `severity_classes.tif` — classified burn severity raster (GeoTIFF, uint8)
- `burn_perimeter.geojson` — vectorized burn perimeter (WGS84)
- `rgb_comparison.png` — pre/post fire true color comparison
- `burn_severity.png` — burn severity map
- `composite_map.png` — RGB + severity overlay + burn perimeter
- `severity_overlay.png` — severity PNG for interactive map
- Interactive folium map (displayed inline in notebook)

---

## Requirements

- CDSE account with S3 credentials ([register here](https://dataspace.copernicus.eu))
- AOI GeoJSON exported from [Copernicus Browser](https://browser.dataspace.copernicus.eu)
- Two cloud-free Sentinel-2 scenes (cloud cover < 10%) for pre- and post-event dates
- Sentinel-2 tile ID covering your AOI (verify in Copernicus Browser)

---

## Setup

1. Clone the repository
2. Create a conda environment and install dependencies:
```bash
conda create -n burnscar python=3.10
conda activate burnscar
pip install -r requirements.txt```
3. Create a `.env` file in the project root:
CDSE_ACCESS_KEY=your_access_key
CDSE_SECRET_KEY=your_secret_key
4. Open the notebook in JupyterLab:
```bash
jupyter lab
```
5. Follow the instructions in each cell

---

## Workflow

1. Define AOI and dates
2. Search CDSE OData catalogue for matching Sentinel-2 L2A scenes
3. List S3 bucket structure to locate band file paths
4. Windowed read of bands directly from CDSE S3
5. Compute NBR and dNBR
6. Classify dNBR into burn severity classes (USGS schema)
7. Vectorize burn perimeter and save as GeoJSON
8. Save classified severity raster as GeoTIFF
9. Generate visualizations and save as PNG
10. Interactive folium map

---

## Limitations

- Assumes AOI falls within a single Sentinel-2 tile
- Multi-tile support planned as future improvement
- Southern hemisphere supported via automatic UTM zone detection

---

## Author

Alexander Vollstädt, 2026
