# IidaPY: Instructions for Windows

## Install build tools

1. Install [Microsoft Visual C++ Build Tools](https://visualstudio.microsoft.com/es/visual-cpp-build-tools); select the option "Desktop development with C++".

## Install Miniconda

1. Install [Miniconda](https://docs.conda.io/en/latest/miniconda.html); choose the **administrator option**.
2. Create user environment variables:

```powershell
C:\ProgramData\Miniconda3
C:\ProgramData\Miniconda3\Scripts
C:\ProgramData\Miniconda3\Library\bin
```

## Create virtual environment

1. As a **normal user**, open a terminal and create a new virtual environment:

```bash
conda create -n iida
```

2. Activate the virtual environment:

```bash
conda activate iida
```

## Install dependencies

1. Install dependencies:

```bash
conda install -c conda-forge python=3.8 cython gdal xarray
```

2. Clone the following repositories:

```bash
git clone https://github.com/radosuav/pyDMS.git
git clone https://github.com/hectornieto/pypro4sail.git
git clone https://github.com/hectornieto/pyTSEB.git
git clone https://github.com/ffuentesp7/pyMETRIC.git
```

3. Open a new terminal **as an administrator** and install the cloned packages:

```bash
cd pyDMS
python setup.py install
cd ../pypro4sail
python setup.py install
cd ../pyTSEB
python setup.py install
cd ../pyMETRIC
python setup.py install
```

## *Manual run*

1. Go to the repository's root folder and prepare your **NDVI, LAI and land surface temperature** GeoTIFF files inside a new folder called *Input*. Also, create a second folder called *Output*.
2. Create a file called *Config_LocalImage.txt* with the following content; adapt it to your needs:

```txt
# Model to run: 
model=pyMETRIC

#==============================================================================
# Input Files with full path (any valid gdal raster file is accepcted (not HDF or NETCDF!!!)
# Use forward slash '/' for path separators, even using Windows
#==============================================================================

# Input land surface temperature in Kelvin # mandatory file
T_R1=./Input/ExampleImage_Trad_pm.tif

# Processing Mask (boolean) # Optional, type a full-path file for processing only on non-masked pixels (all pixels with values > 0 in the mask image will be processed)
input_mask=0

# Effective Leaf Area Index (m2/m2) # Optional, type either a full-path file or a single value for a constant value acroos the area
LAI=./Input/ExampleImage_LAI.tif

# NDVI constant value acroos the area
VI=./Input/ExampleImage_NDVI.tif

# Vegetation Fractional Cover # Optional, type either a full-path file or a single value for a constant value acroos the area
f_c=0.4

# Canopy height (m)# Optional, type either a full-path file or a single value for a constant value along the area
h_C=2.4

# Canopy height/with ratio (wc/hc) # Optional, type either a full-path file or a single value for a constant value along the area
w_C=1
 
#==============================================================================
# Output File
#==============================================================================
# full path to output directory, The output GeoTIFF files will be stored in that folder with a standard name : Output_<TSEB_MODEL>.tif (4 bands: Rn, H, LE, G) and Output_<TSEB_MODEL>_ancillary.tif
output_file=./Output/test_image.vrt

#==============================================================================
# Site Description
#==============================================================================
lat=38.289355	
lon=-121.117794
alt=97
stdlon=-105.0
z_T=5
z_u=5


#==============================================================================
# Meteorology
#==============================================================================
DOY=221	
time=10.9992	
T_A1=299.18
u=2.15
p=1011
ea=13.4
S_dn=861.74
L_dn=

#==============================================================================
# Canopy and Soil spectra
#==============================================================================
emis_C=0.98 # leaf emissivity
emis_S=0.95 # soil emissivity

# Leaf spectral properties:{rho_vis_C: visible reflectance, tau_vis_C: visible transmittance, rho_nir_C: NIR reflectance, tau_nir_C: NIR transmittance}
rho_vis_C=0.07
tau_vis_C=0.08
rho_nir_C=0.32
tau_nir_C=0.33 

# Soil spectral properties:{rho_vis_S: visible reflectance, rho_nir_S: NIR reflectance}
rho_vis_S=0.15
rho_nir_S=0.25

#==============================================================================
# Canopy and soil parameters
#==============================================================================
# Cambpbell 1990 leaf inclination distribution parameter:[x_LAD=1 for spherical LIDF, x_LAD=0 for vertical LIDF, x_LAD=float(inf) for horzontal LIDF]  
x_LAD=1

# Primary land cover IGBP Land Cover Type Classification: CROP=12, GRASS=10, SHRUB=6, CONIFER=1, BROADLEAVED=4
landcover=12


#==============================================================================
# Resistances
#==============================================================================
use_METRIC_resistance=1   # Resistance formulations: 0 - Kustas & Norman 1999; 1 - Allen 1998
# Bare soil roughness lenght (m)
z0_soil=0.01 


#==============================================================================
# Additional options
#==============================================================================
# Soil Heat Flux calculation
#1: default, estimate G as a ratio of Rn_soil, default G_ratio=0.35
#0: Use a constant G, usually use G_Constant=0 to ignore the computation of G
#2: estimate G from Santanello and Friedl with GAmp the maximum ration amplitude, Gphase, the time shift between G and Rn (hours) and Gshape the typical diurnal shape (hours)
#4: Use ASCE PET G ratios for either tall or short vegetation
G_form=4
G_tall=0.04
G_short=0.10

#==============================================================================
# METRIC optons
#==============================================================================
# Search method 
# 0 : Allen et al. 2013: calibration using inverse modeling at extreme conditions (CIMEC)
# 1 : Bhattarai et al. 2017: exhaustive search algorithm (ESA)
# 2 : Minimum and maximum temperatures
endmember_search=2
ETrF_bare=0
```

3. Open a new terminal and run:

```powershell
python METRIC_local_image_main.py
```
