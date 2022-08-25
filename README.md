# GWU_DNSC6301_Project_Group18
## Credit Line Increase Model Card
#### Basic Information
* **People developing model**: 
 Varsha Katam "varsha.katam@gwmail.gwu.edu" ; 
 Ta Wei Lo "twlo@gwmail.gwu.edu" ; 
 Pranita Shetty "pranita1096@gwmail.gwu.edu"
* **Model date**: August, 2022
* **Model version**: 1.0
* **License**: MIT
* **Model implementation code**: [DNSC6301_Project_Group18.ipynb](https://github.com/katamvarsha/GWU_DNSC6301_Project_Group18/blob/6f4d1389302d7cbec5b71e9f46090f638b966d26/DNSC6301_Project_Group18.ipynb)
#### Intended Use
* **Primary intended users**: This model is an *example* probability of default classifier, with an *example* use case for determining eligibility for a credit line increase.
* **Primary intended users**: Students in GWU DNSC 6301 bootcamp.
* **Out-of-scope use cases**: Any use beyond an educational example is out-of-scope.

#### Training Data

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**ID**| ID | int | unique row indentifier |
| **LIMIT_BAL** | input | float | amount of previously awarded credit |
| **SEX** | demographic information | int | 1 = male; 2 = female
| **RACE** | demographic information | int | 1 = hispanic; 2 = black; 3 = white; 4 = asian |
| **EDUCATION** | demographic information | int | 1 = graduate school; 2 = university; 3 = high school; 4 = others |
| **MARRIAGE** | demographic information | int | 1 = married; 2 = single; 3 = others |
| **AGE** | demographic information | int | age in years |
| **PAY_0, PAY_2 - PAY_6** | inputs | int | history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above |
| **BILL_AMT1 - BILL_AMT6** | inputs | float | amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005 |
| **PAY_AMT1 - PAY_AMT6** | inputs | float | amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005 |
| **DELINQ_NEXT**| target | int | whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time |

* **Source of training data**: GWU Blackboard, Patrick Hall jphall@email.gwu.edu
* **How training data was divided into training and validation data**: 50% training, 25% validation, 25% test
* **Number of rows in training and validation data**:
  * Training rows: 15,000
  * Validation rows: 7,500

### Test Data
* **Source of test data**: GWU Blackboard, Patrick Hall jphall@email.gwu.edu
* **Number of rows in test data**: 7,500
* **State any differences in columns between training and test data**: None
### Model details
* **Columns used as inputs in the final model**: 'LIMIT_BAL',
       'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1',
       'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6',
       'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'
* **Column(s) used as target(s) in the final model**: 'DELINQ_NEXT'
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: 0.22.2.post1
* **Hyperparameters or other settings of your model**: 
```
DecisionTreeClassifier(ccp_alpha=0.0, class_weight=None, criterion='gini',
                       max_depth=6, max_features=None, max_leaf_nodes=None,
                       min_impurity_decrease=0.0, min_impurity_split=None,
                       min_samples_leaf=1, min_samples_split=2,
                       min_weight_fraction_leaf=0.0, presort='deprecated',
                       random_state=12345, splitter='best')`
```


### QUANTATIVE ANALYSIS AND MODEL WORK FLOW

### **1.	Load and analyze data**

The credit line increase data was loaded using google collab file upload. Basic data analysis was performed to identify as shape of data, get column names, find missing values, and generate descriptive statistics. Pearson correlation matrix was calculated to find pairwise correlation of the columns in data. All columns in the data are visually represented as histograms. A correlation heatmap figure is generated to represent the correlation matrix.

#### Correlation Heatmap
![Heat Map](https://user-images.githubusercontent.com/111590512/185942386-95eece6c-45d5-483b-a582-e665e5cfa083.png)

### **2.	Train a decision tree model**

The data is partitioned into training, validation, and test sets (50%, 25%, 25% respectively) to accurately evaluate the model.Testing data which is a separate set of data to test the model after training helps us determine how the model will perform in the real world. We train 12 different models using decision trees and calculate the ROC AUC for each model. Plot tree depth vs training and validation AUC.

**AUC ROC**

The AUC ( Area under the curve) ROC ( Receiver operating curve) curve assists in peformance measurement and is one of the most important evaluation metrics for checking any classification model's performance. In simple terms, the higher the AUC the better the model is at predicting between whether an individual would default on payment or pay on time.

![Iteration Plot - 1](https://user-images.githubusercontent.com/111590512/186211589-bea9419e-0285-4fd0-8ab4-b4d28db0d2c3.png)

Viewing the results as a table, we can identify that maximum validation AUC is at depth 6. Next, we plot variable importance.

![Variable importance](https://user-images.githubusercontent.com/111590512/186531478-9e5afb03-3f42-489f-96d6-3513abf4abc2.png)

Lastly, we calculate test AUC. 

|    Data Type     |   AUC    | 
| ---------------- | ---------| 
| Training Data    | 0.7837   |                           
| Validation Data  | 0.7496	  |                        
| Test Data        | 0.7438   |        


### **3.	Test the model for discrimination**

According to the article, A Combinatorial Approach to Fairness Testing of Machine Learning Models, machine learning models could exhibit biased behavior, or algorithmic discrimination, resulting in unfair or discriminatory outcomes. It is importance to not only consider the performance of your model but also other factors such as fairness and security. 

**Adverse Impact Ratio (AIR)**
The adverse impact ratio computes the negative effect a biased selection process has on protected groups. This is computed by dividing the protected group acceptance rate / controlled group acceptance rate.

AIR is associated with the convenient 4/5ths, or 0.8, cutoff threshold. AIR values below 0.8 can be considered evidence of illegal discrimination in many lending or employment scenarios in the U.S.

**Racial bias**

The protected groups for racial bias testing are hispanic, black and asian and the reference group is white. From our confusion matrices and AIR calculation, the 
hispanic-to-white AIR is below the benchmark of 0.8. Although, the black-to-white impact ratio is greater than 0.80, there is room for improvement. According to the Office of Federal Contract Compliance Programs, the agency will require strong additional supporting evidence when the impact ratio is between 0.8 and 0.9
The white-to-asian AIR of 1.0 is favorable. 

![White vs Hispanic1024_1](https://user-images.githubusercontent.com/111590512/186423198-297eacc1-f9dc-4728-a144-7f66d09321ac.jpg)

![White vs Black1024_1](https://user-images.githubusercontent.com/111590512/186423677-dbd98e73-3afd-4bb3-8d9e-e42196c80653.jpg)

![White vs Asian1024_1](https://user-images.githubusercontent.com/111590512/186423696-13a3cbff-3501-4e13-9312-b00414798f3b.jpg)

**Gender bias**

The protected group for gender bias testing is female and reference group is male. The resulting AIR value is favourable for women as it exceeds the best case scenario by 0.6. This indicates that a higher number of females were awarded a loan as compared to males. The magnitude of the marginal effect is also relatively small, another sign that discrimination with respect to gender is low under the model.

![Male vs Femal-1](https://user-images.githubusercontent.com/111590512/186424070-ddc103f8-0ba1-4f75-a7be-493e52350635.jpg)


### **4.	Remediate discovered discrimination**

According to an article by Pew Research Center, people who belong to Black and Hispanic racial groups face more difficulty in getting approved for home loans as compared to White and Asian people. In 2015, 27.4% of black applicants and 19.2% of Hispanic applicants were denied mortgages, compared with about 11% of white and Asian applicants which can be also observed in our initial model. 

Biased behavior of ML models has adverse effects on society. With our initial probability cut off 0.15, the Hispanic-to-white AIR falls below the minimum acceptable value of 0.80 and the black-to-white AIR is just over 0.80 by 0.02.

With our information, we can try to remediate biases by recalculating AIR and confusion matrices by pushing the cutoff from 0.15 to 0.18. Next, we redo the model search by training decision trees with validation-based early stopping. Instead of picking the best model defined by AUC, we are going to try it across 12 different models and observe both performance and fairness indicators to pick a balance between the two.
The below tables show how AIR values specifically for Hispanic-to-white and black-to-white are positively impacted after pushing the cutoff to 0.18

<img src ="https://user-images.githubusercontent.com/111590512/186556387-b9303204-196f-4491-b58e-a5148578ca60.jpg" width="250" height="150">

The below tables indicate the the final values, of the metrics for all data: training, validation, and test data

|    **Data Type** |   **AUC**| 
| ---------------- | ---------| 
| Training Data    | 0.7837   |                           
| Validation Data  | 0.7496	  |                        
| Test Data        | 0.7438   |      

|  **Gender group**  |  **AIR (0.15 cutoff)** | **AIR (0.18 cutoff)**|
| -------------------| ---------------------  | -------------------- |
| White vs. Hispanic | 0.76                   | 0.83                 |     
| White vs. Black    | 0.82  	                | 0.85                 |  
| White vs. Asian    | 1.00                   | 1.00                 |  

![Iteration plot -2](https://user-images.githubusercontent.com/111590512/186219402-b5cc922a-5b68-425b-9825-4767311b22bc.png)

### ETHICAL CONSIDERATIONS

**1. Negative impacts of using the model:**

*Math or Software problems*



*Real world risks*





**2. Potential uncertainities relating to the impacts of using the model:**

*Math or Software problems*



*Real world risks*

While the results of the data has provided with insightful understanding of credit increase and related biases in testing, it is important to note that there are some real world risks which have not been factored into the working. One such example would be that of security and privacy. In today's world data protection is one of the top priorities for any dataset that is being used for numerous purposes. This particular dataset is dense in information and contains confidential details such as demographic information, bill payments and loan balance payments. 

**3. Unexpected results:**


#### Sources:
AUC understanding: https://towardsdatascience.com/understanding-auc-roc-curve-68b2303cc9c5

AIR understanding: https://nbviewer.org/github/jphall663/interpretable_machine_learning_with_python/blob/master/dia.ipynb

Peew Research Center: https://www.pewresearch.org/fact-tank/2017/01/10/blacks-and-hispanics-face-extra-challenges-in-getting-home-loans/

Sex discrimination AIR interpretation: https://nbviewer.org/github/jphall663/interpretable_machine_learning_with_python/blob/master/dia.ipynb

A Combinatorial Approach to Fairness Testing of
Machine Learning Models - https://csrc.nist.gov/csrc/media/Projects/automated-combinatorial-testing-for-software/documents/preprint-iwct-22-fairness.pdf

Office of Federal Contract Compliance Programs https://www.dol.gov/agencies/ofccp/faqs/practical-significance
