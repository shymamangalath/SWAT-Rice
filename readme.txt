SWAT-Rice and Multi-Model Paddy Simulation: Example Dataset, Model Setup, Calibration, Results, and Execution Guide

This package contains the executable files, modified source code, Visual Studio compilations, model setup files, calibration files, simulation results, and inter-model comparison results for two representative paddy fields (Field_1 and Field_8). The package includes simulations using five models:
• ORYZA
• DSSAT CERES-Rice
• SWAT-Rice (SWAT-670 ver)
• SWAT-Pothole (SWAT-690 ver)
• HYDRUS-1D
________________________________________

1. Repository Contents

The model-specific folders for the two representative paddy fields are organized as follows:

• DSSAT simulations
• HYDRUS-1D simulations
• ORYZA simulations
• SWAT-Pothole simulations
• SWAT-Rice simulations

For each field, the package includes the corresponding model setup, model calibration, and simulation results. 

• Results_inter model comparisons : provides comparison of the five model simulations across ten paddy fields.


The SWAT-Rice and SWAT-Pothole model folders are accompanied by:
• Source code
• Executable files
• Visual Studio compilation files
________________________________________

2. SWAT-Rice: Required Input Files and Model Execution Guide

The SWAT-Rice simulation folders contain the model setup and simulation files for the representative paddy fields.

2.1 Required SWAT Input Files
________________________________________

2.1.1 Basin (BSN) File
Set the crack-flow option as:
ICRK = 0
where:
• ICRK = 1 enables simulation of preferential flow through soil cracks.
________________________________________

2.1.2 HRU File
To maintain compatibility with the standard SWAT interface, existing pothole variables are reassigned internally within the source code (readhru.f) to represent paddy-field parameters.

SWAT Variable    SWAT-PADDY Parameter    Description
POT_TILE         switpd                  Puddling switch
POT_VOLX         H_max                   Bund height (mm)
POT_VOL          flood                   Initial ponded water depth (mm)
POT_NSED         swcon                   Whole-profile drainage rate constant
POT_NO3L         pud_coef                Puddling coefficient
SOLP_CON         PFCR                    Critical soil-water tension for crack penetration through the plow sole
POT_SOLP         dep_pd                  Depth of puddled layer (mm)
GWHT             gwDt                    Initial groundwater depth [m]

Note: PFCR is required only when crack-flow simulation is activated.
________________________________________

2.1.3 Groundwater (GW) File
For simulations with shallow groundwater table dynamics, provide values for:
• GWQMN
• REVAPMN
• GW_SPYLD
• gwDt

The groundwater datum and SHALLST are calculated as:
GW_datum = (REVAPMN / GW_SPYLD) + SOL_Z
SHALLST = (GW_datum - gwDt) * GW_SPYLD

where:
• SOL_Z = total soil profile depth (mm)
• GWDT = groundwater table depth below the soil surface (mm)
________________________________________

2.1.4 Management (MGT) File
Manual Irrigation
Specify the irrigation amount directly within the management operations.

Automatic Irrigation (Continuous Flooding)
Use:
• WSTRS_ID = 3
• AUTO_WSTR = minimum ponded water depth that triggers irrigation (mm)
• IRR_MX = target ponded water depth after irrigation (mm)
________________________________________

3. Running SWAT-Rice

After preparing the input files:
1. Place all required input files in the TxtInOut directory.
2. Select the appropriate executable:
        SWAT670_debug.exe for debugging mode
        SWAT670_rel.exe for release mode
3. Execute the model.
4. After successful completion, examine the output files:
        output.std
        output.hru
        output.wtr
________________________________________

4. Output Files

output.wtr
To generate output.wtr, set:
IWTR = 1, in the file.cio file.
The output.wtr file contains additional variables for paddy HRUs such as;
• Ponded water depth (FLOODmm)
• Infiltration (INFILTmm)
• Percolation (PERCmm)
• Lateral flow (LATmm)

output.std
The output.std file contains:
• Surface runoff (SURQ, mm)
• Lateral flow (LATQ, mm)
• Baseflow (GWQ, mm)
• Percolation (PERCOLATE, mm)
• Total soil water content (SW, mm)
• Actual ET (ET, mm)
• Potential ET (PET, mm)
• Water yield (mm)
• Irrigation applied (mm)