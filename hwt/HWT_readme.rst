**System Description**
======================

Verification and scorecards during the HWT 2018 and HWT 2019 Spring
Experiments were run mostly using the METplus system.  The METplus
system is a set of python wrappers that run different programs from the
Model Evaluation Tools package. All Metplus files are stored in the
METplus directory currently located in /home/christina.kalb/METplus

The system is configured such that multiple configuration files are
linked to get the desired output.  The reasoning for this is to avoid
having the same configuration parameter appear in multiple files. The
exceptions are the multiple time configuration files.  As different time
steps and start/end times were needed for the hourly data, 3 hourly
precipitation, and 24 hour precipitation accumulations, the start and
end times appear multiple times in these files.  

The order in which configuration files are listed is important for the 3
hourly and 24 hourly accumulated precipitation.  In all other cases, the
order is irrelevant. However, to avoid issues, it’s easiest to follow
the order of configuration files listed here:

Base Configuration file(s) , model configuration file, MET tool
configuration file, time configuration file, optional regridding
file(s), optional ensemble mean precipitation file

The different MET tools and configuration files used are described later
in the README.  In addition, three scripts were run outside the METplus
system. These will also be discussed at the end of the README.  

**How to Run METplus**
======================

1. Edit the system configuration files

2. cd to the the METplus/ush directory

3. python (must be version 2.7 or higher) master_metplus.py -c
   configuration file 1 -c configuration file 2 -c configuration file 3
   etc…  The location of an nco executable required by the METplus
   system was different on bigbang2 and buxton2 during HWT 2019.
   Therefore, running on bigbang2 required the use of an additional
   parameter file to specify this different location.

| *Specifically for buxton2:*
| base_paths.conf, model configuration file, MET tool configuration file,
time configuration file, optional regridding file(s), optional ensemble
mean precipitation file
 
| *Specifically for bigbang2:*
| bigbang.conf, base_paths.conf , time configuration file, model
configuration file, MET tool configuration file, time configuration
file, optional regridding file(s), optional ensemble mean precipitation
file

**Configuration File Categories:**
__________________________________
| *Base Configuration files:*
| bigbang.conf
| base_paths.conf
| 
| *Model Configuration files:*
| HREFv2_apcp3.conf
| HREFv2_apcp24.conf
| HREFv2_ens.conf
| HREFv2.conf
| HRRRe_apcp3.conf
| HRRRe_apcp24.conf
| HRRRe_ens.conf
| HRRRe.conf
| HRRRv3.conf
| HRRRv4.conf
| NSSLFV3.conf
| 
| *MET tool configuration files:*
| ens_stat.conf
| grid_stat_pcp.conf
| grid_stat_refc.conf
| pb2nc.conf
| pcp_combine_grid_stat.conf
| point_stat.conf
| point_stat_ens.conf
| 
| *Time configuration files:*
| time_init_ens_apcp24_00.conf
| time_init_ens_apcp24_12.conf
| time_init_ens_apcp3.conf
| time_init_ens.conf
| time_valid_24hrly.conf
| time_valid_3hrly.conf
| time_valid_hrly.conf
| 
| *Regridding files (only needed if you want to put the model data on the same grid as another model):*
| hrrre_regrid.conf
| nsslfv3_regrid.conf
| 
| *Ensemble Mean Precipitation Files (only needed when running ensemble mean precipitation):*
| ens_mean_apcp24.conf
| ens_mean_apcp3.conf
| 
| *System configuration files:*
| There are a few configuration files that are not specific to HWT, but
still can be edited.  They are currently set up to run on buxton2 and
bigbang2. However, they may need to be edited if METplus is run from a
different machine.  They are listed here, with the relative path. 

METplus/parm/metplus_config/metplus_logging.conf:  Configuration file
containing information about the format and labeling of log files from
METplus, and how much information each log file contains

METplus/parm/metplus_config/metplus_system.conf:  Configuration file
containing system information including the location where MET tools are
installed, the logging and temporary directories, and the location of
system functions such as rm, cut, wgrib2, nco tools, etc.

