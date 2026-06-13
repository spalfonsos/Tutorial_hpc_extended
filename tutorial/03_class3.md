# In this session we are going to compare the results of using multicore vs job array for hyperparameter tuning.

lets make a folder for this class (class3) and subfolders (data, multicore and jobarrays)

- Download the data: we will use a credit card default dataset. For further information about the data, refer to: https://archive.ics.uci.edu/ml/datasets/default+of+credit+card+clients. 
  **Variable explanations:**

**ID:** ID of each client.

**LIMIT_BAL:** Amount of given credit in NT dollars (includes individual and family/supplementary credit).

**SEX:** Gender reported to the company (1=male, 2=female).

**EDUCATION:** (1=graduate school, 2=university, 3=high school, 4=others, 5=unknown, 6=unknown).

**MARRIAGE:** Marital status (1=married, 2=single, 3=others).

**AGE:** Age in years.

**PAY_0:** Repayment status in September, 2005 (-1=pay duly, 1=payment delay for one month, 2=payment delay for two months, … 8=payment delay for eight months, 9=payment delay for nine months and above).

**PAY_2:** Repayment status in August, 2005 (scale same as above).

**PAY_3:** Repayment status in July, 2005 (scale same as above).

**PAY_4:** Repayment status in June, 2005 (scale same as above).

**PAY_5:** Repayment status in May, 2005 (scale same as above).

**PAY_6:** Repayment status in April, 2005 (scale same as above).

**BILL_AMT1:** Amount of bill statement in September, 2005 (NT dollar).

**BILL_AMT2:** Amount of bill statement in August, 2005 (NT dollar).

**BILL_AMT3:** Amount of bill statement in July, 2005 (NT dollar).

**BILL_AMT4:** Amount of bill statement in June, 2005 (NT dollar).

**BILL_AMT5:** Amount of bill statement in May, 2005 (NT dollar).

**BILL_AMT6:** Amount of bill statement in April, 2005 (NT dollar).

**PAY_AMT1:** Amount of previous payment in September, 2005 (NT dollar).

**PAY_AMT2:** Amount of previous payment in August, 2005 (NT dollar).

**PAY_AMT3:** Amount of previous payment in July, 2005 (NT dollar).

**PAY_AMT4:** Amount of previous payment in June, 2005 (NT dollar).

**PAY_AMT5:** Amount of previous payment in May, 2005 (NT dollar).

**PAY_AMT6:** Amount of previous payment in April, 2005 (NT dollar).

**default.payment.next.month:** Default payment (1=yes, 0=no).

  gdown https://drive.google.com/uc?id=1sqKxJeJhLA2BaEVnCqXcLWZ5oSfyVP56
- In the folder multicore
  save the folowing script

- In the folder jobarrays
  save the following script
  

In our environment lets install xgboost and shap libraries having activated source bcpt_env.sh

pip install --no-index xgboost shap

- Multicore results
  AUC
  figure
  SHAP importance
  We can see the Pay variables are the most important in the SHAP model, the Amount of Credit (Card Limit, LIM_BAL) appears as a significantly important variable in this case. The limit balance causes a bigger shift than the other variables, which is what SHAP is looking for.
  example 1
  

