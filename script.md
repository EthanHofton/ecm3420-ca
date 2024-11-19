# Slide 1 - Introduction

Can device accelerometer data be used to detect the type of activity a person is doing?

In this presentation, we will present this problem as a multi-class classification problem and employ a series of machine learning techniques to answer this question.

But first, why should we care about this problem?

This problem is applicable to a diverse set of applications, For example:

- Health and fitness monitoring

We can already see similar applications in the market, such as Apple Watch, fitbit, garmins and many others. These devices have on board accelermoeters which are used to track the user's activity and provide feedback on their health and fitness. Having a model to interpret this data can be very useful for retrospective analysis of the user's activity and health.

- Safety monitoring

This problem can also be used to monitor the safety of a person. For example, the model can be used to detect if a person has taken a hard fall or was in a car crash. The device can then be programmed to automatically call for help if the user is unresponsive. This can be very useful for elderly people who live alone.

- Environmental monitoring

A model can be used to detect if a person is walking, running or driing. This data can be used to monitor the environmental impact of the users activity. Data like this is especially important in today's world where we are trying to reduce our carbon footprint.

# Slide 2 - The Dataset

The dataset we will be using for this project is the motion sense dataset. The data was origonally collected by researchers of the Queen mary university of London. They got 24 participants to perform 6 different activities, walking, sitting, standing, jogging, walking upstairs and walking downstairs. They contucted 15 different trials for each activity. There wehere 3 trials for each activity for each participant. The trials were split into short trials and long trials, 1-9 and 10-15 respecivally. The data was recorded using the crowdesnse app on the Iphone 6.

The data was recorded at 50hz. The accelerometer data was split into the gravity acceleration and user acceliration in the x, y and z directions. The gyroscope data was split into the rotation rate. The attitude (pitch, roll and yaw) was also recorted. The data was recorded into a csv file, one for each trial, activity and participant pair, and is time series data.

# slide 2 - the datasetr

We will use the motion sense data set - a queen mary university of london project. They had 24 people do 6 activities (walking, upstairs, downstaris, jogging, sitting and standing), with 15 trials each , using an iphone 6 and the crowdesnse app at 50Hz. This recorded the accelerometer data split into gravity and user acceliration), gryrospope data and attitude data, all stored in CSV files. The trials were split into short (1-9) and long (10-15) trials. and its all time series data. This creates a data set with 12 features and 6 classes.

# Slide 3 - Machine Learning Methods

This project can be approached as a multi-class classification problem, where there are 6 classes. This will be solved using supervised learning techniques.

In order to ensure the best results from the supervised learning models, we will use PCA in order to analyse the data and reduce the dimensionality. This will help simplify the data, so the models can be trained more efficiently.

We will also use cross validation to ensure that the models are not overfitting the data. This will help us ensure that the models are generalising well to unseen data.

# Slide 4 - Data Preprocessing

The data must first be intergrated as currently it is a big collection of csv files. All the csv files are concattonated together and the activity labels are also added to the data. The data is allready clean so no cleaning is required.

The data can then be split into test train sets. The short trials will be used for testing and the long trials will be used for training. For nerual network based models, the training data will be split into training and validation sets (80/20 split).

Some feature engineering is also required. The user acceliration and gravity acceliration will be combined to get the total acceliration. As well as this, the norm of the total acceliration will be calculated.

# Slide 5 - Dimensionality Reduction

Is combinig the user acceliration and gravity acceliration a good idea?

On the original data set of 12 features, Using PCA, we can see that the total explained variance of the first 10 principal components is 100%. This means that 10 features can be used to represent the data, making 2 features redundant.

If we use PCA on the dataset with the combined acceliration and drop the user and gravity acceliration (leaving 10 total features). We can see that the total explained variance of the first 10 principal components is 100%. This means that all 10 features are important and none should be removed.

From this, we can see that combining the user acceliration and gravity acceliration is a good idea.

# Slide 6 - Models

We will use a serieis of models to solve this problem and see how the models compare agains eachother. The models we will use are:

Decition trees, random forest, neural networks, CNN, RNN and Long Short Term Memory

# slide 7 - early model archiqecture

The decition tree have no hyperparamiters to tune, the random forest will use 10 estimators and the neural network will have a 10 neruon input layer, 64 neuron hidden layer, 32 neuron hidden layer and 6 neuron ouputlayer with softmax activation

# slide 8 early model results

