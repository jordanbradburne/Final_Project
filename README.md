# Final_Project

## Selected topic: 
* Modeling human activity from smartphone data

### Link to google presentation can be found below:
https://docs.google.com/presentation/d/1tUjPQUAhGOVnQN3aab45WwdwhtgZQFT5DuT2ti0-Qps/edit?usp=sharing

### Link to Outline of Dashboard:
https://docs.google.com/presentation/d/1Zd3sqP9JfdyD5-TyiXzo514swSHfqyXL1HokYcYll-M/edit?usp=sharing

## Reason why I selected my topic: 

* The reason in choosing this topic is to want to know whether or not you can predict human activity from wearble sensor data such as a fitbit.
* The reason in choosing this topic is to want to know whether or not you can predict human activity from wearable sensor data such as a fitbit.

## Dataset Source:
- NOTE: Only a pre-cleaned/pre-processed version of the dataset was available.
- https://www.kaggle.com/uciml/human-activity-recognition-with-smartphones

## Description of my source of data:

- Recordings of 30 study participants performing activities of daily living
- dataset is randomly broken into 2 sets. 70% of the volunteers were selected for generating the training data and 30% for the test data.
- A group of 30 volunteers with ages ranging from 19 to 48 years were selected for this task. 
- Each person had to follow along with a certain physical activity while wearing a waist-mounted Samsung Galaxy S II smartphone. They each had to repeated the activity twice: first with phone mounted on their left side and the second wherever the subject preferred.
- The activities were standing, sitting, laying down, walking, walking downstairs and upstairs. 

