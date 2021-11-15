# Final_Project

Communication Protocol: Individual project

## Selected topic: 
* Modeling human activity from smartphone data

### Link to google presentation can be found below:
https://docs.google.com/presentation/d/1tUjPQUAhGOVnQN3aab45WwdwhtgZQFT5DuT2ti0-Qps/edit?usp=sharing

## Reason why I selected my topic: 
* The reason in choosing this topic is to want to know whether or not you can predict human activity from wearble sensor data such as a fitbit.

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