As an early set of tests, I fit the models to the initial data. As we can see from the results, the models performed with a 70-80% accuracy. This is ok, but can be improved. If we have a look at each individual catagory, the models performed very well for the sitting and standing activities, but not very well for the others. But why is this?

None of the models take into account the time series nature of the data. activities like standing and sitting are stationary and do not need the additional time series information to be classified. However activities like walking and jogging are not stationary and required the time series information in order to be classified.

How can we encode the time series information into our models?

# slide 9: Time series data

In order to add this information to our models we can use 2 methods:

Windowing slides a window of a fixed size over the data and uses the data in the window to make a prediction. The data is a 10 by window size matrix, meaning the data is 2d and must be used by a model that can interpret 2d data.

The other method is rolling features. Using this method, for each feature a rolling mean, standard deviation and median feature is calculated. This means that data can be used by a model that can iterperate 1d data.

Windowing can be more accurate but requires the models to have many more parameters. Rolling features is less accurate but requires less parameters, makiung it more sutable for smaller models.

# slide 10: rolling feature results

using the rolling features dataset shows a massive increse for all models performing at arround 90% accuracy.

# slide 11: comparison

The graph shows the accuracy for each catagory for each model, with the dotted line showing the accuracy of the early models.

The accuracy for the sitting and standing catagories have not changed much, but the accuracy for the other catagories have increased massively.

# slide 12: confusion matrix

This can be further illistrated by comparing the confusion matrix of the early models and the rolling feature models.

The early models misclassified walking for jogging, walking upstairs and walking downstairs. This is because the early models did not have access to the time series information which would reveal where the person was going next. The rolling feature models did have access to this information and were able to classify the activities correctly most of the time.

# slide 13: CNN

In order to using the windowing data, we will use a CNN with the following architecture:

The LSTM and RNN models will have a similar architecture, but will use LSTM and RNN layers instead of CNN layers.

Since this is a more complex model, the L2 regularlisers will be used along side dropout layers to prevent overfitting the data

# slide 14: CNN results

As we can see from the results, The windowing based models preformed very well with a minimum accuracy of 94%. This is a massive improvment. However, there is still room for improvment.

# slide 15: window size

The size of the sliding window can be changed to give more or less information to the model. Here, the results can be seen for different window sizes.

As we can see, the accuracy increases as the window size increases. This is because the model has more information to make a prediction. However, the model also has more parameters, meaning it is more difficult to train and will take longer to predict results.

# slide 16: window size confusion matrix

The confusion matrix for the 2 second window and 10 second window can be seen here:

As we can see, the 10 second window had the biggest trouble classifying walking. This could be due to overfitting the training data or there not being enough varience in the training data set. The 2 second window has biggest trouble classifying walking downstairs, this could be due to the fact that there is not enogh information in the window to classify the activity.

# slide 17: rolling feature vs windowing

The confution matrix for the two best performing models from each method can be seen here:

As we can see, the rolling features method miss classifed walking and jogging for going upstairs and downstairs. This could be due to the fact that the rolling feature data for these activities could look too simiar. The windowing method has this problem, but not to the same extent as the rolling feature method.

# slidce 18: results overview

The best rolling feature model achived an accuracy of 96.3, while the best windowing model achived an accuracy of 97. This is a massive improvment over the early models.

The CNN training time was significantly longer than the other models, taking 10 times longer than the rolling feature models. This is due to the fact that the CNN has many more parameters than the other models.

# slide 20: Limitations

The dataset used in this project posed some limitaitions. It was not very diverse meaning that the models may not generalize well to new data by different test subjects. The data set was relativley small, meaning the models may suffer from overfitting. As well as this, the data is not real world data and was recorded in a controlled environment where only one activity was preformed at a time. This is not the case in the real world, where people may rapidly switch between activities.

The models used in this project also posed some limitations. The RNN model suffers from the vanishing gradient problem, meaning they are not suited for long term time series data. The windowing models also have a very high computational cost due to their large number of parameters. As well as this, the NN models are not very interpretable, making it difficult to understand how and why the model is making its predictions.

# slide 19: Conclusiding remarks

In conclusion, we have shown that device accelirometer data can be used to detect the type of activity a person is doing. We have also shown that the time series informaiton is very important for accuratly classifyign the data. We have shown that the windowing method can be more acurate than the rolling features method, but requires more parameters and takes longer to train. This could mean the rolling features method is more sutable for applications where computational power is limited, for example on a smart watch.

Since the windowing data is more complex, it also allows more complex models to be used without overfitting the data. This means there is also room for imporovement on the architechure of the windowing based models.
