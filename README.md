# satellite-fire-learning

A learning project on wildfire and burn scar mapping from Sentinel-2 satellite imagery.

## Notebooks

### `01_satellite_basics.ipynb`
Queries Sentinel-2 imagery through the [Microsoft Planetary Computer](https://planetarycomputer.microsoft.com/) STAC API (`pystac_client` + `planetary_computer`), loads the bands with `rioxarray` and shows them for an area of interest around south-east Queensland.

### `02_burnt_area_mapping_random_forest.ipynb`
Burnt area mapping built on Digital Earth Australia's
[Burnt area mapping using Sentinel-2 data](https://knowledge.dea.ga.gov.au/notebooks/Real_world_examples/Burnt_area_mapping/) notebook, with a machine learning part added.

- Loads **DEA Sentinel-2 ARD (Collection 3)** straight from DEA's public STAC catalogue with `odc-stac`, with Fmask cloud filtering done by hand, so it runs locally without the DEA Sandbox.
- Recreates the DEA method: pre-fire and post-fire median composites, NBR, dNBR, a 0.3 threshold, and the burnt area in km² for the Clyde Mountain fire (NSW, Black Summer 2020). I got **95.6 km²**; the DEA notebook reports 96.6 km².
- Adds a **Random Forest classifier** (scikit-learn) trained on confident dNBR pseudo-labels, using a **spatial block train/test split**:
  - Model A: pre-fire + post-fire features, 100% on held-out blocks. This is a sanity check, since dNBR is both an input and the source of the labels.
  - Model B: **post-fire image only**, 98.8% accuracy (kappa 0.97) on held-out blocks.
- Permutation feature importance.
- **Transfer test** on the Binna Burra / Sarabah fire (Gold Coast hinterland, QLD, Sept 2019), a fire the models never saw in training.

Related: [fire-detection-neural-nets](https://github.com/mrseabag/fire-detection-neural-nets), my comparison of YOLOv7 and Detectron2 for detecting active forest fires.

## Setup

```
pip install -r requirements.txt
```

No API keys are needed. Planetary Computer's public collections and DEA's `dea-public-data` S3 bucket are both open (notebook 02 sets `AWS_NO_SIGN_REQUEST=YES`).

## Credits

The burnt area method, case study and thresholds in notebook 02 are adapted from Geoscience Australia's DEA notebooks ([dea-notebooks](https://github.com/GeoscienceAustralia/dea-notebooks), Apache 2.0). Data: Geoscience Australia Sentinel-2 ARD Collection 3 (CC BY 4.0).
