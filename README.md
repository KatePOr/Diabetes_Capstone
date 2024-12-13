# Assessing the Influence of Social, Demographic, and Health Factors on Diabetes Risk
By: Stephanie G. and Kate O.
## Project Details and Overview
This project investigates how social, demographic, and health factors influence the likelihood of developing diabetes, addressing a gap in public health research that often overlooks the role of socio-economic and lifestyle factors. Using the CDC’s BRFSS 2023 dataset, we applied machine learning models, including Random Forest and XGBoost, to identify key predictors and make accurate risk predictions. The findings highlight the importance of BMI, age, high blood pressure, and income, offering actionable insights for public health organizations to design targeted interventions.
## Background & Question
### Research Question:
"How do social factors, demographic characteristics, and health conditions influence the likelihood of developing diabetes?"
### Why Is It Worth Exploring?
The increasing prevalence of diabetes, especially in vulnerable populations, would benefit from a deeper understanding of the social and demographic factors influencing it. By exploring these aspects in combination with health conditions, we can offer more tailored prevention strategies for at-risk groups. This research is valuable in helping public health initiatives address not just medical but also socio-environmental contributors to diabetes.
### Is the Question Novel or Original?
This question is not entirely new but offers a fresh perspective by integrating social and demographic factors with health conditions to predict diabetes risk. While research exists on individual factors, this study takes a more comprehensive approach by looking at how these elements interact. The use of the BRFSS CDC data source and combining social factors with health conditions provides new opportunities for insights and interventions.
### Hypothesis:
Social factors (e.g., income, education), demographic characteristics (e.g., age, gender), and health conditions (e.g., high blood pressure) together influence the likelihood of developing diabetes.
### Prediction:
The data will show that certain demographic groups (e.g. older individuals or those from lower-income backgrounds) who also have specific health conditions (e.g. hypertension) are more likely to develop diabetes. Additionally, social factors, such as limited access to healthcare and education, will likely increase the risk of developing diabetes.
## Data and Methods
### Dataset:
This dataset contains information about social factors, demographic characteristics, and lifestyle factors that may impact the likelihood that an individual has diabetes. This dataset is sourced from the BRFSS Survey 2023 performed by the CDC (https://www.cdc.gov/brfss/annual_data/annual_2023.html) as an SAS transport file and imported into R for processing. According to the CDC, the BRFSS Survey is a multi-mode (mail, landline, and cell phone) survey administered by the CDC every year since 1984. The data is collected from all 50 U.S. states, the District of Columbia, and three U.S. territories. The major concerns or caveats present for this dataset include the possibility that, with such a large scale survey, there may be data entry errors, response bias, and recall bias. These are common problems with surveys, especially those that ask people to self-report situations as the CDC does here.
The Louisiana Department of Health states that there are “four types of health factors: health behaviors, clinical care, social and economic, and physical environment factors” (Health Factors & Behaviors). This project will focus on the social factors (PHYSHLTH, MENTHLTH, PERSDOC3, MEDCOST1, SMOKE100, DRNK3GE5, EXERANY2), demographic characteristics (EDUCA, EMPLOY1, INCOME3, WEIGHT2, HEIGHT3, SEXVAR), and health conditions (BPHIGH6, TOLDHI3, CVDINFR4, CVDCRHD4, CVDSTRK3, CHCKDNY2).
### Response/Outcome Variable:
The response/outcome variable will be a combination of DIABETE4 and DIABTYPE. The result will be a variable that indicates whether or not an individual has diabetes or prediabetes (1) or does not have diabetes (0).
### Predictor Variables:
The predictor variables are:
- Social factors:
  - PHYSHLTH: Indicates how many days the person experienced poor physical health in the last 30 days.
  - MENTHLTH: Indicates how many days the person experienced poor mental health in the last 30 days.
  - PERSDOC3: Indicates if the individual has at least 1 personal doctor.
  - MEDCOST1: Indicates if the individual has been unable to afford a doctor when needed in the last 12 months.
  - SMOKE100: Indicates if the individual has smoked at least 100 cigarettes in their lives.
  - DRNK3GE5: Indicates how many times during the past 30 days the individual had 5 or more drinks for men or 4 or more drinks for women on an occasion.
  - EXERANY2: Indicates if the person has exercised, aside from any exercise from a job, in the last month.
- Demographic characteristics:
  - EDUCA: Individuals indicate the highest education they have received.
  - EMPLOY1: Individuals indicate their current employment status.
  - INCOME3: Individuals indicate their annual income from provided ranges.
  - WEIGHT2: Individuals indicate their weight in pounds or in kilograms.
  - HEIGHT3: Individuals indicate their height in feet and inches or meters.
  - SEXVAR: Individuals indicate their sex.
- Health conditions:
  - BPHIGH6: Individuals indicate if they have ever been told by a doctor, nurse or other health professional that they have high blood pressure.
  - TOLDHI3: Individuals indicate if they have ever been told by a doctor, nurse or other health professional that their cholesterol is high.
  - CVDINFR4: Individuals indicate if they have been told they had a heart attack, also called a myocardial infarction.
  - CVDCRHD4: Individuals indicate if they have been told they had angina or coronary heart disease.
  - CVDSTRK3: Individuals indicate if they have been told they had a stroke.
  - CHCKDNY2: Individuals indicate if they have been told they had kidney disease, not including kidney stones, bladder infection or incontinence.
## Analysis Plan:
### Data Cleaning:
Data cleaning steps are included in the data cleaning/processing column in table 1. The current plan for unknown values is to exclude them from the dataset. This may not be feasible depending on how extensive the unknown values are. If there are too many NA values, the columns may either be too sparse to include as a predictor variable or maximum likelihood may be investigated as an alternative. Without any cleaning, the dataset contains 350 columns and 433,323 rows. This will be narrowed down to the 21 columns listed in table 1. 
Further processing will simplify the variables that are present and combine height and weight into one column, BMI. Binge drinking will be defined in the column DRNK3GE5 according to the U.S. Department of Health and Human Services. “Substance Abuse and Mental Health Services Administration (SAMHSA), which conducts the annual National Survey on Drug Use and Health (NSDUH), defines binge drinking as 5 or more alcoholic drinks for males or 4 or more alcoholic drinks for females on the same occasion (i.e., at the same time or within a couple of hours of each other) on at least 1 day in the past month” (U.S. Department of Health and Human Services, 2024). 

DIABETE4 and DIABTYPE will be used to create the response variable column. DIABETE4 indicates whether the individual has diabetes and whether the diagnosis was during pregnancy. DIABTYPE indicates what type the individual was told they have. As we are focusing on type 2 diabetes in the general population, individuals who became diabetic during pregnancy and individuals with type 1 diabetes will be excluded. If the person did not indicate the type of diabetes despite indicating that they have diabetes, they will need to be excluded from the dataset. People with type 2 diabetes or are prediabetic will be determined by those who answer 1 on DIABETE4 and 2 on DIABTYPE or 4 on DIABETE4.
### Predictive Modeling:
The tentative plan is to run predictive models including logistic regression, Random Forest, and xgBoost models to predict the likelihood of an individual having diabetes based on each set of predictor variables in the previous section. Logistic regression was chosen for its high interpretability allowing us to see how each variable is influencing the model. Random Forest and xgBoost models are “ensemble techniques used to solve regression and classification problems that have evolved and proved to be dependable machine learning challenge solver” (Fatima et. al., 2023). Hong et. al. (2022) used logistic regression, random forest, and xgBoost to predict COVID-19 disease severity based on laboratory testing. As these models appear to be commonly used in public health research, these models should be well interpreted and received by the primary stakeholders. 

Given the seven-week timeline, the team is confident in being able to complete the analysis by focusing on efficient data preparation and fine-tuning of model settings, using R’s built-in tools for training, testing, and checking the models. 

The models will output a number between 0 and 1. The preliminary threshold for diabetes will be set at 0.50. If the model outputs a prediction of 0.50 or greater, the individual will be predicted to have diabetes. Otherwise, the individual will be predicted to not have diabetes. Sensitivity and specificity will be determined with the preliminary threshold to determine if the threshold should remain or be updated.

Lasso regression will be used preliminarily to determine if the chosen predictor variables are significant in the model. The accuracy of the models and their overfitting or underfitting will be compared to determine what factors are the best predictors of diabetes. According to Ranstam et. al. (2018), lasso regression (a.k.a. Least absolute shrinkage and selection operator) is a good option for feature selection as it addresses potential model overfitting and model overestimation (a.k.a. “Optimism bias). The Lasso penalty (lambda) determines the strength by which  less significant coefficients will be set to zero, helping narrow down the most significant predictors (Galli, 2022). The penalty will be set to balance the bias-variance tradeoff. Variables with coefficients set to zero by Lasso can then be excluded from logistic regression, Random Forest, or xgBoost, ensuring only the strongest predictors are used. The potential pitfalls of this plan include that some of the factors believed to be important may not be significant in the Lasso regression indicating that it would not make sense to run the predictive models including those variables.
## What will indicate if the question is answered and the hypothesis supported?
The question will be answered depending on the accuracy and fit of the models that are trained. The accuracy and fit (AUC and ROC graph) will be compared between the groups of predictor variables. Multiple models will be trained per group to ensure that the ability of those variables to predict if the individual has diabetes is not due to the predictive model but due to the variables being used to train. The hypothesis will first be supported if the lasso regression shows that there are variables within each group of predictors that are significant to the model. Second, the accuracy of the different models will be compared and, if all the models have high accuracy, the hypothesis will be supported. The variables found to be significant between the different groups and between the two datasets will be recommended to be focused on in future research. 

If the models identify different important variables or show low accuracy, it could indicate a few things. First, if each model highlights different variables, this may suggest complex relationships between predictors and diabetes risk, or it could show that some predictors provide similar information, causing them to substitute for one another in importance. Second, if all models perform poorly, it might mean that social and demographic predictors alone don’t fully capture diabetes risk, possibly missing key influences like genetics. Low accuracy could also result from an uneven number of cases with and without diabetes, or simply from limitations in model choice, suggesting that richer data or more advanced methods may be needed.

## Instructions for Running the Code
To replicate this analysis, start by downloading the 2023 BRFSS Data (SAS Transport Format) dataset from the CDC website (https://www.cdc.gov/brfss/annual_data/annual_2023.html). Unzip the file prior to running any analyses. Ensure that R is installed on your system, along with the packages indicated in the beginning of each file where libraries are loaded. Run the scripts in the following order: (1) Data_Cleaning.Rmd, (2) Exploratory_Data_Analysis.Rmd, (3) Preprocessing_and_Feature_Engineering.Rmd, (4) Initial_Random_Forest_Model_and_Assumptions.Rmd, (5) Random_Forest.Rmd, and (6) xgBoost_All_Predictors.Rmd. The CDC_2023_cleaned.csv file is the file resulting from downloading the SAS file and running the Data_Cleaning.Rmd file. This can be used to run files 2 through 5. The Exploratory_Data_Analysis.Rmd contains many graphs used to analyze the data being used for modeling. Only the sections of code labeled with **Used in report** were used to produce graphs included in the Assessing_the_Influence_of_Social_Demographic_and_Health_Factors.pdf paper. The Initial_Random_Forest_Model_and_Assumptions.Rmd contains the preliminary random forest model and the graphs that show that the assumptions of the random forest and xgBoost models are met.

### Custom Function
A custom function of normalize is used in Preprocessing_and_Feature_Engineering.Rmd, Random_Forest.Rmd, and xgBoost_All_Predictors.Rmd. 

### Troubleshooting and Recommendations
During the analysis, users may encounter missing package errors or file path issues. To resolve these, ensure all required R packages are installed and that dataset files are placed in the correct directories. For additional support, refer to the CDC’s BRFSS documentation for data-related questions or the R manual for troubleshooting R-specific issues. Recommendations for future improvements include exploring additional clinical predictors, such as A1C levels, and testing the models on datasets from previous years to validate their performance over time.

## Team Duties and Work on Project
Both group members were involved in writing code and writing the final report. Individuals wrote much of the code separately but came together to review the code weekly where constructive feedback was given and ensured the code was running properly after all commits on both group members computers. All files were tested by both group members and ran without issue. The group members initials are present next to  each section of code to indicate who wrote each part. These initials indicate the delegated section assignments.

## References:
CDC. (2023) Behavioral Risk Factor Surveillance System (BRFSS) 2023 Data [Dataset]. CDC. https://www.cdc.gov/brfss/annual_data/annual_2023.html
Fatima, S., Hussain, A., Amir, S. B., Ahmed, S. H., & Aslam, S. M. H. (2023). Xgboost and random forest algorithms: an in depth analysis. Pakistan Journal of Scientific Research, 3(1), 26-31.
Galli, S. (2022, August 16). Lasso Feature Selection with Python. Train in Data.  https://www.blog.trainindata.com/lasso-feature-selection-with-python/ 
Health Factors & Behaviors. Health Factors | La Dept. of Health. (n.d.-b). https://ldh.la.gov/page/health-factors#:~:text=Health%20risk%20factors%20are%20attributes,economic%2C%20and%20physical%20environment%20factors. 
Hong, W., Zhou, X., Jin, S., Lu, Y., Pan, J., Lin, Q., ... & Pan, J. (2022). A comparison of XGBoost, random forest, and nomograph for the prediction of disease severity in patients with COVID-19 pneumonia: implications of cytokine and immune cell profile. Frontiers in Cellular and Infection Microbiology, 12, 819267.
Office of Surveillance, Epidemiology and Laboratory Services (n.d.). Behavioral Risk Factor Surveillance System [Fact Sheet].  CDC. https://www.cdc.gov/brfss/factsheets/pdf/brfss-history.pdf
Rajendra, P., & Latifi, S. (2021). Prediction of diabetes using logistic regression and ensemble techniques. Computer Methods and Programs in Biomedicine Update, 1, 100032.
Ranstam, J., & Cook, J. A. (2018). LASSO regression. Journal of British Surgery, 105(10), 1348-1348.
U.S. Department of Health and Human Services. (2024). Drinking levels and patterns defined. National Institute on Alcohol Abuse and Alcoholism. https://www.niaaa.nih.gov/alcohol-health/overview-alcohol-consumption/moderate-binge-drinking#:~:text=The%20Substance%20Abuse%20and%20Mental,or%20within%20a%20couple%20of 
World Health Organization. (2023, April 5). Diabetes. World Health Organization. https://www.who.int/news-room/fact-sheets/detail/diabetes.