**Description of MET Tools, Configuration Files and separate scripts**
======================================================================

**MET programs:**
_________________

ensemble_stat:  used to create ensemble means for temperature, dew
point, wind speed, and precipitation

grid_stat:  used to create statistics on gridded model and gridded
observation data, specifically critical success index, bias, and
fraction skill score during HWT 2019

pb2nc:  used to convert the point observations of temperature, dew
point, and wind speed from prepbufr to netCDF.

pcp_combine:  used to combine hourly precipitation files to make 3 and
24 hourly

point_stat:  used to create statistics on gridded model and point
observation data, specifically RMSE and ME during HWT 2019

stat_analysis:  used to combine statistics from multiple days

**Configuration Files (alphabetical order):**
_____________________________________________

This section contains information about the different METplus
configuration files that were used during HWT 2019.  Not all variables
in each file are described. Rather, it contains an overview of the ones
most likely to be changed.

base_paths.conf:  Configuration file contain paths to the MET
configuration files, daily domain masks, output verification files,
current yearly experiment directory, HREF, HRRRe, MRMS observations, and
prepbufr environment observations

bigbang.conf: A special configuration file for running METplus on
bigbang2 that contains the locaiton of the ncap2 executable.  It’s
location is different on buxton2 and bigbang2

ens_mean_apcp24.conf:  Configuration file containing parameters specific
to computing the 24 hourly ensemble mean precipitation.  This includes
the forecast variable name, observation variable name, observation
level, METplus configuration file, and grid_stat input directory

ens_mean_apcp3.conf:  Configuration file containing parameters specific
to computing the 3 hourly ensemble mean precipitation.  This includes
the forecast variable name, observation variable name, observation
level, METplus configuration file, and grid_stat input directory

ens_stat.conf:  Configuration file containing parameters specific to
running ensemble_stat, including the METplus process list, ensemble_stat
configuration file, output directory, and METplus configuration file.

grid_stat_pcp.conf:  Configuration file containing parameters specific
to running grid_stat on precipitation data.  This includes the
neighborhood size and shape (for fraction skill score), forecast and
observation thresholds, forecast and observation directories,
observation file templates, output directory, and the mask template

grid_stat_refc.conf:  Configuration file containing parameters specific
to running grid_stat on reflectivity data.  This includes the
neighborhood size and shape (for fraction skill score), forecast and
observation thresholds, forecast and observation directories,
observation file templates, output directory, and the mask template.

HREFv2.conf:  Configuration file containing parameters specific to
running HREFv2 ensemble mean data through grid_stat, point_stat, or
pcp_combine.  This includes the initializations times processed, min and
max forecast lead times used, model input directory and file template
and mask used.

HREFv2_apcp24.conf:  Configuration file containing parameters specific
to running HREFv2 through ensemble_stat to create ensemble means for 24
hour accumulated precipitation.  This includes the model input
directory, templates for all ensemble members, number of ensemble
members, and the ensemble_stat configuration file.

HREFv2_apcp3.conf:  Configuration file containing parameters specific to
running HREFv2 through ensemble_stat to create ensemble means for 3 hour
accumulated precipitation.  This includes the model input directory,
templates for all ensemble members, number of ensemble members, and the
ensemble_stat configuration file.

HREFv2_ens.conf:  Configuration file containing parameters specific to
running HREFv2 through ensemble_stat to create ensemble means for
temperature, dew point, and wind speed.  This includes the model input
directory, templates for all ensemble members, number of ensemble
members, and the ensemble_stat configuration file.

HRRRe.conf:  Configuration file containing parameters specific to
running HRRRe ensemble mean data through grid_stat, point_stat, or
pcp_combine.  This includes the initializations times processed, min and
max forecast lead times used, model input directory and file template
and mask used

HRRRe_apcp24.conf:  Configuration file containing parameters specific to
running HRRRe through ensemble_stat to create ensemble means for 24 hour
accumulated precipitation.  This includes the model input directory,
templates for all ensemble members, number of ensemble members, and the
ensemble_stat configuration file.

