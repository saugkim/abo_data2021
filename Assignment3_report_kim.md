## Mini-project 3 REPORT part

Kim Yukyeong    
16.12.2021  
You can open project report using jupyter notebook or open in [my github repo](https://github.com/saugkim/abo_data2021/blob/main/Assignment3_report_kim.md)


## Introduction


*what is the problem you are solving?*  
build a predictive model where the output is `MEDV - Median value of owner-occupied homes in $1000's` using deep learning approaches.

Define problem:
   - Regression problem: single output
   - Supervised learning: predict continuous value of housing price
   - Methods: deep neuron network


Define data:
   - raw data file downloaded from https://archive.ics.uci.edu/ml/machine-learning-databases/housing
   - total number of records: 506 instances   
   - total number of columns: 14 = 13 features (1 binary + 12 continous attributes) + 1 target (MEDV)

##  Data processing 
*what are the choices you made in data processig and how you performed it?*


tools used:  
   - pandas   
   - matplotlib and seaborn

null value check: 
   - Dataset has no missing attribute values (based on the provider descriptions)  


feature selection by correlation matrix
   - features which correlation score is over threshold(0.4) with respect to the target are selected for model inputs features  
   - lowest correlation score with MEDV is CHAS(value of 0.18), highest correlation is LSTAT(0.74).
   - features selected: INDUS, NOX, RM, TAX, PTRATIO, LSTAT
   - target: MEDV

<img src="https://raw.githubusercontent.com/saugkim/abo_data2021/main/image/correlation.PNG" width="800" />

<br>

visualization of distribution of dependent variables
   - visualization using boxplot and histograms
   - there could be outliers from the boxplot result, but all records are used for training and evaluation model
   


normalization of features 
   - In the below table of statistics shows that each feature has different scale and range 
   - It is important to normalize features that use different scales and ranges, because the features are multiplied by the model weights, so the scale of the outputs and the scale of the gradients are affected by the scale of the inputs. 
   - features are scaled to have zero mean and unit variance for each feature.
   
| |mean|	std|
|-- | --| --|
|LSTAT|	12.653063|	7.141062|
|INDUS|	11.136779|	6.860353|
|RM|	6.284634|	0.702617|
|TAX|	408.237154|	168.537116|
|NOX|	0.554695|	0.115878|
|PTRATIO|	18.455534|	2.164946|


<br>


splitting training and testing dataset for training and evaluation models
  - training and testing set divided (ratio of 0.8:0.2 == 404:102)  

##  Modelling

*What is the modelling approach? How you performed it? How you interpret the ouput?*

tools used: tensorflow keras    


Model architecture is input layer and two hidden layers and output layer, with 8 and 10 neurons in each hidden layer and one output which predicts value of MEDV. Model has 157 trainable parameters and mean squared error as a loss function and Adam optimizer with learning rate of 0.005 are used in training. Start with 500 epochs, early stopping callback stopped training around 200 epochs, training loss stable at level of 10 and validation loss at level of 20.  

<br>

performance of model on test dataset 

  - evaluation metrics: Mean squared error (MSE) (tf.losses.MeanSquaredError)  
  - result of our model: `14.2`
  
  
<br>

the error level of 14 and based on the prediction vs true value in graph below, my model looks working well (linearity can be found between them)

<img src="https://raw.githubusercontent.com/saugkim/abo_data2021/main/image/result.PNG" width="400" />



##  Conclusion 
*what were the “scientific” bottlenecks? How did you overcome them?*   

scientific bottleneck was preprocessing of features, our dataset is small enough so well selected features or removing outliers could be key to have good model. I used correlation matrix to find good features to predict well target values. I have not removed any records, highly possibly few or more outliers exists, with removing them model performance could be much improved. 

another bottleneck is design network architecture, there is lots of factors can be optimized or tuned for example number of hidden layers, number of neurons, non-linearity and optimizer(and its parameters), loss functions, epochs, regularization and so on. I started with model architecture with help of tensorflow keras tutorial. My goal of this project is not finding best or perfect model, so I am satisfied with every learing step. 

