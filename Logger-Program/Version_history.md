CR6 Logger Program for IRGASON and REA System. The latest version is the version with the highest version number. Files are named "ICOSCitiesV`XXX`.CR6", where `XXX` is the three digit version number.

V026
=========

- Online calculation of the dry mole fraction by Josh Hashemi
- In use for REA sampling in Zurich, Hardau: 14 July 2022, 16:07 - 20 July 2022, 06:30 

V027
=========

- Introduces a new public variable for `SYS_REA_Sampler_Voltage_Threshold`. If voltage monitored at Channel U7/U8 was in the previous scan loop (50 ms) above the value set in `SYS_REA_Sampler_Voltage_Threshold` in mV and drops in the current loop below the value of `SYS_REA_Sampler_Voltage_Threshold` then `REA_Active` is set to `0`. This should happen if one of the reservoirs of the sampler is full. The run is ended, all valves are closed and run statistics are written. During start-up, when the voltage monitored at Channel U7/U8 is below `SYS_REA_Sampler_Voltage_Threshold` and also was below in the previous loop (50 ms) - nothing happens, this is the time the sampler is not yet ready yet, as denoted in the flags. New default value is 1500.
- In use for REA sampling in Zurich, Hardau: 20 July 2022, 07:00 - 21 July 2022, 13:00 

V028
=========

- Added dry mole fraction to `RAW_20Hz` output table.
- In use for REA sampling in Zurich, Hardau: 21 July 2022, 14:00 - 11 August 2022, 10:00 

V029
=========

- Changed functionality of REA sampler triggering. Instead of user setting `Rea_Active` = 1 the program watches voltage at Channel U7/U8 and if Voltage goes from around zero to over 1.5 V the run is started.
- Added wind direction and horizontal wind speed to `REA_RunSummary` table
- Added "All" averages of measurements to `REA_RunSummary` table (i.e., average of measurement over entire REA run - "up" + "down" + deadband)
- In use for REA sampling in Zurich, Hardau: 11 August 2022, 10:20 - 20 October 2022, 14:30 

V030
=========

- NTP function is deactivated
- Time is remotely set every 24 h from the Basel computer server

V031
=========

- Flushing of lines: Added extra X seconds immediately at end of every REA run during which both valves are opened and kept open. Once X seconds after the end of the run has reached, both vales are closed. The number of seconds X is set by the parameter `SYS_REA_OpenValvesAtEnd` with default value `12` (seconds) as proposed by Ann-Kristin. During the X seconds, the variable `REA_Run_EndCounter` will increment. `REA_Run_EndCounter` will be zero during any other condition (so after the flushing ended, during a REA run).
- Mainenance of Valves: Added a regular maintenance valve opening and closures twice a day, for one second open, fixed at 01:00 and 13:00 UTC. The maintenance valve opening and closures will not be communicated to the sampler and they will not trigger a run. They just happen and are written with code `9` into the REA flag. Maintenance valve opening and closures will not happen if an active run is ongoing (no need).
- Added an option to have a runniung mean for w and sigma w during any REA run. Up to version V30, within a REA run, the mean vertical wind (Uz) and the standard deviation of vertical wind (Sigma Uz) were frozen (=kept constant) at the beginning of the run, based on the statistics during the 30 minutes preceeding the run. Now, you can choose to keep this behaviour by setting `REA_FreezeWindStatistics = 1`, or, you can also opt to set `REA_FreezeWindStatistics=0` and the mean vertical wind and the standard deviation of vertical wind will continue to be adjusted during the run based on their moving average (The length of the moving average is set by `SYS_MovingBlockDuration = 1800`. By default it is 30 minutes).
- Run summary files are uploaded by FTP to the server after each REA run. The file name is created based on the end date / time of the run. Each file will have one run included.
- In use for REA sampling in Zurich, Hardau: 20 August 2022, 14:33 - 30 March 2023  
          

V032.INRAE
=========

- Adding a delay of 2 min prior to gziping and sending files since writing data in memory takes ~1:45 min
- gziping the 30 min files
- sending the zipped files to the server

V033.INRAE.MPI
========= 
- Adaptations to Romainville site and zipping of files
- In use for REA sampling in Paris, Romainville: 05 July 2023 - 10 July 2023, 09:00

V034.INRAE.MPI
=========

 - Add a force reset of REA_PreviousReservoir to 0 during the X seconds flush after runs. Under the previous logic if one cycled Manual_OpenValves on/off twice without an actual REA sample inbetween the codeblock controlling the valves would never execute, causing some of the confusing ROV behaviours.
 - In use for REA sampling in Paris, Romainville: 10 July 2023, 10:00 - 07 September 2023, 11:00 

V035.INRAE.MPI
=========
 - In use for REA sampling in Paris, Romainville: 07 September 2023, 11:30 - 20 February 2023, 10:00

V036.INRAE.MPI
=========
 - In use for REA sampling in Paris, Romainville: 20 February 2023, 10:00 - 26 March 2024, 10:30

V038
=========

- Added the option instead of a deadband based on vertical wind to sample based on a hyperbolic relaxed eddy accumulation (HREA). The following new public variables have been introduced: 
    - `REA_Hyperbolic` If the public variable `REA_Hyperbolic` is set to `1`, then HREA is chosen as the sampling strategy, otherwise the classical deadband based on vertical wind is implemented. The hyperbolic holesize is defined by the parameter `REA_DeadBandWidth`: (so `REA_DeadBandWidth` sets both, the deadband for the classical separation based on vertical wind and the holesize of the hyperbolic hole.
    - `REA_HyperbolicDistance`: This is the instantaneous product of ABS((c'-C)/Sigma_c) * ABS((w'-W)/Sigma_w). Is used in calculations but not stored. If `REA_HyperbolicDistance` < `REA_DeadBandWidth` then we are inside the hole region and no sampling takes place.
    - `RAW_IRGASON_CO2Dry_SqDeparture`: (instantaneous squared departure of CO2 fluctuation from (running) mean CO2)
    -  `AGG_IRGASON_CO2Dry_RunAvg`: Either frozen or constantly adjusted running mean of CO2 dry mole faction.
    -   `AGG_IRGASON_CO2Dry_RunSqDeparture`,: Either frozen or constantly adjusted squared deviation of CO2 dry mole faction (variance).
    -   `AGG_IRGASON_CO2Dry_Stddev`,: Either frozen or constantly adjusted standard deviation of CO2 dry mole faction.

 The variable `REA_FreezeWindStatistics` has been renamed to `REA_FreezeStatistics` because it now also includes CO2.

The run summary outputs have been changed as follows: Added a variable `REA_Hyperbolic` that is either 0 (no HREA, classical mode) or 1 (HREA mode). Added variables `AGG_IRGASON_CO2Dry_RunAvg` and `AGG_IRGASON_CO2Dry_Stddev`, but note they make only sense if `REA_FreezeStatistics` is set to 1.


V039
========= 

 - The `EC100()` command was modified to use trigger method 2, which uses humidity-correcetd 20 Hz sonic temperature to calculate CO2 density.
 - An additional variable was added to data tables due to the above, namely `RAW_IRGASON_CO2Dry_FastResp`
 - The calculation of variable `RAW_IRGASON_CO2Dry` was modified to use this new fast response density, which is needed for accurate control of the hyperbolic deadband.

V039_Munich
========= 
- REA settings adapted to Munich setup
- In use for REA sampling in Munich, Blutenburgstraße:  since 1 July 2024