HRRRe_apcp3.conf:  Configuration file containing parameters specific to
running HRRRe through ensemble_stat to create ensemble means for 3 hour
accumulated precipitation.  This includes the model input directory,
templates for all ensemble members, number of ensemble members, and the
ensemble_stat configuration file.

HRRRe_ens.conf:  Configuration file containing parameters specific to
running HRRRe through ensemble_stat to create ensemble means for
temperature, dew point, and wind speed.  This includes the model input
directory, templates for all ensemble members, number of ensemble
members, and the ensemble_stat configuration file.

hrrre_regrid.conf:  Configuration file containing parameters specific to
regridding other models to the grid used for the HRRRe (CLUE grid). 
This includes the location of different MET configuration files for
grid_stat and point_stat, different output directories for grid_stat and
point_stat, and the location of the verification masks for the CLUE
grid.

HRRRv3.conf:  Configuration file containing parameters specific to the
HRRRv3 model, including the initializations times processed, min and max
forecast lead times used, model input directory and file template and
mask used.

HRRRv4.conf:  Configuration file containing parameters specific to the
HRRRv4 model, including the initializations times processed, min and max
forecast lead times used, model input directory and file template and
mask used.

NSSLfv3.conf:  Configuration file containing parameters specific to the
NSSL-FV3 model, including the initializations times processed, min and
max forecast lead times used, model input directory and file template
and mask used.

nsslfv3_regrid.conf:  Configuration file containing parameters specific
to regridding other models to the grid used for the NSSL-FV3 (CLUE
grid).  This includes the location of different MET configuration files
for grid_stat and point_stat, different output directories for grid_stat
and point_stat, and the location of the verification masks for the CLUE
grid.

pb2nc.conf:  Configuration file containing parameters specific to
running pb2nc to convert observed temperature, dew point, and wind speed
from prepbufr to netCDF.  This includes the observation input and output
directories and file name templates, the pb2nc configuration file, and
the variables to process.

pcp_combine_grid_stat.conf:  Configuration file containing parameter
specific to running grid_stat on precipitation data.  This includes the
neighborhood size and shape (for computing FSS), the precipitation
thresholds to use for the model and observation data, the input and
output directories and templates for pcp_combine for the model, and
pcop_combine and grid_stat for the observations, and the verification
mask template.

point_stat.conf:  Configuration file containing parameters specific to
running statistics using point_stat on the deterministic model
temperature, dew point, and wind speed compared to the point
observations for these same variables.  This includes the location of
the point_stat configuration file, model and observation names, levels,
and thresholds for temperature, dew point, and wind speed, input
directories and templates for the model and observations, the location
of the output directory, and the location of the verification mask.

point_stat_ens.conf:  Configuration file containing parameters specific
to running statistics using point_stat on the ensemble mean temperature,
dew point, and wind speed compared to the point observations for these
same variables.  This includes the location of the point_stat
configuration file, model and observation names, levels, and thresholds
for temperature, dew point, and wind speed, input directories and
templates for the model and observations, the location of the output
directory, and the location of the verification mask.

time_init_ens_apcp24_00.conf:  Configuration file containing parameters
specific to running statistics on ensembles (generated at 0000 UTC) by
initialization time on 24 hourly lead times, including the
initialization time format, initialization begin and end times,
initialization increment, lead times to process, and forecast variable
level.

time_init_ens_apcp24_12.conf:  Configuration file containing parameters
specific to running statistics on ensembles (generated at 1200 UTC) by
initialization time on 24 hourly lead times, including the
initialization time format, initialization begin and end times,
initialization increment, lead times to process, and forecast variable
level.

time_init_ens_apcp3.conf:  Configuration file containing parameters
specific to running statistics on ensembles by initialization time on 3
hourly lead times, including the initialization time format,
initialization begin and end times, initialization increment, and lead
times to process.

