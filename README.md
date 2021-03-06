# Lagrangian Analysis of Thunderstorms in Switzerland
## Master Thesis

Author:

Thomas M. Lanz | MSc in Climate Sciences | OCCR - University of Bern | thlgwatt@hotmail.com

## Abstract
Thunderstorms occur frequently in Switzerland and they can cause severe damage. Although their frequent occurrence, the initiation of thunderstorms is still incompletely understood. Here, this master thesis aims to contribute to a better understanding of conditions and processes leading to thunderstorm initiation in Switzerland. Therefore, trajectories were calculated with Lagranto in high resolution WRF model data simulated for May 2018. The research of thunderstorms by calculating trajectories in high resolution model data, is a novel approach and enabled new insights into the formation of the atmospheric layers. Combined with other analysis tools, five case studies were conducted and the basic ingredients for thunderstorm initiation were therein investigated. The assessment of the responsible triggering mecha- nisms revealed a high relevance of the presence of low-level convergence (all case studies), a direct impact of storm outflow boundaries (two case studies) and a strong influence of the orography (three case studies) on the initiation of the respective thunderstorm. The second part of this thesis addressed the impact of the changing climate on the initiation of thunderstorms, which is still an open question. Therefore, the pre-strom environments of all thunderstorms in May 2018 and in end of the century climate conditions were analysed. The comparison between the two climate conditions indicates a higher potential for strong and short-lived convection in end of the century climate conditions.

## Analysis Tools for Investigating Thunderstorm Initiation Conditions

