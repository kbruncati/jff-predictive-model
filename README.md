# jff-predictive-models
Two objectives were tackled here: Objective #1 refers to developing a predictive model of a workforce training participant’s percent change in earnings and Objective #2 refers to developing a predictive model of whether or not a participant will complete their training program.  

# Data
All data used to complete these objectives pertains to Dallas, TX. The data was provided by Workforce Solutions Greater Dallas and included childcare information, earnings information, demographics of the participants, and training provider information.  Columns used include: 

Title         | Meaning
--------------|-------------------------------------------------------------------------------
training_flag | whether or not the participant was engaged in training or a different service
at_risk       | whether or not the participant was at-risk youth
start_dt      | start date of training participation
exit_dt       | end date of training participation
gender_cd     | gender of the participant
msfw          | whether or not the participant was a migrant farm worker
customer_id   | the ID of each individual participant
runaway_youth | whether or not the participant was runaway youth
cm_claimant   | whether or not the participant received a Regular UI
pregnant_youth| whether or not the participant was pregnant youth
postq1earnings| the participant's earnings Q1 post-exit
postq2earnings| the participant's earnings Q2 post-exit
 
Two columns were created to assist in completing the objectives: `completion` (whether or not the participant completed the training program based on start and end dates) and `percent_change_earnings` (the percent increase or decrease in the participant's wage post-exit determined via the Q1 and Q2 earnings information).

# Wrangling and Preparation
Data were wrangled and prepared for the training process with help from features of Domo as well as within RStudio itself.

* Joining the data
Data were joined with DataFusion in Domo which is specialized to work with larger datasets. The datasets were joined via `customer_ID` column present in all datasets.

* Making the new columns
`completion` and `percent_change_earnings` were created within RStudio via the `mutate` function which simply calls for the desired column name and the conditions to determine its values.

* Shrinking data
In Domo, Magic ETLs were used to de-select undesirable columns so that exporting took up less computer space.

* Filtering data
Rows were filtered for those only in training (which equates to an input of 1 in the `training_flag` column) via Magic ETL in Domo. RStudio's `na.omit` function helped to remove NAs.

* Splitting data
Training and testing datasets were made via RStudio's `caret` package. 