time_init_ens.conf:  Configuration file containing parameters specific
to running statistics on ensembles by initialization time on hourly lead
times, including the initialization time format, initialization begin
and end times, initialization increment, and lead times to process.

time_valid_24hrly.conf:  Configuration file containing parameters
specific to running statistics by valid time at a 24 hourly frequency,
including the valid time format, valid begin and end times, and time
increment.  This file also contains information about the variable name
and level for 24 hourly accumulated precipitation.

time_valid_3hrly.conf: Configuration file containing parameters specific
to running statistics by valid time at a 3 hourly frequency, including
the valid time format, valid begin and end times, and time increment. 
This file also contains information about the variable name and level
for 3 hourly accumulated precipitation.

time_valid_hrly.conf:  Configuration file containing parameters specific
to running statistics by valid time at an hourly frequency, including
the valid time format, valid begin and end times, and time increment

Additional configuration notes:

The ensemble means for the HRRRe and HREFv2 were created in a separate
process from the verification through grid_stat.  The reasoning for this
was to divide up the load on the machine. Ensemble means could be
created prior to the arrival of observations, allowing these to be run
outside the time when the majority of the verification was processing.

**Scripts Outside of METplus**
______________________________

These scripts are currently located in
/home/christina.kalb/python_separates

create_met_poly.py:  Takes a json file containing the sector bounds for
the daily domain and converts it to a MET poly file which can then be
run through gen_vx_mask to create a masking file for the daily domain.

run_met_surrogate_severe_perc.py:  Runs the surrogate severe files
created by Burkely through grid_stat twice.  The first run creates CSI
and bias, and the second computes probability statistics such as ROC and
Reliability.  This program also calls create_met_poly.py to create a
masking region for the surrogate severe data.

run_pcp_href.py:  Converts each member of the HREFv2 ensemble from
hourly precipitation to 3 hourly and 24 hourly.

Run_pcp_hrrre.py:  Converts each member of the HRRRe ensemble from
hourly precipitation to 3 hourly and 24 hourly.

run_pcp_obs.py:  Takes hourly Stage IV precipitation data and
accumulates it to compute 3 hourly and 24 hourly data.

Run_stat_analysis_refc_hrrrv3_hrrrv4_nsslfv3.py:  Takes the output of
MET from grid_stat for the HRRRv3 and HRRRv4, and accumulates using
stat_analysis, so the data is in a format for Brett to display on the
webpage

run_stat_analysis_surrogate_severe.py:  Takes the output of MET from
grid_stat for the surrogate severe data, and accumulates using
stat_analysis, so the data is in a format for Brett to display on the
webpage

**Statistics cron jobs in 2019**
================================

These are shortened examples; the full paths are omitted for clarity. 
The full versions and time each job was run can be found in the files
crontab.tina and crontab.tina.bigbang.

