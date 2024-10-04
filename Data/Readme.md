## REAruns_Zurich
Data on the 103 REA sampling periods that were selected for flask analysis. ΔCO2 was simulated from high-frequency measurements of the MGA and the IRGASON to estimate measurement uncertainties due the sampling process (Sect. 5.2). Flask results are discussed in Sect. 5.3.2 and 6.

Column | Units | Description
---- | ---- | ---- 
REA_ID | - | Unique, consecutively assigned number for each REA run
REA_start | UTC | Date and time of REA run start, synchronized to IRGASON measurements
REA_end | UTC | Date and time of REA run end, synchronized to IRGASON measurements
REA_DeadBandWidth | - | Deadband width
REA_FreezeWindStatistics | - | If statistics are frozen or a moving average is used
REA_Run_SwitchesUp | Count | Number of times the "up" valve is opened/closed
REA_Run_SwitchesDown | Count | Number of times the "down" valve is opened/closed
d14C_meas_up | ‰ | Δ14C of updraft flask sample measured by AMS
d14C_meas_up_err | ‰ | Δ14C uncertainty of updraft flask sample measured by AMS
d14C_meas_down | ‰ | Δ14C of downdraft flask sample measured by AMS
d14C_meas_down_err | ‰ | Δ14C uncertainty of downdraft flask sample measured by AMS
CO2_meas_up | ppm | CO2 of updraft flask sample measured by GC
CO2_meas_up_err | ppm | CO2 uncertainty of updraft flask sample measured by GC
CO2_meas_down | ppm | CO2 of downdraft flask sample measured by GC
CO2_meas_down_err | ppm | CO2 uncertainty of downdraft flask sample measured by GC
DCO2_meas | ppm | CO2 difference between up- and downdraft flask samples measured by GC
DCO2_meas_err | ppm | Uncertainty of CO2 difference between up- and downdraft flask samples measured by GC
DCO2_MGA | ppm | CO2 difference between up- and downdraft samples estimated from high-frequency measurements of the MGA
DCO2_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from high-frequency measurements of the IRGASON
DCO2_lag100ms_MGA | ppm | CO2 difference between up- and downdraft samples estimated from MGA measurements with 100 ms time lag between wind signal and CO2
DCO2_lag100ms_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from IRGASON measurements with 100 ms time lag between wind signal and CO2
DCO2_lag200ms_MGA | ppm | CO2 difference between up- and downdraft samples estimated from MGA measurements with 200 ms time lag between wind signal and CO2
DCO2_lag200ms_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from IRGASON measurements with 200 ms time lag between wind signal and CO2
DCO2_lag300ms_MGA | ppm | CO2 difference between up- and downdraft samples estimated from MGA measurements with 300 ms time lag between wind signal and CO2
DCO2_lag300ms_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from IRGASON measurements with 300 ms time lag between wind signal and CO2
DCO2_lag400ms_MGA | ppm | CO2 difference between up- and downdraft samples estimated from MGA measurements with 400 ms time lag between wind signal and CO2
DCO2_lag400ms_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from IRGASON measurements with 400 ms time lag between wind signal and CO2
DCO2_lag500ms_MGA | ppm | CO2 difference between up- and downdraft samples estimated from MGA measurements with 500 ms time lag between wind signal and CO2
DCO2_lag500ms_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from IRGASON measurements with 500 ms time lag between wind signal and CO2
DCO2_lag600ms_MGA | ppm | CO2 difference between up- and downdraft samples estimated from MGA measurements with 600 ms time lag between wind signal and CO2
DCO2_lag600ms_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from IRGASON measurements with 600 ms time lag between wind signal and CO2
DCO2_lag700ms_MGA | ppm | CO2 difference between up- and downdraft samples estimated from MGA measurements with 700 ms time lag between wind signal and CO2
DCO2_lag700ms_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from IRGASON measurements with 700 ms time lag between wind signal and CO2
DCO2_lag800ms_MGA | ppm | CO2 difference between up- and downdraft samples estimated from MGA measurements with 800 ms time lag between wind signal and CO2
DCO2_lag800ms_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from IRGASON measurements with 800 ms time lag between wind signal and CO2
DCO2_dtrinse-4s_MGA | ppm | CO2 difference between up- and downdraft samples estimated from MGA measurements when the rinse time is 4 s too short
DCO2_dtrinse-4s_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from IRGASON measurements when the rinse time is 4 s too short
DCO2_dtrinse-2s_MGA | ppm | CO2 difference between up- and downdraft samples estimated from MGA measurements when the rinse time is 2 s too short
DCO2_dtrinse-2s_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from IRGASON measurements when the rinse time is 2 s too short
DCO2_dtrinse2s_MGA | ppm | CO2 difference between up- and downdraft samples estimated from MGA measurements when the rinse time is 2 s too long
DCO2_dtrinse2s_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from IRGASON measurements when the rinse time is 2 s too long
DCO2_dtrinse4s_MGA | ppm | CO2 difference between up- and downdraft samples estimated from MGA measurements when the rinse time is 4 s too long
DCO2_dtrinse4s_IRGA | ppm | CO2 difference between up- and downdraft samples estimated from IRGASON measurements when the rinse time is 4 s too long
std(DCO2_var103) | ppm | Standard deviation of 103 ΔCO2 estimates obtained from high-frequency measurements of the MGA or, if not available, the IRGASON, with varying weights of the measurements
std(CO2)_MGA | ppm | Standard deviation of CO2 measured by the MGA during the REA run
std(CO2)_IRGA | ppm | Standard deviation of CO2 measured by the IRGASON during the REA run
Comment | - | Comment on data availability and micrometeorological conditions


## qc_lab.csv
Results of the quality control tests of the REA flask sampling system that were carried out in the ICOS Flask and Calibration Laboratory in Jena to investigate bias or uncertainty due to contamination and memory effects from preceding REA runs. "qc1_1" - "qc1_6" refer to the contamination tests described in Appendix B1, while "qc2_1" - "qc2_6" refer to the memory effect test described in Appendix B2.

Column | Units | Description
---- | ---- | ---- 
Test_no  | - | Test number 
CO2_meas_up  | ppm | CO2 measured in the "up" flask at the GC
CO2_meas_up_err  | ppm | CO2 uncertainty measured in the "up" flask at the GC
CO2_meas_down  | ppm | CO2 measured in the "down" flask at the GC
CO2_meas_down_err  | ppm | CO2 uncertainty measured in the "down" flask at the GC



## qc_Zurich.csv
Results of the "all-valves-open tests" performed during the Zurich campaign to check for biases between updraft and downdraft sampling, as well as for leaks or other sources of contamination. Details are described in Sect. 5.3.1.


Column | Units | Description
---- | ---- | ----
Test_no | - | Test number
Test_start  | UTC | Start of the all-valves-open test
Test_end  | UTC | End of the all-valves-open test
CO2_meas_up  | ppm | CO2 measured in the "up" flask at the GC
CO2_meas_up_err  | ppm | CO2 uncertainty measured in the "up" flask at the GC
CO2_meas_down  | ppm | CO2 measured in the "down" flask at the GC
CO2_meas_down_err  | ppm | CO2 uncertainty measured in the "down" flask at the GC
CO2_meas_direct  | ppm | CO2 measured in the "direct" flask at the GC
CO2_meas_direct_err  | ppm | CO2 uncertainty measured in the "up" flask at the GC
