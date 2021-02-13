# precipNet

This software repository contains code to create and interrogate the 'precipNet' machine learning model developed to specify magnetospheric particle precipitation into the ionosphere as observed by the Defense Meteorological Satellite Program (DMSP) spacecraft SSJ 4/5 observations. 

Development and details of precipNet are provided here ([link to preprint](https://arxiv.org/abs/2011.10117)). 

Previous work by the International Space Sciences Institute (ISSI) team "[Novel approaches to multiscale geospace particle transfer: Improved understanding and prediction through uncertainty quantification and machine learning](https://www.issibern.ch/teams/multigeopartransfer)" laid the foundation for this work and provides many useful resources.

### Dependencies
- [OvationPyme](https://github.com/lkilcommons/OvationPyme) (note that for purposes of reproducibility have tagged the specific Github commit of OvationPyme used for the manuscript, which can be obtained [here](https://github.com/lkilcommxons/OvationPyme/releases/v0.1.1)
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
- Final__Data_Read_And_Prepare.ipynb
    - Sample notebook revealing how to read in the database and prepare it for machine learning investigation
- Existing resources from the ISSI team: https://github.com/rmcgranaghan/ISSI_geospaceParticles
- New resources will appear here as they are prepared

### Database creation: 
- The central data file used in the scripts is titled ''ML_DB_subsamp.csv'' and is provided here (DOI to published dataset forthcoming). The steps to create those data were (note that we do not provide intermediate datasets): 
    1. Access NASA-provided DMSP data at https://cdaweb.gsfc.nasa.gov/pub/data/dmsp/
    2. Read CDF files for given satellite (e.g., F-16)
    3. Collect the following variables at one-second cadence: SC_AACGM_LAT, SC_AACGM_LTIME, ELE_TOTAL_ENERGY_FLUX, ELE_TOTAL_ENERGY_FLUX_STD, ELE_AVG_ENERGY, ELE_AVG_ENERGY_STD, ID_SC
    4. Sub-sample the variables to one-minute cadence and eliminate any rows for which ELE_TOTAL_ENERGY_FLUX is NaN
    5. Combine all individual satellites into single yearly files
    6. For each yearly file, use [nasaomnireader](https://github.com/lkilcommons/nasaomnireader) to obtain solar wind and geomagnetic index data programmatically and [timehist2](https://github.com/rmcgranaghan/ISSI_geospaceParticles/blob/master/time_hist2.py) to calculate the time histories of each parameter. Collate with the DMSP observations and remove rows for which any solar wind or geomagnetic index data are missing. 
    7. For each row, calculate cyclical time variables (e.g., local time -> sin(LT) and cos(LT))
    8. Merge all years
    
The database for this work is considered ['Artificial Intelligence (AI)-ready'](https://github.com/rmcgranaghan/data_science_tools_and_resources/wiki/Curated-Reference%7CChallenge-Data-Sets) and can serve as a 'challenge data set' for further exploration. It has been published on Zenodo. If used, please cite:
   
    McGranaghan, R. M., Ziegler, J., Bloch, T., Camporeale, E., Lynch, K., Owens, M., . . . Skone, S. (2020, November). Dmsp particle precipitation ai-ready data. Zenodo. Retrieved from https://doi.org/10.5281/zenodo.4281122 doi: 10.5281/zenodo.4281122
    
    
### Fruitful paths for ML investigation:
We believe it helpful to prioritize such next steps based on our experience and to inspire active extension of this work. We envision that those from the ML practitioner community may particularly benefit from the recommendations. Below is a prioritized list of ML investigations with accompanying justification for each recommendation ranking: 
- Develop an understanding of importance of auroral boundaries information to the prediction of particle precipitation and to the specification of the entire magnetosphere-ionosphere-thermosphere system. Auroral boundaries organize the high-latitudes and therefore the magnetosphere-ionosphere coupling. The regions are distinguished by different coupling to the magnetosphere, different behavior of the ionosphere-thermosphere, and are reflected by distinct particle precipitation characteristics. We have attempted here to understand the model's capability to specify the auroral boundaries, but the question remains about the information content of auroral boundary data. [Hardy et al., [2008]](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2007JA012746) discovered that the boundaries are the organization that separates precipitation populations, so it stands to reason that they would also be important to improving precipitation models. Two investigations are recommended: the extent to which auroral boundary data are the key to improved precipitation models and the improvement to GCMs possible with improved boundaries. 
- Explore the transfer of knowledge from trained ML models to new applications for space weather. Transfer learning is the process of storing knowledge gained while solving one problem and applying it to a different but related problem. Many space weather applications share characteristics, so transfer learning may offer a framework to spread information more effectively. The question to answer in this investigation is how transferring knowledge can improve model capability? There is indeed some precedent [Clausen and Nickisch, [2018]](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2018JA025274).
\end{enumerate}
