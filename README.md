# precipNet

This software repository contains code to create and interrogate the 'precipNet' machine learning model developed to specify magnetospheric particle precipitation into the ionosphere as observed by the Defense Meteorological Satellite Program (DMSP) spacecraft SSJ 4/5 observations. 

Development and details of precipNet are provided here (link to paper forthcoming). 

Previous work by the International Space Sciences Institute (ISSI) team "[Novel approaches to multiscale geospace particle transfer: Improved understanding and prediction through uncertainty quantification and machine learning](https://www.issibern.ch/teams/multigeopartransfer)" laid the foundation for this work and provides many useful resources.

### Dependencies
- [OvationPyme](https://github.com/lkilcommons/OvationPyme)
- [geospacepy](https://github.com/lkilcommons/geospacepy-lite)
- [nasaomnireader](https://github.com/lkilcommons/nasaomnireader)
- Keras version 2.3.0 ([implementation of the Keras API in TensorFlow](https://www.tensorflow.org/api_docs/python/tf/keras))
- TensorFlow version 2.2.0

### Notebooks and Scripts
- Precipitation_Model_Evaluation_Utilities.ipynb
    - Functions to calculate auroral boundaries and hemispheric powers given global high-latitude energy flux maps 
- standard_assessment_metrics_function.ipynb
    - Function to calculate the standard assessment metrics, the set of which follows guidance for geospace given by [Liemohn et al., 2018](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2018SW002067)
- time_hist2.py
    - Function to calculate the time history of OMNI data (solar wind and geomagnetic indices) given data frame 
- Existing resources from the ISSI team: https://github.com/rmcgranaghan/ISSI_geospaceParticles
- New resources will appear here as they are prepared

### Database creation: 
- The central data file used in the scripts is titled ''ML_DB_subsamp.csv'' and is provided here (DOI to published dataset forthcoming). The steps to create those data were. We do not provide intermediate datasets: 
    1. Access NASA-provided DMSP data at https://cdaweb.gsfc.nasa.gov/pub/data/dmsp/
    2. Read CDF files for given satellite (e.g., F-16)
    3. Collect the following variables at one-second cadence: SC_AACGM_LAT, SC_AACGM_LTIME, ELE_TOTAL_ENERGY_FLUX, ELE_TOTAL_ENERGY_FLUX_STD, ELE_AVG_ENERGY, ELE_AVG_ENERGY_STD, ID_SC
    4. Sub-sample the variables to one-minute cadence and eliminate any rows for which ELE_TOTAL_ENERGY_FLUX is NaN
    5. Combine all individual satellites into single yearly files
    6. For each yearly file, use [nasaomnireader](https://github.com/lkilcommons/nasaomnireader) to obtain solar wind and geomagnetic index data programmatically and [timehist2](https://github.com/rmcgranaghan/ISSI_geospaceParticles/blob/master/time_hist2.py) to calculate the time histories of each parameter. Collate with the DMSP observations and remove rows for which any solar wind or geomagnetic index data are missing. 
    7. For each row, calculate cyclical time variables (e.g., local time -> sin(LT) and cos(LT))
    8. Merge all years
