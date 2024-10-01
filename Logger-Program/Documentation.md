# ICOS-Cities - Eddy Covariance

Internal documentation and planning of the H2020 Project ICOS Cities Task 3.4 (Eddy Covariance)

## Current issues

Check out current issues on data flow, logger program and processing:

https://github.com/envmet/ICOS-Cities/issues/

Check out the project board for current issues and tasks related to implementation:

https://github.com/envmet/ICOS-Cities/projects/1

## Documentation of ICOS-Cities CR6 Logger and Program

The program `ICOSCitiesV###.cr6` is a universal logger program for the Campbell Scientific [CR6 data logger](https://www.campbellsci.com/cr6), where `###` is its three digit version number. We call it "universal" program because the same program can be run in Zurich, Paris and Munich with and without IOP instruments attached. In all cases the program is recording the raw 20 Hz data of the `IRGASON`, and while the intensive observations are conducted also controls the valves and the ICOS relaxed eddy accumulation sampler built by MPI Jena (implemented) and records the REA runs. In addition, an optional small all-in-one weather sensor can be recorded to provide uninterrupted data on wind, temperature / humidity, radiation, pressure etc.

### Wiring of CR6 Logger

The permanent equipment associated and connected with the `CR6` program include a `IRGASON` and optionally a `ClimaVUE50` all-in-one weather sensor. For the intensive observation periods, also a `SDM-CD8S` DC Controller is prepared at each tower to control 2x solenoid valves and a 24 port ICOS Flask Sampler is prepared to communicate with the logger. The `SDM-CD8S`  will be placed inside the electronics box of the IRGASON `EC100`. Most likely the valves will be placed for the entire 2-year period at the sites in Zürich, Paris and Munich.

<p align="center"><img width="898" alt="Zurich" src="https://user-images.githubusercontent.com/9665366/180293968-c23859b1-6dd4-4f3e-929f-86f002936af0.png"></p>

Figure 1: Head of the IRGASON with 2 inlets for REA, 1 inlet for the MIRO `MGA7` analyzer and one for a COS analyzer, the last one is only planned in Zurich (Photo by Roland Vogt, University of Basel)

<p align="center"><img width="898" alt="Diagram" src="https://user-images.githubusercontent.com/104753279/180418723-56053ff2-3319-457e-a8ad-7d9d4465f374.png"></p>

Figure 2: General wiring diagram between `IRGASON` head, `EC100` and `CR6` data logger. The power distribution can differ from site to site.

#### IRGASON

The `IRGASON` is an infrared gas analyzer and 3D sonic anemometer combined into a single sensor for eddy-covariance measurements. It was decided to use an IRGASON in all three cities, given its low maintenance needs despite not being an ICOS standard instrument.  The `IRGASON` is connected to an `EC100` electronics module that interfaces with the `CR6` via "SDM" data output. 12V power is provided by either the 12V channel from the `CR6` or external 12V Power supply

`EC100` Channel | `CR6` Channel | Cable Color (standard cable `CABLE4CBL-L`) | Use
---- | ---- | ---- | ----
SDM-C1 | C1 | Green | SDM Data 
SDM-C2 | C2 | White | SDM Clock 
SDM_C3 | C3 | Red (or Brown) | SDM Enable
G | G | Black | Digital Ground 
G | G | Clear | Shield 
PWR | -- | -- | 12V power provided by an external supply

#### SDM-CD8S Controller for REA valves

The `SDM-CD8S` is an 8 Channel DC Controller that adds capability to the `CR6` to control DC devices that have a moderate current load, such as solenoid valves. The `SDM-CD8S` also interfaces with the `CR6` via "SDM" data output. As this uses the same communications port as the `EC100` electronics module, it is necessary to set the SDM address on the outside of the `SDM-CD8S` DC Controller to one different to that of the `EC100` electronics module -- Set this to "2", as the `EC100` will be set to "1". 

`SDM-CD8S` Channel | `CR6` Channel | Cable Color (standard cable `CABLE4CBL-L`) | Use
---- | ---- | ---- | ----
SDM-C1 | C1 | Green | SDM Data 
SDM-C2 | C2 | White | SDM Clock 
SDM_C3 | C3 | Red (or Brown) | SDM Enable
G | G | Black | Common Ground 
G | G | Clear | Shield
PWR | ---- | ---- | 24V power provided by an external supply

Two solenoid valves are connected to the `SDM-CD8S` in DC Channels 1 & 2 that are mounted near the `IRGASON` sonic transducers.

#### ICOS Flask Sampler

The 24 Port ICOS Flask Sampler is used for the automated collection of air samples under specified conditions for Relaxed Eddy Accumulation (REA).

ICOS Flask Sampler cable pin  | `CR6` Channel | Use
---- | ---- | ----
Pin 1 | U1 | Flask Sampler Active Signal
Pin 2 | U2 | Valve 1 "Up" Signal
Pin 3 | U3 | Valve 2 "Down" Signal
Pin 4 | U4 | ----
Pin 5 | U5 | ----
Pin 6 | U6 | ----
Pin 7 | U7 | Voltage Differential (+)
Pin 8 | U8 | Voltage Differential (G)
---- | U8-G | ----

#### Clima Vue50 additional weather sensor

The `ClimaVUE50` is an all-in-one meteorological sensor that serves as a compact mid-cost weather station. For each city, the University of Freiburg is providing a `ClimaVUE50` for the two year period (so does not need to supplied by Uni Basel, INRAE or KIT unless preferred).

The data between the `ClimaVue50` and the `CR6` is exchanged using the "SDI-12" protocol, which is a one-wire protocol. SDI-12 is integrated on the `CR6` which meaning only four connection wires are needed.

`CR6` Channel | Cable Color (standard cable of `ClimaVUE50`) | Use
---- | ---- | ----
U11 | white | `ClimaVue50` Signal 
12V | brown | `ClimaVue50` 12V 
G | black | `ClimaVue50` Signal Ground 
G | clear | `ClimaVue50` Shield 

### General settings

The following settings are defined as constants and must be properly adjusted prior to compiling the program:

Constant | Description | Default value proposed
---- | ---- | ----
`SYS_NTP_Server_IP` | IP Address or full domain name of NTP server to set logger time at start of program and every 24 hours during operation | "192.168.1.1" (Note: This was referring to the local router running its own NTP, but had failed to update the clock correctly. Use an alternative NTP Server you trust! This feature is deactivated in Zurich and the clock is synchronised remotely from Basel using our loggernet connection)
`SYS_FTP_ServerIP` | IP Address or full domain name of FTP server to submit data files at end of every 30 min block | "132.230.103.24"  (Note FTP at Univeristy of Freiburg for data transmission, contact @z--m-n prior to submitting data there)
`SYS_FTP_User` | Username for FTP server | "datagate7" |  
`SYS_FTP_Password` | Password for FTP server | 
`SYS_Offset_from_UTC` | Time offset from UTC. As we use UTC across ICOS Cities, keep at `0` | 0
`SYS_EC100_SDMAddress` | SDM Address of IRGASON EC100 | 1
`SYS_CD8S_SDMAddress` | SDM Address of CD8S Relay Controller | 2
`SYS_SDM_BitPeriod` | Speed of SDM Communication, possibly to be changed if cable length is longer | 28.8
`SYS_CV50_SDI12Port` | Port on which a ClimaVue50 is optionally connected over SDI-12 | U11
`SYS_OutputRate` | Sampling and output of fast data tables in Hz | 20 
`SYS_MovingBlockDuration` | Number of seconds to average running mean and standard deviation of vertical wind for a REA run to determine deadband width and average vertical wind. Depending on the setting of `REA_FreezeWindStatistics` the block average will be frozen during a Run or continues to be dynamically adjusted | 1800 (=30 min)
`SYS_EC100_Bandwidth` | bandwith of IRGASON, should be 1/2 output rate | 10
`SYS_REA_Sampler_Voltage_Threshold` | If voltage drops below this value for one time step then run will stop (too indicate buffers are full) | 1500
`SYS_IRGASON_Azimuth` | Azimuth of IRGASON's y-axis relative to North (i.e. if head points to N = 0, E = 90, S = 180, W = 270) | To be entered for each installation
`SYS_CV50_Azimuth` | Azimuth of ClimaVue's N arrow relative to North (i.e. if N label on ClimaVue points to N = 0, E = 90, S = 180, W = 270) | 90.0
`SYS_ClimaVue50_Enable` | Set to TRUE if a ClimaVue50 is attached (conditional compile), set to FALSE if no ClimaVue50 is attached | TRUE


### CR6 Logger Output Tables

The `CR6` outputs three tables: the RAW Table contains 20Hz data from the `IRGASON` and the status of the REA solenoid valves (either open or closed); the AGG Table contains aggregated 1 minute data including averages and counts from the `IRGASON`, and the `ClimaVUE50`. The `REA_RunSummary` table contains information relevant to all REA run. In case no REA sampler is attached the `REA_RunSummary` is empty. 

Note that the time stamp in the column with the `TIMESTAMP` header always denotes the end of an averaging interval. In accordance with other ICOS-Cities measurements, all time stamps must be always in UTC.

#### RAW_20Hz Table Variables

The `RAW_20Hz` table is written at 20 Hz. Every 30 min, a new files in "TOA5" format is written to the SD card and also transmitted by FTP to the FTP server specified. Transfers occur via Slow Sequence instruction to be executed as time allows - set to 30 minutes but often hourly transfers.

Variable Name | Units | Variable Desciption | Sensor/Device
---- | ---- | ---- | ----
TimeStamp | UTC | Date and Time | `IRGASON`
RAW_IRGASON_Ux | m/s | Ux orthogonal wind component | `IRGASON`
RAW_IRGASON_Uy | m/s | Uy orthogonal wind component | `IRGASON`
RAW_IRGASON_Uz | m/s | Uz orthogonal wind component | `IRGASON`
RAW_IRGASON_Ts | °C | Sonic Temperature | `IRGASON`
RAW_IRGASON_DiagSonic | Arbitrary | Sonic Diagnostic Flag | `IRGASON`
RAW_IRGASON_CO2 | mg/m^3 | CO2 Density | `IRGASON`
RAW_IRGASON_H2O | g/m^3 | H2O Density | `IRGASON`
RAW_IRGASON_DiagIRGA | Arbitrary | Gas Diagnostic Flag | `IRGASON`
RAW_IRGASON_CellT | °C | Air Temperature | `IRGASON`
RAW_IRGASON_CellPrs | kPa | Air Pressure | `IRGASON`
RAW_IRGASON_CO2SignStrngth| Nominally 0.0 to 1.0 | CO2 Signal Strength | `IRGASON`
RAW_IRGASON_H2OSignStrngth| Nominally 0.0 to 1.0 | H2O Signal Strength | `IRGASON`
RAW_IRGASON_CO2Dry | ppm | Dry CO2 Fraction | `IRGASON` Derived
REA_Flag | Arbitrary | Solenoid Valve Flag (see below) | `CR6`

#### AGG_1min Table Variables

The `AGG_1min` table is written at a resolution of 1 min. Every 30 min, a new files in "TOA5" format is written to the SD card and also transmitted by FTP to the FTP server specified. Transfers occur via Slow Sequence instruction to be executed as time allows - set to 30 minutes but often hourly transfers.

Variable Name | Units | Variable Desciption | Sensor/Device
---- | ---- | ---- | ----
TimeStamp | UTC | Date and Time | `IRGASON`
RAW_IRGASON_Ux_Avg | m/s | Ux orthogonal wind component | `IRGASON`
RAW_IRGASON_Uy_Avg| m/s | Uy orthogonal wind component | `IRGASON`
RAW_IRGASON_Uz_Avg | m/s | Uz orthogonal wind component | `IRGASON`
RAW_IRGASON_Ts_Avg | °C | Sonic Temperature | `IRGASON`
RAW_IRGASON_DiagSonic_Avg | Arbitrary | Sonic Diagnostic Flag | `IRGASON`
RAW_IRGASON_CO2_Avg | mg/m^3 | CO2 Density | `IRGASON`
RAW_IRGASON_H2O_Avg  | g/m^3 | H2O Density | `IRGASON`
RAW_IRGASON_DiagIRGA_Avg | Arbitrary | Gas Diagnostic Flag | `IRGASON`
RAW_IRGASON_CellT_Avg | °C | Air Temperature | `IRGASON`
RAW_IRGASON_CellPrs_Avg | kPa | Air Pressure | `IRGASON`
RAW_IRGASON_CO2SignStrngth_Avg | Nominally 0.0 to 1.0 | CO2 Signal Strength | `IRGASON`
RAW_IRGASON_H2OSignStrngth_Avg | Nominally 0.0 to 1.0 | H2O Signal Strength | `IRGASON`
RAW_IRGASON_CO2Dry_Avg | ppm | Dry CO2 Fraction | `IRGASON` Derived
RAW_IRGASON_HorWindVel_ms_Avg | m/s | Horizontal Wind Speed | `IRGASON` Derived
RAW_IRGASON_HorWindDir_DegN_Avg | °N | Horizontal Wind Direction | `IRGASON` Derived
MET_CV50_PressureActual_hPa_Avg | hPa | Air Pressure | `ClimaVUE50`*
MET_CV50_Wind_ms_Avg | m/s | Wind Speed | `ClimaVUE50`*
MET_CV50_WindGustMax_ms_Max | m/s | Maximum Wind Gust Speed | `ClimaVUE50`*
MET_CV50_WindDirection_deg_Vec |°N | Horizontal Wind Direction | `ClimaVUE50`*
MET_CV50_AirTemperature_degC | °C | Air Temperature | `ClimaVUE50`*
MET_CV50_RelHumidity_percent | % | Relative Humidity | `ClimaVUE50`*
MET_CV50_DewPoint_degC | °C | Dew Point Temperature | `ClimaVUE50`*
MET_CV50_VapourPressure_hPa | hPa | Vapor Pressure | `ClimaVUE50`*
MET_CV50_VapPressDeficit_hPa | hPa | Vapor Pressure Deficit | `ClimaVUE50`*
MET_CV50_MixingRatio_gKg | g/kg | Mixing Ratio | `ClimaVUE50`*
MET_CV50_GHI_Wm2 | W/m^2 | Global Horizontal Irrandiance | `ClimaVUE50`*
MET_CV50_Precipitation_mm | mm | Measured Precipitation | `ClimaVUE50`*
MET_CV50_LtngStrikes_cnt | count | Number of detected lighting strikes | `ClimaVUE50`*
MET_CV50_LtngDist_km | km | Shortest lighting strike distance | `ClimaVUE50`*
SYS_CV50_TiltNS_deg | ° | Orientation Tilt | `ClimaVUE50`*
SYS_CV50_TiltWE_deg | ° | Orientation Tilt | `ClimaVUE50`*
SYS_BattVolt | V | Logger Power Supply | `CR6`  
SYS_PanelTemp | °C | Logger Wiring Panel Temperature | `CR6`  

* This output is optional and only written if the program is compiled with `SYS_ClimaVue50_Enable` = `TRUE` . If the program is complied with `SYS_ClimaVue50_Enable` = `FALSE` then these fields are not written and it is assumed no `ClimaVue50` is attached.

#### REA_RunSummary Table Variables

An entry in the `REA_RunSummary` table is written only at the end of a successful REA run. A continuous files in "TOA5" format is written to the SD card. Run Summary tables are transmitted by FTP to the FTP server specified and occur if an REA run has ended in the previous 5 seconds.

Variable Name | Units | Variable Desciption 
---- | ---- | ---- 
REA_Run_Start | UTC | Start time of REA run
REA_Run_Stop | UTC | End time of REA run
REA_Run_TimeUp | sec | Total time while sampling "up"
REA_Run_TimeDown | sec | Total time while sampling "down"
REA_Run_SwitchesUp | count | Number of times the "up" turned off/on
REA_Run_SwitchesDown | count | Number of times the "down" turned off/on
REA_Run_CO2AvgUp | mg/m^3 | Average CO2 concentration while sampling "up"
REA_Run_CO2AvgDown | mg/m^3  | Average CO2 concentration while sampling "down"
REA_Run_CO2DryAvgUp | ppm | Average dry CO2 concentration while sampling "up"
REA_Run_CO2DryAvgDown | ppm | Average dry CO2 concentration while sampling "down"
REA_Run_H2OAvgUp | g/m^3  | Average H2O concentration while sampling "up"
REA_Run_H2OAvgDown | g/m^3  | Average H2O concentration while sampling "down"
REA_Run_PressureAvgUp | kPa | Average air pressure while sampling "up"
REA_Run_PressureAvgDown | kPa | Average air pressure while sampling "down"
REA_Run_TempAvgUp | °C | Average temperature while sampling "up"
REA_Run_TempAvgDown | °C | Average temperature while sampling "down"
REA_DeadBandWidth | ---- | Deadband Width or holesize in HREA mode
REA_FreezeStatistics | ---- | If statistics are frozen or a moving average is used.
REA_Hyperbolic | ---- | If HREA Mode was selected = 1 (introduced in V38)
AGG_IRGASON_Uz_RunAvg | m/s | Mean vertical wind considered during run (makes only sense if `REA_FreezeStatistics` is chosen)
AGG_IRGASON_Uz_Stddev | m/s | Standard deviation of vertical wind considered during run  (makes only sense if `REA_FreezeStatistics` is chosen)
AGG_IRGASON_Uz_RunAvg | ppm | Mean of CO2 dry mole fraction considered during run (introduced in V38, makes only sense if `REA_FreezeStatistics` is chosen)
AGG_IRGASON_Uz_Stddev | ppm | Standard deviation of CO2 dry mole fraction considered (introduced in V38, makes only sense if `REA_FreezeStatistics` is chosen)

### Relaxed Eddy Accumulation (REA) operation

#### What is a REA run?

We call a REA run a period during which two samples are filled, an "up" sample and a "down" sample. The duration to fill a sampler is typically around 30 min to 1 hr, but depends on settings (deadband width) and atmospheric conditions. A REA run translates to a flux estimates for the compounds analyzed in the sample. Generally, the samples are alayzed for CO2, CH4, N2O and other trance cases and H20 at ICOS Central Anaytical Lab at MPI Jena and for 14C at the ICOS Central Radiocarbon Lab in Heidelberg.

#### REA Flags

During a REA run, flags are output in the column `REA_Flag` of table `RAW_20Hz` that document the system status and control actions in each 20 Hz step as follows: 

In the REA code 

- the last digit (1) is the newest state to which the system is in the current time step
- the second last digit (10) is the state in which the system was in the previous time step
- the third last digit (100) tells if the REA sampler is active (Voltage on Channel U7/U8 exceeds `SYS_REA_Sampler_Voltage_Threshold`

`REA_Flag` Digit | Meaning
---- | ---- 
0 | Both valves closed
1 | Up valve open, down valve closed
2 | Down valve open, up valve closed
3 | Both valves open (only if `REA_Manual_OpenValves` manually set to `1`)
9 | Both valves open during scheduled automatic maintenance (see below)

This combination will result in several control actions of the system (where X = {0,1}, with 0 = Sampler not active, 1 = Sampler active)

`REA_Flag` | Status of REA | Up valve | Down valve
---- | ---- | ----  | ---- 
-9999 | REA incative or not connected | remains closed | remains closed 
X00 | REA active and vertial wind within deadband | remains closed | remains closed 
X01 | REA active and vertial wind moves from within deadband to outside deadband (up) | opens | remains closed 
X02 | REA active and vertial wind moves from within deadband to outside deadband (down) | remains closed | opens 
X03 | REA active and vertial wind in previous step within deadband and user has just set `REA_Manual_OpenValves` to `1` | opens | opens |
X09 | REA vales are opened for scheduled maintenance | remains open for one second | remains open for one second |
X10 | REA active and vertial wind moves from outside deadband (up) to inside deadband | closes | remains closed 
X11 | REA active and vertial wind stay outside deadband (up) | remains open | remains closed 
X12 | REA active and vertial wind moves from outside deadband (up) to outside deadband (down) | closes | opens 
X13 | REA active and vertial wind in previous step outside deadband (up) and user has just set `REA_Manual_OpenValves` to `1` | remains open | opens |
X20 | REA active and vertial wind moves from outside deadband (down) to inside deadband | remains closed | closes 
X21 | REA active and vertial wind moves from outside deadband (down) to outside deadband (up) | opens | closes 
X22 | REA active and vertial wind stay outside deadband (down) | remains closed | remains open 
X23 | REA active and vertial wind in previous step outside deadband (down) and user has just set `REA_Manual_OpenValves` to `1` | opens | remains open |
X31 | REA active and user has just set `REA_Manual_OpenValves` back to `0` and system returns to wind outside deadband (up) | remains open | closes |
X32 | REA active and user has just set `REA_Manual_OpenValves` back to `0` and system returns to wind outside deadband (down) | closes | remains open |
X33 | REA active and user has left `REA_Manual_OpenValves` at `1` | remains open | remains open |

#### Starting a REA run

To initiate a REA run, a sampling event must be scheduled on the computer associated with the ICOS 24 Port Flask Sampler. The desired deadband width and the way the mean and standard deviation of the vertical wind are calculated (frozen vs. dynamic, see below) can be set in the fields next to `REA_DeadbandWidth` or `REA_FreezeWindStatistics` in the 'Public' table from the Table Monitor drop down menu on the LoggerNet Connect window. When the sampling start time is reached, the `REA_Sampler_Active_Voltage` shows ~5000 mV, signaling the valves at the top of the mast to begin switching based on the wind conditions measured by the IRGASON and the deadband width. When the REA Run is completed, the voltage will drop, and the Run_Summary table will be populated. At the end of the REA Run, both valves will open for 12 seconds to allow the remaining sample in the intake lines to be collected in the buffers. Once the 12 second period lapses, both intake valves will close. 

#### How is a REA run recorded by the logger?

The sampler is connected to the datalogger, the sampler sets a voltage at Channel U7/U8. If the sampler is running and pumps air in the buffers, then this voltage applied by a differentional signal at Channel U7/U8  should be about 5V.  If the sampler is off, or is running but does not pump air in the buffers / flasks, then this voltage should be about 0V. Once the buffers are full, the voltage drops from 5V to 0V. The public variable `SYS_REA_Sampler_Voltage_Threshold` in the program sets the threshold above which the sampler is active (by default 1500 mV).

If voltage monitored at Channel U7/U8 was in the previous scan loop (50 ms) above the value set in `SYS_REA_Sampler_Voltage_Threshold` in mV and drops in the current loop below the value of `SYS_REA_Sampler_Voltage_Threshold` then `REA_Active` is set to `0`. This should happen if one of the reservoirs of the sampler is full. The run is ended and the 12 second period to clear the line has lapsed, all valves are closed and run statistics are written. During start-up, when the voltage monitored at Channel U7/U8 is below `SYS_REA_Sampler_Voltage_Threshold` and also was below in the previous loop (50 ms) - nothing happens, this is the time the sampler is not yet ready yet, as denoted in the flags. The preset value in the code for `SYS_REA_Sampler_Voltage_Threshold` is 1500 (mV).

Immediately at the end of every REA run, both valves will be opened for 12 seconds. Once 12 seconds after the end of the run have elapsed, both vales are closed. The number of seconds (Default 12) is set by the parameter `SYS_REA_OpenValvesAtEnd`. During the 12 seconds, the variable `REA_Run_EndCounter` will increment. `REA_Run_EndCounter` will be zero during any other condition (so after the flushing ended, during a REA run).

#### How to change how wind is calculated (frozen vs. dynamic)

There are two options how the mean vertical wind and the standard deviation of the vertical wind are treated during any REA run.

- `REA_FreezeStatistics = 1`: The mean vertical wind (Uz) and the standard deviation of vertical wind (Sigma Uz) are frozen (=kept constant) at the beginning of the run, based on the statistics during the 30 minutes preceeding the run.

- `REA_FreezeStatistics = 0`: The mean vertical wind and the standard deviation of vertical wind will continue to be adjusted during the run based on their moving average (The length of the moving average is set by `SYS_MovingBlockDuration`. By default this is set to be 30 minutes).

To change these settings, select the 'Public' table from the Table Monitor drop down menu on the LoggerNet Connect window. Here enter the desired value (1 or 0) in the field next to `REA_FreezeStatistics`. This should be done before starting a run, not during a run (the latter will cause unexpected behaviour).

These settings also apply how in HREA run (see next paragraph) the CO2 mixing ratio and the standard deviation of the mixing ratio are calculated. So in the HREA mode it also affects CO2. 

#### Hyperbolic Relaxed Eddy Accumulation (HREA)

If the public variable `REA_Hyperbolic` is set to `1`, then a Hyperbolic Relaxed Eddy Accumulation (HREA) is chosen as the sampling mode. If `REA_Hyperbolic` is set to `0` then the classical deadband based on only vertical wind is implemented. The hyperbolic holesize is defined by the parameter `REA_DeadBandWidth` (so `REA_DeadBandWidth` sets both, the deadband for the classical separation based on vertical wind and the holesize of the hyperbolic hole.

In the HREA mode, the instantaneous product of ABS((c'-C)/Sigma_c) * ABS((w'-W)/Sigma_w) is calculated (displayed as `REA_HyperbolicDistance`).If `REA_HyperbolicDistance` < `REA_DeadBandWidth` then we are inside the hole region and no sampling takes place. If `REA_HyperbolicDistance` >= `REA_DeadBandWidth` then sampling happens based on the criteria whether w' is positive (updraft reservoir) or w' is negative (downdraft reservoir).

If `REA_FreezeStatistics = 1` then the HREA mode is run using a fixed ("frozen") mean CO2 mixing ratio (C), a fixed standard deviation of CO2 (Sigma_c), a fixed mean vertical wind (W), and a fixed standard deviation of the vertical wind (Sigma_w). 

If `REA_FreezeStatistics = 0` then the HREA mode is run using a variable ("running") mean CO2 mixing ratio (C), a variable standard deviation of CO2 (Sigma_c), a variable mean vertical wind (W), and a variable standard deviation of the vertical wind (Sigma_w).  

### Manual override (for testing only)

In the event that both valves located at the top of the mast need to remain open coninuously, e.g. for testing the Flask Sampler, select the 'Public' table from the Table monitor drop down menu on the LoggerNet Connect window and enter "1" in the field associated with BOTH the `REA Active` variable and the `REA_Manual_OpenValves` variable. Then both valves are open.

### Maintenance valves opening

To prevent the valves from clogging or corroding when not used for extended periods, twice a day, the valves are automatically scheduled and triggered to be opened and closed for just one second, at 01:00 and 13:00 UTC. The maintenance valve opening and closures will not be communicated to the sampler and they will not trigger a run. They just happen and are written with code `9 into the REA flag. Maintenance valve opening and closures will not happen if an active run is ongoing (no need).
