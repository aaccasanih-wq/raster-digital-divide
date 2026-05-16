# Territorial Digital Divide — Geospatial Raster Analysis | Cusco, Peru

## Project Description and Research Question

This project builds a geospatial analysis pipeline to measure the **territorial digital divide** in the Cusco region of Peru. By cross-referencing two satellite-derived raster datasets — NASA nighttime lights (VNL 2025) as a proxy for urbanization and OSIPTEL mobile network coverage density (2019) as a proxy for internet access — the pipeline produces four thematic maps and a territorial classification that expose inequality patterns between connected urban zones and digitally excluded rural areas.

**Research question:** To what extent does the distribution of mobile connectivity in the Cusco region follow the pattern of urbanization measured by nighttime light intensity, and where are the critical gaps of digital exclusion?

---

## Dependencies and Installation

### Option A — Conda environment (recommended)

```bash
conda create -n raster-digital-divide python=3.11
conda activate raster-digital-divide
conda install -c conda-forge rasterio numpy matplotlib scipy seaborn pandas jupyter
```

### Option B — pip

```bash
pip install -r requirements.txt
```

---

## Input Data

Download the two raster files from the Google Drive folder and place them inside the `data/` directory before running the notebook. **Do not commit these files to the repository.**

| File | Description | CRS |
|------|-------------|-----|
| `VNL_cusco_2025.tif` | NASA Black Marble nighttime radiance (nW·cm⁻²·sr⁻¹) | EPSG:4326 |
| `kernel_cobmovil2019_50m.tif` | OSIPTEL mobile coverage kernel density | EPSG:32719 |

---

## How to Run the Notebook End-to-End

1. Clone the repository and set up the environment (see above).
2. Download the input rasters and place them in `data/`.
3. Launch Jupyter and open the notebook:

```bash
conda activate raster-digital-divide
jupyter notebook notebooks/digital_divide_cusco.ipynb
```

4. Run all cells in order from top to bottom (**Kernel → Restart & Run All**).  
   All output files will be generated automatically in the `output/` folder.

---

## Output Files

| File | Description |
|------|-------------|
| `output/vnl_norm.tif` | Normalized NASA nighttime lights raster [0–1], percentile-clipped (p2–p98), aligned to VNL grid |
| `output/conn_norm.tif` | Normalized OSIPTEL mobile coverage raster [0–1], reprojected from EPSG:32719 to EPSG:4326 and resampled to VNL grid |
| `output/ibd_brecha_digital.tif` | Digital Divide Index (IBD = VNL\_norm − Conn\_norm), range [−1, 1]; positive values indicate zones with more light than connectivity |
| `output/clasificacion_brecha.tif` | 4-class territorial classification raster (1 = Urban Connected, 2 = Urban Divide, 3 = Rural Connected, 4 = Critical Divide) |
| `output/dashboard_brecha_digital.png` | Composite dashboard (3×3 grid) summarizing all indices and maps at 150 dpi |

---

## Main Findings

The analysis reveals that digital exclusion in Cusco is not the exception but the norm: the vast majority of the territory falls into the **Critical Divide** category (VNL < 0.15 and connectivity < 0.15), and **87% of all pixels reach the maximum Social Exclusion Risk score of 1.0**, meaning they have neither detectable nighttime light nor mobile coverage. Most striking is the intervention priority layer: **34,522 pixels (3.32% of the territory) are classified as P3 Critical** — zones with high urbanization (VNL ≥ 0.30) but virtually zero connectivity (Conn < 0.10), representing the most urgent and unjust gaps where population density is clearly visible from space yet mobile internet is absent. The Pearson correlation between nighttime light intensity and mobile coverage is only moderate (r ≈ 0.35), confirming that urbanization and digital access do not overlap spatially — a significant share of populated areas remains disconnected from mobile networks.
