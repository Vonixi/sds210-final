# SDS210 Project: ZueriWieNeu - Understanding Infrastructure Complaints in Zurich

This project analyzes public ZueriWieNeu reports in the City of Zurich. It uses report locations, timestamps, categories, statuses, and Zurich statistical neighbourhood boundaries to explore spatial and temporal reporting patterns.

## Objective

The main goal is to understand how ZueriWieNeu reports are distributed across Zurich neighbourhoods and issue categories. The analysis answers these research questions:

- Which neighbourhoods receive the most reports?
- Which problem categories are most common city-wide?
- Are certain issue types concentrated in particular neighbourhoods?
- How does reporting vary over time?
- Are strongly worded reports spatially clustered?
- Has the category profile changed over time?
- Which neighbourhoods have the highest report density?

## Data

Input files are stored in `data/`:

- `stzh.zwn_meldungen_p.csv`  
  ZueriWieNeu point reports containing timestamps, coordinates, issue categories, statuses, text fields, and report URLs.  
  Source: [City of Zurich Open Data – ZueriWieNeu Dataset](https://data.stadt-zuerich.ch/dataset/geo_zueri_wie_neu)

- `zh_neighbourhoods.gpkg`  
  Zurich statistical neighbourhood boundaries used for spatial joins and neighbourhood-level analysis.  
  Source: [City of Zurich Open Data – Statistical Neighbourhoods](https://data.stadt-zuerich.ch/dataset/geo_statistische_quartiere)

All datasets remain publicly available through the City of Zurich Open Data Portal.

The spatial workflow uses Swiss coordinates (`EPSG:2056`). Raw data are treated as read-only; cleaning, filtering, joining, and aggregation happen programmatically in the notebook.

## Workflow

Run the notebooks from top to bottom:

1. `notebooks/01_exploring.ipynb`: first inspection of the available tables and variables.
2. `notebooks/02_analysis.ipynb`: cleaned analysis workflow with defensive checks, maps, plots, and interpretation below each result.

The analysis notebook validates required files, columns, CRS, and spatial overlap before performing the spatial join.

## Outputs

Generated figures are saved in `outputs/`:

- `reports_map.png`: report point locations over Zurich neighbourhoods.
- `reports_per_neighbourhood_map.png`: raw report counts by neighbourhood.
- `strong_wording_map.png`: share of reports flagged by the strong-wording proxy.
- `report_density_map.png`: reports per square kilometre by neighbourhood.

## Reproducing the Environment

This project uses a Conda environment because geospatial Python packages depend on GDAL, PROJ, and GEOS.

Create the environment:

```bash
conda env create -f environment.yml
```

Activate it:

```bash
conda activate sds210-project
```

Then start Jupyter and run the notebooks from the project root:

```bash
jupyter lab
```

## Main Assumptions

- Report coordinates are interpreted as Swiss LV95 coordinates (`EPSG:2056`).
- Neighbourhood assignment is based on a point-in-polygon spatial join.
- Strong wording is measured with a transparent keyword and punctuation proxy that was generated with the help of LLM ChatGPT-5.
- Density is calculated as report count divided by neighbourhood area in square kilometres.
