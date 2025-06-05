#Pediatric Septic Shock Prediction
This is a record of our models for predicting the risk of septic shock in the first 24 hours among children (60 days - 18 years) in whom there is clinical concern for sepsis in the emergency setting. These models are currently intended for research only and do not represent medical advice. These models are intended to estimate risk in children after a clinician has already indicated a concern for potential sepsis; they are not intended to be sniffers. They do not find patients with sepsis when clinicians are not concerned, but are intended to provide decision support when a clinician begins to worry about sepsis and is not sure how likely it is or the next steps to take.

Study Team: Halden F. Scott, Kathryn L. Colborn, Carter J. Sevick, Lalit Bajaj, Niranjan Kissoon, Sara J. Deakyne Davies, Allison Kempe
Supported by funding from AHRQ K08HS025696

1.  Our first model predicting septic shock using only data available in the Electronic Health Record at the time of hospital arrival was published in the Journal of Pediatrics, where full text of all of the methods, including population characteristics and intended clinical use, is available. This model was derived using an elastic net procedure: 

	Scott HF, Colborn KL, Sevick CJ, Bajaj, L, Kissoon N, Deakyne-Davies SJ, Kempe A. Development and Validation of a Predictive Model of the Risk of Pediatric Septic Shock Using Data Known at the Time of Hospital Arrival. Journal of Pediatrics. 2020 Feb; 217:145-151. 
	
	https://www.jpeds.com/article/S0022-3476(19)31317-4/fulltext

	This model produced an AUROC of 0.79 (0.76-0.83) in the training set, 0.75 (0.69-0.81) in the temporal test set and 0.87 (0.72-0.90) in the geographic test set, with additional characteristics described in the article.

2.	We have since updated this model. In our new procedure, we used the same datasets, but modified our statistical procedure to the following: We used the glmnet package in R with a logit link, binomial distribution and the lasso algorithm for variable reduction. Then, using only the predictors identified through the lasso procedure, we used logistic regression to estimate the coefficients of the predictors in the model. This modification was made to improve the calibration of the model.

	This resulted in AUROC of 0.79 (0.76-0.83) for the temporal training set 0.75 (0.69-0.81) and 0.85 (0.71-0.99). The calibration was improved with the new model. A full report of the performance of the model is in the attached documentation. The model is as follows:

	Final model for the prediction of septic shock among patients in whom ED clinicians were concerned for sepsis at the time of ED arrival. The model is a linear predictor that estimates the log odds of septic shock, using the sum of the intercept and the predictors multiplied by their coefficient. To transform the odds of septic shock to a probability, use the transformation e^xB/(1+e^xB).

		Intercept			5.033; 
		Systolic blood pressure, mmHg	-0.0283;
		Piecewise diastolic blood pressure term:
		If >=69 mmHg, set to 0
		If <69 mmHg, set to 69-DBP	 0.0270;
		Temperature, °C			-0.1344;
		Age, years * respiratory rate, breaths per minute	
                                         	 0.0017;
		Age, years * shock index, beat per minute/mmHg	
                                          	0.0733;
		Arrival via EMS		    	0.3280;
		Oncological comorbidity		-0.9967;
		Indwelling central line present on arrival
                                        	-0.2177;
		Hospitalized in the last year	           -0.3078;
		(Shock index = HR/SBP)

3.	The PMML code of this second model (variable selection via lasso, coefficients derived via logistic regression) was developed in order to facilitate implementation and testing of the model in Electronic Health Record software. This model and its performance are described in the paper: https://pmc.ncbi.nlm.nih.gov/articles/PMC7790844/ Model perfomance was: area under the receiver operating curve in the training set of 0.85 (0.82-0.88); 0.83 [0.78-0.89] in the temporal test set; 0.83 [0.60-1.00] in the geographic test set. Sensitivity and specificity varied based on cutpoint; when sensitivity in the training set was set to 90% (83%, 94%), specificity was 62% (60%, 65%).
