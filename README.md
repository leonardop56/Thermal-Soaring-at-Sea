Create by: Leonardo Porcacchia, 08-10-2021  
Edited by: Leonardo Porcacchia, 10-02-2025  
leonardo.porcacchia@gmail.com


The Jupyter Notebook “Notebook_Soaring_NS” is a collection of Python3 scripts aimed at importing meteorological variables from ECMWF Reanalysis v5 (ERA5) for the Dutch North Sea and analyzing them in relation to bird thermal soaring. Two locations are considered (IJmuiden and Schiermonnikoog) were wind farms are present since we are interested in bird behavior around these structures. The study is limited to the years 2019 and 2020.

ERA5 is a climate reanalysis produced by the European Center for Medium Range Weather forecast (ECMWF), providing hourly data on many atmospheric, land-surface and sea-state parameters together with estimates of uncertainty. ERA5 data are available in the Climate Data Store https://cds.climate.copernicus.eu/cdsapp#!/home on regular latitude-longitude grids at 0.25 deg x 0.25 deg resolution, with atmospheric parameters on 37 pressure levels.

In this notebook several variables from ERA5 are considered:   
- sea surface temperature (SST)  
- atmospheric temperature at 2m (Ta)  
- wind u and v components at 10m and 100m (u10m, v10m, u100m, v100m)   
- mean sea level pressure (pa)  
- surface sensible heat flux  
- boundary layer height  
- convective available potential energy (file *North_Sea_2019_2020.nc* in the *Data* folder)  
- total cloud cover  
- total precipitation (file *North_Sea_2019_2020_prec.nc*)  
- wind u and v components at 500hPa and 925hPa pressure levels (file *North_Sea_2019_2020_geo.nc*)  

The first two sets of data can be downloaded from ERA5 hourly data on single levels at:  
https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels?tab=form

The last set of data can be donloaded from ERA5 hourly data on pressure levels at:  
https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-pressure-levels?tab=form
 
An example of API request to download the same data from terminal is in *ERA5_request_North_Sea.py*.

**Estimates of Sensible Heat Flux**  
Following a power law for wind speed at level z1 and z2 (u1 and u2, respectively)  
<pre>
u2/u1 = (z2/z1)P               P=0.11 at sea, Hsu et al. (1993)    
u2m = u10m [(2/10)^0.11]       2m u-component of wind speed    
v2m = v10m [(2/10)^0.11]       2m v-component of wind speed  
vel = sqrt(u2m2 + v2m2)        2m wind speed  
</pre>
 
1) Formula 1    
Rate of sensible heat loss from ocean surface by convection and conduction Qh [Wm-2]    
This approximation is valid for Pacific Ocean (Mohammed Faizal & Ahmed, 2011)  
Qh = 1.88 vel (SST - Ta)

2) Formula 2      
Convection and conduction heat flux H [Wm-2]    
This approximation is valid mostly for lakes and rivers (Kalinowska, 2019)  
The function of wind comes from Brady et al. (1969), Ahsan and Blumberg (1999), Arifin et al. (2016) and Ji (2008)  
H = cb (pa/(100 p0)) f_u (SST - Ta)  
where  
cb   = 0.62                        [mb/degC]  
p0   = 1013                        [mb]  
f_u = 6.9 + 0.34 (vel2)            [Wm-2mb-1]  

4) Formula 3  
ERA5’s Instantaneous Surface Sensible Heat Flux [Wm-2]. “This parameter is the transfer of heat between the Earth's surface and the atmosphere, at the specified time, through the effects of turbulent air motion (but excluding any heat transfer resulting from condensation or evaporation). The magnitude of the sensible heat flux is governed by the difference in temperature between the surface and the overlying atmosphere, wind speed and the surface roughness. For example, cold air overlying a warm surface would produce a sensible heat flux from the land (or ocean) into the atmosphere. The ECMWF convention for vertical fluxes is positive downwards." (https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels?tab=overview).

The ISHF used in the notebook is defined as the negative of this parameter to represent positive upwards fluxes (from the sea to the atmosphere).

Plots of time series of sensible heat flux estimates, for Ijmuiden and for Schiermonnikoog, and for July 2020, are in the Plot folder, along with other time series plots generated in the notebook. 
The KNMI Weerkaarten folder within the Plots folder contains surface pressure charts downloaded from the KNMI website https://www.knmi.nl/nederland-nu/klimatologie/daggegevens/weerkaarten for the days 14-15-20-21-22/07/2020.

Finally, a Summary has been uploaded to the repository that describes the methodology used and presets my results. It also provides a general picture of the atmospheric conditions related to bird soaring over the North Sea.
