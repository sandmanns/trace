# TRACE: TRAnsfer and Contact nEtworks
Contact networks for the interactive analysis of transfer chains and contacts in hospitals

<p align="center">
    <img height="600" src="https://uni-muenster.sciebo.de/s/1A6rqzAA710sIwG/download">
</p>


## Requirements
To run TRACE, you need R (Version 4.1.0 or higher) and R Shiny.

##  Installation
To install TRACE, just download the repository. All required packages will be installed, all functions loaded automatically by the help of the global.R script.

## Running TRACE
ICONE is available as an R Shiny GUI. To run the software: 1) navigate to the folder in which the following files are stored: `global.R`, `ui.R`, `server.R`, `www/UKM.png` and `www/white.png`. 2) Execute `Run App`.


## Examplary use of TRACE

## Input
To execute any analyses with TRACE, in input file has to be 1) read in and 2) configured.

### Read in file

* Clinics:
    * `Upload data` Select a tabular file for upload, containing information on admission and discharge of cases from clinics.
    * `Separator` Select the field separator used in the input file (one of: Comma, Semincolon, Tab). 
* Units:
    * `Upload data` Select a tabular file for upload, containing information on admission and discharge of cases from units.
    * `Separator` Select the field separator used in the input file (one of: Comma, Semincolon, Tab). 

Note: at least one file has to be uploaded. A second file, e.g. providing more fine-granular information on transfer of cases between units, so, within one clinic, can be uploaded.


### Configure input

* `Select column containing information on...`
  * `...case number` Name of the column containing information on the considered cases (case ID).
  * `...clinic/unit` Name of the column containing information on the clinic/unit.
  * `...admission` Name of the column containing information on the admission of the case to the clinic/unit (format requirement: YYYY-MM-DD hh:mm:ss).
  * `...discharge` Name of the column containing information on the discarge of the case from the clinic/unit (format requirement: YYYY-MM-DD hh:mm:ss).
  * `...main diagnosis` Name of the column containing information on the main diagnosis (ICD code).

    
### Example

Select `Load demo data`to read in the simulated exemplary input files `Clinic_V2.txt` and `Unit_V2.txt`. Both files are available in the www folder and can, as an alternative, be uploaded manually.

In case you are reading in the demo data automatically, the colums to configure the input are already defined correctly by default ('CaseID' for `case number`, 'Clinic' for `clinic`, 'Unit' for `unit`, 'Admission' for `admission`, 'Discharge' for `discharge` and 'MainDiagnosis' for `main diagnosis`).



## Transfers & Contacts

You can switch between an analysis per clinic and per unit.

### Analysis of a clinic/unit
An analysis can be conducted for one or more clinics (resp. units). Optionally, only cases with selected ICD codes or chapters my be considered.

### Selected observation period
For the analysis, an observation period is selected.

The earliest date for the `Start of observation period` is the first day for which information on the clinic in focus is available in the uploaded input file. The latest date for the `Start of the observation period` is the last day for which information on the clinic in focus is available in the uploaded input file.

The earliest date for the `End of observation period` is the first day for which information on the clinic in focus is available in the uploaded input file. The latest date for the `End of the observation period` is the last day for which information on the clinic in focus is available in the uploaded input file.

Please consider that for a long observation period, the calculations may - dependent on the computer's performance - take up to 1 minute.

### Contact analysis
For the contact analysis, 1 parameter has to be defined:

* Minimum contact time: The time that 2 cases have to be in contact, so that a contact is displayed for them (default: 1 hour).


## Results
The primary result of the analysis with TRACE is an interactive contact network (R package 'networkD3').

### Analysis of 1 clinic

<b> Visualization: </b>

If only a single clinic/unit is picked for analysis, contacts of all cases within the selected observation period in this clinic are analyzed, based on the defined configuration.

Every contact between 2 cases (nodes) is visualized by a gray edge. The edge's thickness correlates with the length of contact time. The size of the nodes correlates with the length of stay in the selected clinic/unit.

If a node is displayed with a black border, this indicates that the case visited at least one more clinic/unit during the observation period.

<b> Pop-up window: </b>

For every node, detailed information can be obtained. By clicking on the node, a window pops up. The following information is provided:

* Case
* Main diagnosis
* Length of stay altogether
* Contacts at clinic
* Length of stay at clinic
* Further clinics visited


<b> General informationen: </b>

Oberhalb der Visualisierung sind Basis-Informationen zusammengefasst:

* Konfiguration der Analyse
    * Abteilung ausgewählt
    * ICD-Filter
    * Beobachtungszeitraum
    * Minimale Kontaktzeit
* Weiterführende Informationen
    * Anzahl Fälle
    * Kontakte innerhalb der Abteilung
    * Anzahl Fälle mit Verlegung von der gewählten Abteilung
    * Top-5 Ziel-Abteilungen
    * Anzahl Fälle mit Verlegungen hin zur gewählten Abteilung
    * Top-5 Ursprungs-Abteilungen

On top of the visualization, basic information is summarized:

* Configuraiton of the analysis
    * Clinic
	  * ICD filter
  	* Observation period
	  * Minimum contact time
* Additional information
    * Number of cases
    * Contacts within the clinic
    * Number of cases with transfers from the selected clinic
    * Top-5 target clinics
    * Number of cases with transfers to the selected clinic
    * Top-5 source clinics



<b> Example: </b>

Please select:

* Clinics
* Analysis of the clinics: Clinic 1
* Filter main diagnosis for ICD code: no
* Select observation period
    * Start of observation period: 2023-01-02
    * End of observation period: 2023-02-01
