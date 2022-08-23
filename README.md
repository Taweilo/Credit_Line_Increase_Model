# GWU_DNSC6301_Project_Group18
## Credit Line Increase Model Card
#### Basic Information
* **Person or organization developing model**: Varsha Katam "varsha.katam@gwmail.gwu.edu" ; Ta Wei Lo "twlo@gwmail.gwu.edu" ; Pranita Shetty "pranita1096@gwmail.gwu.edu"
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


### Quantitative Analysis

#### Correlation Heatmap
![Heat Map](https://user-images.githubusercontent.com/111590512/185942386-95eece6c-45d5-483b-a582-e665e5cfa083.png)

### Metrics of discrimination:

##### 1. AUC ROC:
The AUC ( Area under the curve) ROC ( Receiver operating curve) curve assists in peformance measurement and is one of the most important evaluation metrics for checking any classification model's performance. In simple terms, the higher the AUC the better the model is at predicting between whether an individual would default on payment or pay on time.


![Iteration Plot - 1](https://user-images.githubusercontent.com/111590512/186211589-bea9419e-0285-4fd0-8ab4-b4d28db0d2c3.png)

|    Data Type     |   AUC    | 
| ---------------- | ---------| 
| Training Data    | 0.7837   |                           
| Validation Data  | 0.7496	  |                        
| Test Data        | 0.7438   |        



##### 2. Adverse Impact Ratio (AIR):
The adverse impact ratio computes the negative effect a biased selection process has on protected groups. This is computed by dividing the protected group acceptance rate / controlled group acceptance rate.

![Iteration plot -2](https://user-images.githubusercontent.com/111590512/186219402-b5cc922a-5b68-425b-9825-4767311b22bc.png)

AIR is associated with the convenient 4/5ths, or 0.8, cutoff threshold. AIR values below 0.8 can be considered evidence of illegal discrimination in many lending or employment scenarios in the U.S.

For the dataset that has been trained, validated and tested, discrimination tests have been performed for sex and race. The protected groups for race are hispanic, black and asian and reference group is white. As per the workings, it is understood that the AIR percentage is dangerously below the accepted level of 0.8 for white vs. hispanic, where as AIR ratios for the other race groups is within acceptable level of 0.8 to 1.

![White vs Hispanic1024_1](https://user-images.githubusercontent.com/111590512/186240311-a5132e05-4689-4ceb-8ce0-0da23cdc4acc.jpg)

![White vs Asian1024_1](https://user-images.githubusercontent.com/111590512/186240313-857701f1-c813-4162-880c-0d5ed2b79b6c.jpg)

![White vs Black1024_1](https://user-images.githubusercontent.com/111590512/186240314-081b92c3-2423-466f-9e3b-ed6d02aeefc2.jpg)

The protected group for discrimination testing sex is female and reference group is male. The AIR ratio has provided favourable results females. 

![Male vs Femal-1](https://user-images.githubusercontent.com/111590512/186240768-2ed0fd0b-0ba0-4f15-b829-d3f19d88f4ec.jpg)

### 3.Bias Remediation

In the above workings for AIR, accuracy was tested with a 0.15 probability cutoff which led to white vs. hispanic ratio to be lower than 0.8. To improve the AIR ratio the probability cutoff was increased to 0.18, which improved the AIR for the required groups.

White proportion accepted: 0.735

Hispanic proportion accepted: 0.613

**Hispanic-to-White AIR: 0.83**

White proportion accepted: 0.735

Black proportion accepted: 0.626

**Black-to-White AIR: 0.85**

White proportion accepted: 0.735

Asian proportion accepted: 0.739

**Asian-to-White AIR: 1.00**



#### SOURCE:
AUC understanding: https://towardsdatascience.com/understanding-auc-roc-curve-68b2303cc9c5

AIR understanding: https://nbviewer.org/github/jphall663/interpretable_machine_learning_with_python/blob/master/dia.ipynb

