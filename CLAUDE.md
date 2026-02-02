# CLAUDE.md

This file provides guidance for AI assistants working with this repository.

## Project Overview

This repository creates a **2020 Census Tract to 2010 Vintage PUMA Crosswalk for New York City**. It uses geospatial analysis with Python (GeoPandas) to overlay 2020 Census tract boundaries with 2010 PUMA boundaries, determining which census tracts fall within which Public Use Microdata Areas (PUMAs).

### Key Terminology

- **PUMAs (2010)**: Census-defined boundaries that roughly approximate NYC's Community Districts, each covering at least 100,000 residents. NYC has 55 PUMAs.
- **Census Tracts (2020)**: Small, permanent geographical units covering 1,200-8,000 people each. NYC has 2,325 census tracts as of 2020.

## Repository Structure

```
nyc-crosswalk-2020tract-to-2010puma/
├── README.md                                              # Project documentation
├── CLAUDE.md                                              # This file - AI assistant guidance
├── crosswalk_nyc_tract2020_to_puma2010.ipynb             # Main Jupyter notebook with analysis
├── census_tract_2020_to_puma_2010_crosswalk_tabular.csv  # CSV output (2,323 rows)
├── census_tract_2020_to_puma_2010_crosswalk_shapefile.*  # Shapefile outputs (5 files + zip)
└── .ipynb_checkpoints/                                    # Jupyter checkpoints (auto-generated)
```

## Key Files

| File | Purpose |
|------|---------|
| `crosswalk_nyc_tract2020_to_puma2010.ipynb` | Main analysis notebook - contains all code and methodology |
| `census_tract_2020_to_puma_2010_crosswalk_tabular.csv` | Tabular crosswalk output (non-geographic) |
| `census_tract_2020_to_puma_2010_crosswalk_shapefile.shp` | Shapefile with intersection geometries |

## Data Sources

Data is fetched directly from NYC Open Data Portal via API:
- [2010 PUMA Boundaries](https://data.cityofnewyork.us/Housing-Development/2010-Public-Use-Microdata-Areas-PUMAs-/cwiz-gcty)
- [2020 Census Tracts](https://data.cityofnewyork.us/City-Government/2020-Census-Tracts-Tabular/63ge-mke6)

## Development Environment

### Required Dependencies

```python
pandas
geopandas
numpy
pyproj
fiona
shapely
rtree
matplotlib
```

### Installation Note

GeoPandas and its dependencies can be complex to install. Using conda-forge is recommended:

```bash
conda install -c conda-forge geopandas
```

## Workflow and Code Patterns

### Main Analysis Pipeline

1. **Data Import**: Fetch GeoJSON from NYC Open Data Portal via REST API
2. **Area Calculation**: Convert to Mercator projection (EPSG:3395) for accurate area measurements
3. **Spatial Overlay**: Use `geopandas.overlay()` with 'intersection' method
4. **Quality Checks**: Validate borough/PUMA relationships
5. **Data Cleaning**: Remove invalid entries (tracts crossing borough boundaries incorrectly)
6. **Export**: Output to both shapefile and CSV formats

### Key Function

```python
def import_shapefile_and_area_in_sqkm(code):
    """
    Fetches GeoJSON from NYC Open Data Portal and calculates area in square km.
    Uses EPSG:3395 (Mercator) for area calculations.
    """
```

### Geographic Conventions

- **Input Projection**: WGS 84 (EPSG:4326)
- **Work Projection**: Web Mercator (EPSG:3395) for area calculations
- **Output Projection**: WGS 84 (EPSG:4326)
- **Area Unit**: Square kilometers

## Output Data Format

### CSV Columns

| Column | Description |
|--------|-------------|
| `geoid` | Unique geographic identifier |
| `ct2020` | 2020 Census Tract ID |
| `ctlabel` | Census Tract label |
| `puma` | 2010 PUMA code |
| `nta2020` | 2020 Neighborhood Tabulation Area |
| `ntaname` | NTA name |
| `cdtaname` | Community District Tabulation Area name |
| `boroname` | Borough name |
| `borocode` | Borough code |
| `area_in_sqkm_1` | Tract area in sq km |
| `area_in_sqkm_2` | PUMA area in sq km |
| `intersection_sqkm` | Intersection area in sq km |
| `tract_percent_of_puma_km` | Percentage of tract within PUMA |

### Statistics

- Total crosswalk entries: 2,323
- Total unique PUMAs: 55
- Total area covered: ~1,350 sq km

## Common Tasks

### Re-running the Analysis

1. Open `crosswalk_nyc_tract2020_to_puma2010.ipynb` in Jupyter
2. Run all cells in order
3. Outputs are automatically saved to CSV and shapefile

### Modifying the Analysis

- The intersection threshold (10%) can be adjusted in the filtering step
- Additional columns can be preserved by modifying the column selection
- Different projections can be used by changing the EPSG codes

## Notes for AI Assistants

1. **This is a data analysis project** - The main code is in a Jupyter notebook, not traditional Python modules
2. **API-dependent** - Data fetching requires internet access to NYC Open Data Portal
3. **Geospatial focus** - Understanding of GIS concepts (projections, shapefiles, spatial overlays) is helpful
4. **Quality-focused** - The code includes QA checks for edge cases like tracts crossing borough boundaries
5. **Output files are committed** - Both the notebook and output data files are tracked in git

## Git Workflow

- Main branch: `main`
- Commit messages should describe what changed and why
- Output files (CSV, shapefiles) should be regenerated if the analysis changes