For reaching the aim of the master thesis (access to PDF via <https://occrdata.unibe.ch/students/theses/msc/300.pdf> or e-mail above), the analysis tools consists of different tools for investigating the conditions and processes of the atmosphere: terrain height maps, horizontal maps, soundings, vertical cross-section, temporal evolution of variable at location, maps of trajectories and temporal evolution along trajectories. In the second part of the master thesis, the temporal evolution of the pre-storm environment of all thunderstorms in May 2018 and in end of the century climate conditions was of interest and analysed by multiprocessing the TITAN and WRF data. The figures created with these tools served as basis for the analysis of thunderstorm initiation and the answering of the research question of the master thesis.

The programming was done in IDE Jupyter Notebook (v6.0.3) with the IPython Kernel (v7.13.0) and with package and environment management by Conda. For each of the analysis tools, a notebook document was created. In the following section the workflow of the analysis tools is described.

## What the Analysis Tools Do and How to Use
For the analysis tools data from the Weather Research and Forecasting (WRF) model (v4 ARW) and from the Lagranto program is used, which takes input data from the WRF model. The WRF data is in netCDF file format and the Lagranto output data in ASCII file format. 

The following subsections provide information regarding the useage of the codes.

---

## Terrain Height Maps
A map of the terrain height is produced by this analysis tool with optional add-ons like a smaller map extent (subsets), a rectangle of the horizontal map's extent (see next section), the initiation location, the cross-section path, the starting locations of the trajectories, the case study locations, the initiation locations of all thunderstorms and a divergence map with height contours.

### Example of Usage:
terrain_height_map('case_study_2', subset=True, initiation=True, divergence=True, save=True)

---

## Horizontal Maps
This analysis tool produces horizontal (2D) maps of different variables. Supported variables for the plotting function (horizontal_map) are updraft, reflectivity, helicity, pw, cape, cin, ctt, temperature_surface, wind_shear, updraft_reflectivity, rh, omega, pvo, avo, theta_e, water_vapor, uv_wind and divergence. Besides the selection of the desired variable_name for the function, more input parameters need to be defined like date, start_hour, start_minute, end_hour, break_minute, case_study_name, pressure_level, subset, initiation, save and gif. The first parameters need a specific input value (e.g. variable_name="divergence", date="2018-05-30", start_hour=16, end_hour=17, pressure_level=850), where the remaining parameters need Boolean values (True or False). Before using the plotting function, the definition of some other variables in first section of the code is necessary (data_dir, save_dir, subset_extent and initiation_location). The horizontal_map function iterates with a 5 minutes time step over a list of files from start_hour until end_hour or break_minute and creates figures of each time step. If the gif parameter is set True, a gif is created from all the generated figures of the interation process.

### Example of Usage:
horizontal_map(variable_name="wind_shear", start_hour=14, start_minute=50, end_hour=15, break_minute=50, case_study_name='case_study_5', title=False, subset=True, initiation=True, save=True)

---

## Sounding
The sounding is an analysis tool for investigating the vertical distribution of atmospheric physical properties (e.g. temperature, pressure, wind, etc.) and represents the WRF model data in a similar way (thermodynamic diagram) like measurement data from an real world atmospheric sounding (e.g. balloon sounding). For a selected location (lat, lon) and time (date), the analysis tool generates a skew T-log p diagram, based on the variables derived from WRF data file (filename). Because the WRF model data lacks some of these specific variables (e.g. pressure, dewpoint temperature or wind speed), these required variables need to be computed in advance (by wrf-python function getvar()) and added back to the WRF dataset. Afterwards, the variables are selected for a specific location and some further variables need to be calculated with MetPy functions. Finally, the code generates a figure according to the specific layout of a skew T-log p diagram.

### Example of Usage:
sounding(save_name='case_study_5', title=False, save=True)

---

## Vertical Cross-Section
Vertical cross-sections show a vertical slice of the atmosphere along a line with a specified start (start_lat, start_lon) and end point (end_lat, end_lon). The analysis tool is represented by a plotting function (cross_section), which supports the following variables: vertical_velocity, rh, omega, absolute_vorticity, theta_e and reflectivity. The only input parameters left to define are date, time and save, if a saving of the figure is desired (default save=False). Before the cross_section function can be used, the data and save directory need to be adjusted according to the respective setting of the user. After interpolation and removing of white space between terrain height and contour of the variable, the code finally creates a vertical cross-section figure with filled mountain area.

### Example of Usage:
cross_section(variable_name='theta_e', case_study_name='case_study_1', save=True)

---

## Temporal Evolution of Variable at Location
With this analysis tool the temporal evolution of a variable is calculated in the atmosphere above the initiation location and its surroundings (same locations as trajectory starting locations) over a certain amount of 5 minutes time steps (periods) before the thunderstorm initiation. Supported variuables for plotting procedure are wind_speed, rh, omega, absolue_vorticity and theta_e.

### Example of Usage:
temporal_evolution_variable(variable_name='rh', periods=102, case_study_name='case_study_1', save=True)

---

## Maps of Trajectories
The analysis tool for mapping trajectories uses the output data (trajectories) from the Lagranto program. With the help of the lagranto_plotting function, the desired variables (water_vapor, updraft, height or theta_e) along the trajectories can be plotted on the background of terrain height contours (greys). The start_time and end_time of the calculated trajectories need to be indicated as input parameters, as well as Booloean value True, if a subset or a saving of the figure is requested. Furthermore, a bunch of trajectories can be selected according to their height level (pbl, 5 or 10) or otherwise, all available trajectrories will be plotted. Prior to executing the function, some more variables need to be set for defining the Planteary Boundary Layer (PBL) height. The number of plotted trajectories in the figure is adjustable by specifying number_trajs_plot. In addition, the trajectory data and save directory need to be specified, as well as the location of thunderstorm initiation and the extent of subset. Further, the end_cut parameter can be used to adjust the plotted time of the calculated trajectories by cutting by the indicated number of 5 minutes time steps. After the definiton of all necessary input parameters, the function can be executed and a horizontal map of trajectories is generated.

### Example of Usage:
lagranto_plotting(traj_variable_name='height', case_study_name='case_study_1', start_time='1630', end_time='0800', end_cut=-6, end_time_adj='0830', subset=True, save=True)

---

## Temporal Evolution along Trajectories
This analysis tool creates a figure of the temporal evolution (time since initiation on the x-axis) of the chosen variable along the trajectories (variable values on the y-axis). As already mentioned in the subsection before, also the function temporal_evolution_trajectories is capable of seperating trajectories in bunches of different heights ('pbl', '5' or '10', default = 'all'). Besides the directory of the trajectory data and the desired save directory, delta time (dt in minutes) between the data files needs to be specified as well. If only a portion of trajectories should be included in the figure, then the number of plotted trajectories (number_trajs_plot) can be varied according to the requests of the user. Further, some variables for getting the PBL height need to be set. For the analysis and comparison between different vertical trajectory bunches, the 10th and 90th percentile and mean for each bunch of trajectories is computed and inidicated with colored lines on the figure (see legend for labeling). The time_cut parameter behaves similarly as the end_cut parameter from the previous section (see Maps of Trajectories) and adjusts the shown calculated time of the trajectories on the x-axis. With all needed variables and input parameters defined, the function is ready for creating a figure of the temporal evolution along trajectories. 

### Example of Usage:
temporal_evolution_trajectories(variable_name='theta_e', case_study_name='case_study_5', start_time='1420', end_time='0600', time_cut=4, trajs_bunch_level='all', title=False, legend=False, save=True)
                            
---

## Temporal Evolution of Pre-Storm Environment of All Thunderstorms
In a first step, a list of all WRF files is generated according to selected data_part and the TITAN data is preprocessed for the multiprocessing function afterwards. Therein, the initiations are selected according to some criteria (e.g. minimum duration) and for these remaining initiations backward time steps are calculated. In a second step, a multiprocessing function iterates through all WRF files and extracts the WRF variables at the initiation locations defined by the preprocessed TITAN data, if the time of initiation or one of the time steps before match with the time of the WRF file (next iteration starts, if no initiations happend at the time of the WRF file). The extracted WRF variables are stored in a list, from which the different variables (e.g. CAPE, CIN, WVMR) are selected and their mean and percentiles are calculated in a next step. This builds the basis for the final plotting step, during which the temporal evolution figures are generated.

---