**Buxton2**
___________
| *Point Observations, temperature, dew point, and wind speed converted to netCDF:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c pb2nc.conf -c time_valid_hrly.conf
| 
| *Surrogate Severe grid_stat:*
| /usr/local/Python2.7.11/bin/python run_met_surrogate_severe_perc.py
| 
| *Surrogate Severe stat_analysis:*
| /usr/local/Python2.7.11/bin/python run_stat_analysis_surrogate_severe.py
| 
| **HREFv2 data**
| *Combining ensemble member precipitation to 3 hourly and 24 hourly:*
| /usr/local/Python2.7.11/bin/python run_pcp_href.py
| 
| *Ensemble mean temperature, dew point, and wind speed with ensemble_stat:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c time_init_ens.conf -c HREFv2_ens.conf -c ens_stat.conf
| 
| *24 hour Precipitation (0000 initialization time) ensemble mean with ensemble_stat:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c /HREFv2_apcp24.conf -c ens_stat.conf -c time_init_ens_apcp24_00.conf
| 
*24 hour Precipitation (1200 initialization time) ensemble mean with ensemble_stat:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c HREFv2_apcp24.conf -c ens_stat.conf -c time_init_ens_apcp24_12.conf
| 
| *3 hour Precipitation ensemble mean with ensemble_stat:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c HREFv2_apcp3.conf -c ens_stat.conf -c time_init_ens_apcp3.conf
| 
| *Ensemble mean environment point_stat:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c HREFv2.conf -c point_stat_ens.conf -c time_valid_hrly.conf
| 
| *Ensemble mean environment point_stat, regridded to the CLUE Domain:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c HREFv2.conf -c point_stat_ens.conf -c time_valid_hrly.conf -c hrrre_regrid.conf
| 
| *24 Hour Precipitation ensemble mean grid_stat regridded to the CLUE Domain:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c HREFv2.conf -c grid_stat_pcp.conf -c time_valid_24hrly.conf -c hrrre_regrid.conf -c ens_mean_apcp24.conf
| 
| *3 Hour Precipitation ensemble mean grid_stat regridded to the CLUE Domain:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c HREFv2.conf -c grid_stat_pcp.conf -c time_valid_3hrly.conf -c hrrre_regrid.conf -c ens_mean_apcp3.conf
| 
| **HRRRv3 data**
| *Reflectivity grid_stat:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c HRRRv3.conf -c grid_stat_refc.conf -c time_valid_hrly.conf
| 
| *Environment point_stat:*
| 
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c HRRRv3.conf -c point_stat.conf -c time_valid_hrly.conf
| 
| *Reflectivity grid_stat regridded to the CLUE grid:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c HRRRv3.conf -c grid_stat_refc.conf -c time_valid_hrly.conf -c nsslfv3_regrid.conf
|
| *Environment point_stat regridded to the CLUE grid:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c HRRRv3.conf -c point_stat.conf -c time_valid_hrly.conf -c nsslfv3_regrid.conf
| 
| *24 hour precipitation grid_stat:*
| 
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c HRRRv3.conf -c pcp_combine_grid_stat.conf -c time_valid_24hrly.conf
| 
| *3 hour precipitation grid_stat:*
| /usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf -c HRRRv3.conf -c pcp_combine_grid_stat.conf -c time_valid_3hrly.conf
| 
| *24 hour precipitation grid_stat regridded to the CLUE grid:*

/usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf
-c HRRRv3.conf -c grid_stat_pcp.conf -c time_valid_24hrly.conf -c
nsslfv3_regrid.conf

*3 hour precipitation grid_stat regridded to the CLUE grid:*

/usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf
-c HRRRv3.conf -c grid_stat_pcp.conf -c time_valid_3hrly.conf -c
nsslfv3_regrid.conf

*NSSL-FV3 data*

*Reflectivity grid_stat:*

/usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf
-c NSSLfv3.conf -c grid_stat_refc.conf -c time_valid_hrly.conf

*Environment point_stat:*

/usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf
-c NSSLfv3.conf -c point_stat.conf -c time_valid_hrly.conf

*24 hour precipitation grid_stat:*

/usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf
-c NSSLfv3.conf -c pcp_combine_grid_stat.conf -c time_valid_24hrly.conf

*3 hour precipitation grid_stat:*

/usr/local/Python2.7.11/bin/python master_metplus.py -c base_paths.conf
-c NSSLfv3.conf -c pcp_combine_grid_stat.conf -c time_valid_3hrly.conf

**Bigbang2**

*Combining observed ST4 hourly precipitation to 3 and 24 with
pcp_combine:*

/bin/python run_pcp_obs.py

*Reflectivity stat_analysis:*

/bin/python run_stat_analysis_refc_hrrrv3_hrrrv4_nsslfv3.py

*HRRRe data*

*Ensemble mean temperature, dew point, and wind speed with
ensemble_stat:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRe_ens.conf -c ens_stat.conf -c time_init_ens.conf 

*Combining ensemble member precipitation to 3 hourly and 24 hourly:*

/bin/python run_pcp_hrrre.py

*24 hour Precipitation (0000 initialization time) ensemble mean with
ensemble_stat:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRe_apcp24.conf -c ens_stat.conf -c time_init_ens_apcp24_00.conf

