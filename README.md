# satellite-fire-learning

A small learning project exploring wildfire/burn detection from Sentinel-2 satellite imagery.

## What's here

`01_satellite_basics.ipynb` - queries Sentinel-2 imagery via the [Microsoft Planetary Computer](https://planetarycomputer.microsoft.com/) STAC API (`pystac_client` + `planetary_computer`), loads the relevant bands with `rioxarray`, and visualizes them for a given bounding box (currently a test area around south-east Queensland). Also experiments with sampling random sub-regions for testing.

## Goals

- Load and visualize Sentinel-2 bands for an area of interest
- Compute burn/fire indicators (e.g. Normalized Burn Ratio, using the NIR and SWIR bands) from the loaded imagery
- Compare pre/post-fire imagery for a known burn area

## Status

Early exploration - data loading, band access, and visualization work. No burn/fire index computation yet.

## Setup

```
pip install pystac-client planetary-computer rioxarray numpy matplotlib
```

Open `01_satellite_basics.ipynb` in Jupyter. No API key is required for public Planetary Computer STAC collections.