### How data is incorporated with the model:
![image](https://user-images.githubusercontent.com/85847344/139778677-728b71a9-c5f4-4d07-a77a-501031956f46.png)

## Questions I hope to answer with the data
- Whether or not you can predict what specific physical activities one is performing:
  - Can you differentiate between walking on a flat surface, walking up stairs and/or down stairs?
  - Is it possible to determine the difference between sitting, standing and laying? If so, how are these differentiated?

## Description of the data exploration phase of the project
1. Load Data

  * Working with multivariate time series data, it is useful to have the data structured in the format:
        [samples, timesteps, features]
  * Once loaded each file as a NumPy array, then combine or stack all three arrays together.
  * Use the dstack() NumPy function to ensure each array is stacked in a way that the features are separated in the third dimension
  * Use the two functions to load all data for the train and the test dataset.
  * develop a new function that loads all input and output data for a given folder
  * function builds a list of all 9 data files to load and loads them as one NumPy array with 9 features
  * next load the data file containing the output class
  
  * Results
    * loaded the train and test datasets
    * test dataset has 2,947 rows of window data.
    * the size of windows in the train and test sets match and the size of the output (y) in each the train and test case matches the number of samples

2. Balance of Activity Classes
  * good first check of data is to investigate the balance of each activity.
  * 30 subjects performed each of the six activities. Confirming this expectation will both check that the data is balanced, making it easier to model, and confirm that the dataset is being loaded and interpreted properly.
  * will develop a function that summarizes the breakdown of the output variables
  * wrapp the provided NumPy array in a DataFrame, group the rows by the class value, and calculate the size of each group (number of rows).
  * results are summarized, including the count and the percentage.

  * Results
    * it summarizes the breakdown for the training set.
    * similar distribution of each class hovering between 13% and 19% of the dataset.
    * result on the test set and on both datasets together look very similar.
    * likely safe to work with the dataset assuming distribution of classes is balanced per train and test set

3. Plot Time Series Data for One Subject
  * time series data
  * an import check: create a line plot of the raw data.
  * The raw data is comprised of windows of time series data per variable, and the windows do have a 50% overlap.
  * may see repetition in the observations as a line plot unless the overlap is removed.

<img width="490" alt="Screen Shot 2021-11-15 at 12 01 54 AM" src="https://user-images.githubusercontent.com/85847344/141744111-6cc85a2e-1992-449b-b458-d510903bb2dc.png">

  * First
    * Retrieve all of the rows for a single subject
    * take the loaded training data, the loaded mapping of row number to subjects, and the subject identification number for the subject interested in, and return the X and y data for only that subject.

  * Second
    * data for one subject plot it.
    * write a function to remove data overlap and shrink windows down for given variable into one long sequence which can be plotted directly as a line plot.

  * Final
    * plot each of the nine variables for the subject in turn
    * a final plot for the activity level.
    * Each series will have the same number of time steps (length of x-axis)

  * Results
    * periods of large movement corresponding with activities 1, 2, and 3: the walking activities
    * much less activity (i.e. relatively straight line) for higher numbered activities, 4, 5, and 6 (sitting, standing, and laying).
    * good confirmation that the dataset has been correctly loaded and interpreted.
    * see a lot of commonality across the nine variables so very likely that only a subset of these traces are needed to develop a predictive model.
    * will find other subjects show similar behavior given that they performed the same actions.
 
4. Plot Histograms Per Subject

  * regularity in movement data across subjects
  * data has been scaled between -1 and 1, per subject, so the amplitude of detected movements will be similar.
  * check for similarity of behavior across subjects by plotting and comparing the histograms of movement data across subjects.
  * create one plot per subject and plot all three axis of a given data, then repeat this for multiple subjects.

<img width="314" alt="Screen Shot 2021-11-15 at 12 02 43 AM" src="https://user-images.githubusercontent.com/85847344/141744228-48502aae-dc68-46ff-babc-719797d40beb.png">

  * Results
    * some of the distributions align (e.g. main groups in the middle around 0.0), so there may be some continuity of movement data across subjects
    * strong consistency across subjects could later help for the modeling
    * differences across subjects in total acceleration data may not be as helpful.

5. Plot Histograms Per Activity
  * interested in discriminating between activities based on activity data.
  * discriminate between activities for a single subject.
  * review the distribution of movement data for a subject by activity.
  * create a histogram plot per activity, with the three axis of a given data type on each plot.
  * expect to see differences in the distributions across activities down the plots.
  
  <img width="315" alt="Screen Shot 2021-11-15 at 12 03 19 AM" src="https://user-images.githubusercontent.com/85847344/141744313-6978bb4e-5039-4cbc-be4c-878c71be2eb5.png">

  * Results
    * each activity has a different data distribution, with a marked difference between the large movement (first three activities) with the stationary activities (last three activities).
    * Data distributions for the first three activities look Gaussian
    * Distributions for the latter activities look multi-modal (i.e. multiple peaks).
    * creates plots with the similar pattern as the body acceleration data, although shows perhaps fat-tailed Gaussian-like distributions instead of bimodal
    * distributions for the in-motion activities.
    * would expect to see similar distributions and relationships for the movement data across activities for other subjects.
 
6. Plot Activity Duration Boxplots
  * final area to consider is how long a subject spends on each activity.
  * related to the balance of classes.
  * If activities (classes) are balanced within dataset, then expect balance of activities for a given subject over course of their trace would be reasonably well balanced.
  * confirm this by calculating how long (in samples or rows) each subject spends on each activity
  * look at distribution of durations for each activity.
  * summarize the distributions as boxplots: create a boxplot per activity of the duration measurements.

<img width="296" alt="Screen Shot 2021-11-15 at 12 03 53 AM" src="https://user-images.githubusercontent.com/85847344/141744370-31e4a8dc-223d-4dc3-b2e0-8e14505ee4b5.png">

<img width="298" alt="Screen Shot 2021-11-15 at 12 04 52 AM" src="https://user-images.githubusercontent.com/85847344/141744471-74d9e028-8948-4970-9511-76a0560ec082.png">

  * Results
    * Shows a similar relationship between activities.
    * suggests that the test and training dataset are reasonably representative of the whole dataset.

2. Balance of Activity Classes
  A good first check of the data is to investigate the balance of each activity.
3. Plot Time Series Data for One Subject
  time series data,  an import check is to create a line plot of the raw data.
4. Plot Histograms Per Subject
  There must be regularity in the movement data across subjects. The data has been scaled between -1 and 1, presumably per subject, so the amplitude of the subjects’ detected movements will be similar.
6. Plot Histograms Per Activity
  interested in discriminating between activities based on activity data.
7. Plot Activity Duration Boxplots
  A final area to consider is how long a subject spends on each activity.

## Description of the analysis phase of the project
* For the analysis phase, I utilized a confusion matrix to look at the performance of my SVM model.
* As indicated in the confusion matrix, the model was able to predict when a subject was laying with 100% accuracy (i.e. a probability of 1.00).
* The least accurate prediction was for sitting (90% i.e. 0.90).

## Machine Learning Portion
### Description of data preprocessing
* The available dataset was already pre-cleaned but still analyzed to make sure quality data was being used. (Reference "Description of the data exploration phase of the project")

### Description of feature engineering and feature selection, including decision-making process
* Preliminary feature engineering examined 6 different features: sitting, standing, laying, walking, walking up stairs and walking down stairs. These features were selected based on the activities that were recorded by the wearable and associated with that specific activity.

### Description of how data was split into training and testing sets
<img width="454" alt="Screen Shot 2021-11-14 at 10 43 48 PM" src="https://user-images.githubusercontent.com/85847344/141734613-87f9e8df-b031-4f9f-90e7-2794b9364b65.png">
* The data set was split 70% training and 30% testing based on Activity 

### Explanation of model choice, including limitations and benefits
* Support Vector Machine (SVM) was chosen because it is a good place to start when someone is not sure of the data. The advantages for this dataset also outweigh the disadvantages which was a large part in the choice of this model.
* Advantages: Works well when there is a good margin between classes, computationally efficient.
* Disadvantages: Doesn't work well with a lot of noise (i.e. when target classes overlap), or with large data sets.

### Explanation of changes in model choice 
* (if changes occurred between the Segment 2 and Segment 3 deliverables)
* N/A
* No changes made between segements 2 and 3

### Description of how the model has been trained thus far
* The data set was split 70% training and 30% testing based on Activity 
* No additional changes have been made in the training and testing strategy since Segment #2

### Description of current accuracy score
* Accuracy Score: 0.96 avg i.e. 96% accuracy on average.
<img width="503" alt="Screen Shot 2021-11-21 at 9 00 14 PM" src="https://user-images.githubusercontent.com/85847344/142803497-e08c75b2-0216-4f1f-97e8-2af7fa6e39e9.png">