*24 hour Precipitation (1200 initialization time) ensemble mean with
ensemble_stat:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRe_apcp24.conf -c ens_stat.conf -c time_init_ens_apcp24_12.conf

*3 hour Precipitation ensemble mean with ensemble_stat:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRe_apcp3.conf -c ens_stat.conf -c time_init_ens_apcp3.conf

*Ensemble mean environment point_stat:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRe.conf -c point_stat_ens.conf -c time_valid_hrly.conf

24 *Hour Precipitation ensemble mean grid_stat:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRe.conf -c grid_stat_pcp.conf -c time_valid_24hrly.conf -c
ens_mean_apcp24.conf

*3 Hour Precipitation ensemble mean grid_stat:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRe.conf -c grid_stat_pcp.conf -c time_valid_3hrly.conf -c
ens_mean_apcp3.conf

*HRRRv4 data*

*Reflectivity grid_stat:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
${CPATH}/HRRRv4.conf grid_stat_refc.conf -c time_valid_hrly.conf

*Environment point_stat:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRv4.conf -c point_stat.conf -c time_valid_hrly.conf

*Reflectivity grid_stat regridded to the CLUE grid:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRv4.conf -c grid_stat_refc.conf -c time_valid_hrly.conf -c
nsslfv3_regrid.conf

*Environment point_stat regridded to the CLUE grid:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRv4.conf -c point_stat.conf -c time_valid_hrly.conf -c
nsslfv3_regrid.conf

*24 hour precipitation grid_stat:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRv4.conf -c pcp_combine_grid_stat.conf -c time_valid_24hrly.conf

*3 hour precipitation grid_stat:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRv4.conf -c pcp_combine_grid_stat.conf -c time_valid_3hrly.conf

*24 hour precipitation grid_stat regridded to the CLUE grid:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRv4.conf -c grid_stat_pcp.conf -c time_valid_24hrly.conf -c
nsslfv3_regrid.conf

*3 hour precipitation grid_stat regridded to the CLUE grid:*

/bin/python master_metplus.py -c bigbang.conf -c base_paths.conf -c
HRRRv4.conf -c grid_stat_pcp.conf -c time_valid_3hrly.conf -c
nsslfv3_regrid.conf

**Creating Scorecards**
=======================

Scorecards are created by running METviewer in a container.  The output
from MET tools is first added to a METviewer database, and then
scorecards are run on this database.  Both of these processes are
launched from a container. The files associated with creating scorecards
are located in /raid/efp/se2019/ftp/dtc/metviewer.

Hank...  How to turn on the container

To load data into the METviewer database, run one of the load shell
scripts which are described below.  The shell scripts reference
parameter files that list the data to be loaded into the database.
Scorecards are run by calling mv_scorecard.sh followed by an xml file
that contains the models and variables to display on the scorecard.  The
color and symbol settings, as well as significance thresholds are
located in the xml file, thresh_sigdiff.xml.

*Database Loading Files:*

add_mv_hwt_2019.sh:  Add the HRRRv3 and HRRRv4 data to the database

load_mv_hwt_2019.sh: reload all data from all models

*Model Scorecard xml files:*

scorecard_cam_2019_hrrrv3_hrrrv4.xml:  HRRRv3 versus HRRRv4

scorecard_cam_2019_nsslfv3_hrrrv3_cluegrid.xml:  NSSL-FV3 versus HRRRv3

scorecard_cam_2019_nsslfv3_hrrrv4_cluegrid.xml:  NSSL-FV3 versus HRRRv4

scorecard_cam_2019_href_hrrre_mean.xml:  HREFv2 versus HRRRe ensemble
mean

*Surrogate Severe Scorecard xml files:*

scorecard_cam_2019_ss_hrrrv3_hrrrv4.xml:  HRRRv3 versus HRRRv4

scorecard_cam_2019_ss_nsslfv3_hrrrv3_cluegrid.xml:  NSSL-FV3 versus
HRRRv3

