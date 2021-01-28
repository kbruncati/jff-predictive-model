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

* **Joining the data**

  Data were joined with DataFusion in Domo which is specialized to work with larger datasets. The datasets were joined via `customer_ID` column present in all datasets.

* **Making the new columns**

  `completion` and `percent_change_earnings` were created within RStudio via the `mutate` function which simply calls for the desired column name and the conditions to determine its values.

* **Shrinking data**

  In Domo, Magic ETLs were used to de-select undesirable columns so that exporting took up less computer space.

* **Filtering data**

  Rows were filtered for those only in training (which equates to an input of 1 in the `training_flag` column) via Magic ETL in Domo. RStudio's `na.omit` function helped to remove NAs.

* **Splitting data**

  Training and testing datasets were made via RStudio's `caret` package. These datasets were downloaded from R to upload into Domo, and are named as follows:
  * Objective #1 - `finaltrainingwages.csv`, `finaltestingwages.csv`
  
  * Objective #2 - `finaltrainingcompletion.csv`, `finaltestingcompletion.csv`
  
After completing these steps, the final dataset was saved within Domo under the title `Proper Columns`, which can also be seen from the code.
  
# Modeling Choices

The objectives were dealt with separately as the first called for prediction of specific values (the `percent_change_earnings` of the participant) whereas the second simply called for a binary response for `completion`.

## Objective #1: Random Forest Regression ##

Random forest regression was done via the `randomForest` package in RStudio. This approach allowed for total optimization of the model by being able to run individual testing to tune each component of the random forest process (such as `maxnodes`and `ntrees`). With this in mind, there is less of a risk of the machine overfitting to the data as many rounds are involved and this also helps to increase accuracy. 

## Objective #2: Binary Classification/Cross Entropy ##

Packages originally used in Python, `tensorflow` and `keras`, were imported to execute binary classification within RStudio. During the training process, each iteration of the computer viewing the data can be closely monitored with accuracy and loss measures, which was very helpful in troubleshooting. Averages are taken after the iterations are complete and helps to optimize the model. It was also very easy to view the results by developing a confusion matrix once prediction on the testing dataset was done.

# Results

## Objective #1: `percent_change_earnings` ##

The calculated R-squared upon prediction was 0.9948, yielding a very strong correlation between the predictors and `percent_change_earnings`. This indicates that there is something to be said about the demographics of a training participant and how their earnings will change upon their exit or completion of the program.

## Objective #2: `completion` ##

The final prediction accuracy was a less-exciting 0.4745, which is on the lower end of the spectrum. Despite this, there are still important conclusions to be made: the predictors selected for this process must simply not be the most important in telling whether or not an individual will successfully complete their program of choice.
