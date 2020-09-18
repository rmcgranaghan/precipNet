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
- time_hist2.py
    - Function to calculate the time history of OMNI data (solar wind and geomagnetic indices) given data frame 
- Existing resources from the ISSI team: https://github.com/rmcgranaghan/ISSI_geospaceParticles
- New resources will appear here as they are prepared