scorecard_cam_2019_ss_nsslfv3_hrrrv4_cluegrid.xml:  NSSL-FV3 versus
HRRRv4

scorecard_cam_2019_ss_href_hrrre_cluegrid.xml:  HREFv2 versus HRRRe

*Other xml files*

thresh_sigdiff.xml:  Contains the color and symbol settings, as well as
significance thresholds

**Scorecard cron jobs in 2019**
===============================

The full versions and time each job was run can be found in the files
/home/hank.fisher/cron/crontab.

*Run Tues - Saturday at 11am*

*Add HRRRv3 and HRRRv4* *to the database*

/bin/docker exec -e JAVA=/usr/bin/java -d metviewer_1 sh -c
"/raid/efp/se2019/ftp/dtc/metviewer/scripts/add_mv_hwt_2019.sh"

*Run HRRRv3/HRRRv4 Surrogate Severe Scorecard*

/bin/docker exec -e JAVA=/usr/bin/java -d metviewer_1 sh -c
"bin/mv_scorecard.sh
/raid/efp/se2019/ftp/dtc/metviewer/xml/scorecard_cam_2019_ss_hrrrv3_hrrrv4.xml"

*Run HRRRv3/HRRRv4 Scorecard*

/bin/docker exec -e JAVA=/usr/bin/java -d metviewer_1 sh -c
"bin/mv_scorecard.sh
/raid/efp/se2019/ftp/dtc/metviewer/xml/scorecard_cam_2019_hrrrv3_hrrrv4.xml"

*Run Friday at 11am*

*Reload all the data*

/bin/docker exec -e JAVA=/usr/bin/java -d metviewer_1 sh -c
"/raid/efp/se2019/ftp/dtc/metviewer/scripts/load_mv_hwt_2019.sh"

*Run NSSL-FV3 versus HRRRv3 Scorecard*

/bin/docker exec -e JAVA=/usr/bin/java -d metviewer_1 sh -c
"bin/mv_scorecard.sh
/raid/efp/se2019/ftp/dtc/metviewer/xml/scorecard_cam_2019_nsslfv3_hrrrv3_cluegrid.xml"

*Run NSSL-FV3 versus HRRRv3 Scorecard*

/bin/docker exec -e JAVA=/usr/bin/java -d metviewer_1 sh -c
"bin/mv_scorecard.sh
/raid/efp/se2019/ftp/dtc/metviewer/xml/scorecard_cam_2019_nsslfv3_hrrrv4_cluegrid.xml"

*Run NSSL-FV3 versus HRRRv3 Surrogate Severe Scorecard*

/bin/docker exec -e JAVA=/usr/bin/java -d metviewer_1 sh -c
"bin/mv_scorecard.sh
/raid/efp/se2019/ftp/dtc/metviewer/xml/scorecard_cam_2019_ss_nsslfv3_hrrrv3_cluegrid.xml"

*Run NSSL-FV3 versus HRRRv3 Surrogate Severe Scorecard*

/bin/docker exec -e JAVA=/usr/bin/java -d metviewer_1 sh -c
"bin/mv_scorecard.sh
/raid/efp/se2019/ftp/dtc/metviewer/xml/scorecard_cam_2019_ss_nsslfv3_hrrrv4_cluegrid.xml"

*Run HREFv2 versus HRRRe Ensemble Mean Scorecard*

/bin/docker exec -e JAVA=/usr/bin/java -d metviewer_1 sh -c
"bin/mv_scorecard.sh
/raid/efp/se2019/ftp/dtc/metviewer/xml/scorecard_cam_2019_href_hrrrre_mean.xml"

*Run HREFv2 versus HRRRe Surrogate Severe Scorecard*

/bin/docker exec -e JAVA=/usr/bin/java -d metviewer_1 sh -c
"bin/mv_scorecard.sh
/raid/efp/se2019/ftp/dtc/metviewer/xml/scorecard_cam_2019_ss_href_hrrrre_cluegrid.xml"
