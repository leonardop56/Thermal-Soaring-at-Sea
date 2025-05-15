# Thermal Soaring at Sea

This repository contains the Jupyter notebook `Notebook_Soaring_NS.ipynb`, which includes Python 3 scripts to analyze meteorological variables from **ERA5 reanalysis data** for the **Dutch North Sea**, focusing on **bird thermal soaring** near offshore wind farms.

Two key locations are studied:

* **IJmuiden**
* **Schiermonnikoog**  

These areas host wind farms, and the goal is to better understand how birds soar around such structures. The analysis focuses on the years **2019 and 2020**.

---

## 🌍 Data Source: ERA5 (ECMWF)

ERA5 is the latest climate reanalysis from the **European Centre for Medium-Range Weather Forecasts (ECMWF)**, providing hourly estimates of atmospheric, land-surface, and sea-state variables.

* Spatial resolution: 0.25° x 0.25° grid
* Data access: [Climate Data Store](https://cds.climate.copernicus.eu)

**ERA5 datasets used:**

* **Single levels:** [Link](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels?tab=form)
* **Pressure levels:** [Link](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-pressure-levels?tab=form)

**API example:** Available in `ERA5_request_North_Sea.py`

---

## 📦 Variables Included

From multiple NetCDF files in the `Data/` folder:

* Sea surface temperature (SST)
* 2m air temperature (Ta)
* Wind components at 10m & 100m: `u10m`, `v10m`, `u100m`, `v100m`
* Wind at 500hPa and 925hPa (from `North_Sea_2019_2020_geo.nc`)
* Surface pressure (pa)
* Surface sensible heat flux (ERA5-derived)
* Boundary layer height
* Total cloud cover
* Total precipitation (from `North_Sea_2019_2020_prec.nc`)
* Convective available potential energy (from `North_Sea_2019_2020.nc`)

---

## 🔥 Sensible Heat Flux Estimations

### **Formula 1 – Ocean surface convection/conduction (Faizal & Ahmed, 2011)**

```text
Qh = 1.88 * vel * (SST - Ta)
```

Where `vel` is 2m wind speed derived via power law:

```text
u2/u1 = (z2/z1)^P     with P = 0.11 for sea (Hsu et al., 1993)
u2m = u10m * (2/10)^0.11
v2m = v10m * (2/10)^0.11
vel = sqrt(u2m^2 + v2m^2)
```

### **Formula 2 – Lake/river model (Kalinowska, 2019)**

```text
H = cb * (pa / (100 * p0)) * f_u * (SST - Ta)
```

Where:

* cb = 0.62 \[mb/°C]
* p0 = 1013 \[mb]
* f\_u = 6.9 + 0.34 \* vel^2

### **Formula 3 – ERA5 Instantaneous Surface Sensible Heat Flux (ISHF)**

ERA5 provides ISHF as positive **downwards** (from air to surface). In this notebook, it is **negated** to represent **upward flux** (from ocean to atmosphere).

Reference: [ERA5 Parameter Description](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels?tab=overview)

---

## 🗺️ Surface Pressure Charts

The `KNMI_Weerkaarten/` folder includes weather maps from the KNMI archive for key dates:

* **14, 15, 20, 21, and 22 July 2020**
  Source: [KNMI Klimatologie](https://www.knmi.nl/nederland-nu/klimatologie/daggegevens/weerkaarten)

---

## 📑 Summary Document

A summary report is available in:
📄 `Summary.pdf`

It describes:

* Data sources and methodology
* Thermal soaring metrics
* Atmospheric conditions and patterns around the North Sea wind farms

---

## 📬 Contact

For questions, please contact:
[leonardo.porcacchia@gmail.com](mailto:leonardo.porcacchia@gmail.com)
