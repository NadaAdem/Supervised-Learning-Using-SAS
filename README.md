# Supervised-Learning-Using-SAS  breast-cancer-dataset.csv.


## Download the breast-cancer-dataset.csv. Complete the following tasks 
## Read the file in SAS and display the contents using the import and print procedures. 
'''
proc import
/* out keyword is used to name a table*/
out= breastcancer 
/* Datafile keyword takes the path of the file from the hard disk*/
datafile ="breast_cancer_dataset.csv"
/* dbms= csv replace is telling SAS it is a csv file. */
dbms=csv replace; 
/* Getnames=yes will use first line of the csv file as column names*/
getnames=yes;
/* data keyword takes the name of the SAS table imported as auto_csv.
 print keyword outputs the contents in Results Viewer */
proc print data= breastcancer (obs=10);
title "Breast Cancer Dataset";
run;
'''
## Develop a decision tree-based classification model using the hpsplit procedure of SAS. (2 points) 


proc import
/* out keyword is used to name a table*/
out= breastcancer 
/* Datafile keyword takes the path of the file from the hard disk*/
datafile ="breast_cancer_dataset.csv"
/* dbms= csv replace is telling SAS it is a csv file. */
dbms=csv replace; 
/* Getnames=yes will use first line of the csv file as column names*/
getnames=yes;
/* data keyword takes the name of the SAS table imported as auto_csv.
 print keyword outputs the contents in Results Viewer */
proc print data= breastcancer (obs=10);
title "Breast Cancer Dataset";
run;

ods graphics on;
proc hpsplit data= breastcancer;
/* categorical variable */
 class class age menopause tumor_size inv_nodes node_caps deg_malig breast breast_quad irradiat; 
 /* dependent var = 13 independent variables */
 model class = age menopause tumor_size inv_nodes node_caps deg_malig breast breast_quad irradiat; 
 grow entropy; /* specify the criterion for splitting parent nodes */
 prune costcomplexity; /* find a smaller subtree that results in a low error rate */
run;


 
## The resulting decision tree has 286 examples at the root node. Each decision node in the tree is labeled with the corresponding independent variable name and split value. The leaf nodes show the classification decision.
## 	Navigate the contents of Results View by clicking on HPSplit breast-cancer-dataset, and then by selecting Model Assessment. Examine the confusion matrix, fit statistics, and variable importance.
## Using the confusion matrix, compute the following assessment metrics accuracy, recall, and precision (see lecture for formulas and state your assumption in bold which class in the confusion matrix you want to consider positive--i.e., recurrence-event or not-recurrence-event)

 

Condition for marks: 3 points for accuracy, 1 point for precision, and 1 point for recall. 

 If we consider Not -recurrence-event is considered  as positive.

-	Accuracy = TP+TN / TP+TN +FP+FN
Accuracy = 194+23/ 194+7+62+23 
                            =217/286 
                            = 0.759


-	 Recall = TP/TP+FN
         Recall = 194/194+7
                    =194/201
                     =0.965
  
-	 Precision = TP/TP+FP
Precision=194/194+62
               = 194/256
               = 0.757
  

 If we considered recurrence-event as positive.

-	Accuracy = TP+TN / TP+TN +FP+FN
Accuracy = 23+ 194 /194+7+62+23
                = 217/286
                 = 0.759


-	 Recall = TP/TP+FN
 Recall = 23/23+62
            = 23/85
          = 0.2706






-	Precision = TP/TP+FP
         Precision= 23/23+7
                     = 23/30
                      = 0.7667
                   

 I will consider recurrence-event is position, because it is important to recurrence -event have less negative (FN).  There  ML algorithm missed 62 people who has tumors.

## Change the grow algorithm to “gini” and recompute the metrics from question 2. Does entropy build a more accurate classifier or gini? (5 points) 

proc import
/* out keyword is used to name a table*/
out= breastcancer 
/* Datafile keyword takes the path of the file from the hard disk*/
datafile ="breast_cancer_dataset.csv"
/* dbms= csv replace is telling SAS it is a csv file. */
dbms=csv replace; 
/* Getnames=yes will use first line of the csv file as column names*/
getnames=yes;
/* data keyword takes the name of the SAS table imported as auto_csv.
 print keyword outputs the contents in Results Viewer */
proc print data= breastcancer (obs=10);
title "Breast Cancer Dataset using grow GINI";
run;


ods graphics on;
proc hpsplit data= breastcancer;
/* categorical variable */
 class class age menopause tumor_size inv_nodes node_caps deg_malig breast breast_quad irradiat; 
 /* dependent var = 13 independent variables */
 model class = age menopause tumor_size inv_nodes node_caps deg_malig breast breast_quad irradiat; 
 grow gini; /* specify the criterion for splitting parent nodes */
 prune costcomplexity; /* find a smaller subtree that results in a low error rate */
run;


 For entropy   Accuracy = TP+TN / TP+TN +FP+FN
    Accuracy = 194+23/ 194+7+62+23 
                            =217/286 
                            = 0.759


For Gini  Accuracy = TP+TN / TP+TN +FP+FN
    Accuracy = 191+27/ 191+10+58+27 
                             =218 /286
                              = 0.762 

As result, Gini was more  accuracy than entropy and also it give us high Ture  recurrence-event .
                             

 