* Contact analysis
    * Minimum contact time: 6 Hours


### ### Analysis of >1 clinic

<b> Visualization: </b>

Für jeden Fall wird auf jeder besuchten Abteilung ein Fall-Knoten erstellt. Dies führt dazu, dass ein Fall, der während des Beobachtungszeitraums 2 Abteilungen besucht hat, 2x als Knoten in dem Netzwerk auftritt. Diese Knoten sind durch eine schwarze Kante verbunden und zeigen so die Verlegung des Falls zwischen zwei Abteilungen an. 

If two or more clinics/units are selected for analysis, the transfers of all cases within the observation period between these clinics are analyzed, along with an additional contact analysis within the clinics, according to the defined configuration.

Every clinic is represented by a large clinic-node. Every clinic is assigned an individual color. The size of the node correlates with the number of cases considered at the clinic. All cases in the clinic are represented as smaller case-nodes in the same color as the clinic and are connected to the clinic-node by a grey edge. The size of the case-nodes correlates with the length of stay on the respective clinic or unit.

If a case-node is displayed with a black border, this indicates that the case has visited at least one other clinic or unit during the observation period (considering all clinics, not only those selected for analysis).

For every case, a case-node is created for every clinic he/she visited. This means that a case, who has visited 2 departments during the observation period, is represented by 2 nodes in the network. These nodes are connected by a black edge, indicating the cases' transfer between the two departments.

<b> Pop-up window per clinic/unit: </b>

* Clinic/Unit
* Cases altogether
* Number of cases with transfer from the selected clinic
* Top-5 target clinics
* Number of cases with transfer to the selected clinic
* Top-5 source clinics
* Average length of stay altogether
* Average length of stay at the selected clinic

<b> Pop-up window per case: </b>

* Case
* Main diagnosis
* Contacts altogether
* Length of stay altogether
* Contacts at clinic
* Length of stay at unit
* Further units visited


<b> General information: </b>

* Configuration of the analysis
    * Clinics selected
    * Clinics included in analysis (Note: this may not be all of the selected clinics if, according to the defined configuration, all cases from a clinic are being filtered)
    * ICD filter
    * Observation period
    * Minimum contact time
* Additional information
    * Number of cases
    * Number of contacts within the units
    * Number of transfers between the units


<b> Example: </b>

Please select:

* Units
* Analysis of units: Unit 10A, Unit 10B, Unit 2A, Unit 2B, Unit 8A
* Filter main diagnosis for ICD code: no
* Select observation period
    * Start of observation period: 2023-03-12
    * End of observation period: 2023-04-11
* Contact analysis
    * Minimum contact time: 1 Hour

 


## Summary Transfers

You can switch between an analysis per clinic and per unit.

### Transfers
One clinic is at the center of the analysis. It may be switched between two perspectives: Transfers away from the clinic (`from`) or transfers to the clinic (`to`). Optionally, only cases with selected ICD codes or chapters my be considered.  

### Selected observation period
For the analysis, an observation period is selected. 

The earliest date for the `Start of observation period` is the first day for which information on the clinic in focus is available in the uploaded input file. The latest date for the `Start of the observation period` is the last day for which information on the clinic in focus is available in the uploaded input file.

The earliest date for the `End of observation period` is the first day for which information on the clinic in focus is available in the uploaded input file. The latest date for the `End of the observation period` is the last day for which information on the clinic in focus is available in the uploaded input file.

For this summarizing analysis, large observation periods are recommended (default: 1 year). 


## Results
The primary result of the analysis with TRACE is an interactive transfer network (R package 'networkD3').

<b> Visualization: </b>

The selected clinic/unit is placed in the center of the network. Clinics from/to which transfers have taken place, are connected with the central clinic by a gray, directed edge. The arrow shows the direction of transfer. The size of the nodes correlates with the total number of cases at the clinic in the observation period. The edges' thickness correlates negative with the effective distance of the two clinics (small distance, thick endge). Additionally, the diestance of two nodes correlates positive with their effective distance.

The effective distance is calculated according to Brockmann and Helbing 2013 as 1-log(x/y), where x defines the number of cases that were transferred from A to B, and y defines the total number of cases that were transfered to A. Analogously for `to`.


<b> Pop-up window: </b>

For every node, detailed information can be retrieved. By clicking a node, a pop-up window opens. The following information is provided:

* Clinic
* Number of cases altogether
* Number of cases transferred (if a case is transferred from A-B-A-C, two transfers from A take place, which results in the case being counted twice for clinic A; therefore, the number of transferred cases may be lager than the total number of cases)
* Fraction of cases from clinic \<center\> to clinic
* Effective distance based on cases transferred to the central clinic
* Effective distance based on all cases atthe central clinic


<b> General information: </b>

On top of the visualization, the following basic information is summarized:

* Configuration of the analysis
    * From
    * To
    * ICD filter
    * Observation period

<b> xlsx export: </b>

TRACE provides the option to export all results as xlsx file. The following information is included:

* From
* ICD filter
* Observation period
* Number of cases altogether
* Number of cases transferred
* Details
    * Transferred from clinic to
    * Number of patients absolute
    * Number of patients relative
    * Effective distance based on transferred cases
    * Effective distanz based on all cases


<b> Example: </b>

Please select:

* Units
* Transfers: from
* Unit: Unit 12A
* Filter main diagnosis for ICD code: no
* Select observation period
    * Start of observation period: 2023-01-01
    * End of observation period: 2024-01-01
