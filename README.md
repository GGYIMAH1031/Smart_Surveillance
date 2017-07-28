# Smart_Surveillance
An intelligent video surveillance system that flags suspicious activities.



THE PROBLEM

The current video surveillance technology is limited by the inability of human operators to focus on live footage continuously and effectively. Typically, operators viewing a single monitor lose 95% of their focus and discernment after twenty minutes of concentration. These days, with most surveillance systems having a few dozen cameras, the surveillance task is clearly beyond human ability. Therefore, this project is aimed at providing a smart system which will improve video surveillance efficiency by automatically flagging anomalous human behavior; thereby drawing the operator’s attention to suspicious activities.  



THE DATA

The problem is posed as a classification problem where the goal is to identify normal and anomalous human behavior in public settings. The input data (videos) have both spatial and temporal features. Videos from the following three datasets were combined and re-categorized into normal and anomalous behavior:-

- Human Motion Database (HMDB51)
(http://serre-lab.clps.brown.edu/wp-content/uploads/2013/10/hmdb51_org.rar)


- Violent Scenes Videos (http://vsddownloads.technicolor.com/VSD2014_officialrelease.zip)


- CAVIAR Test Scenarios
(http://homepages.inf.ed.ac.uk/rbf/CAVIARDATA1/)

 
For the sake of this project, “anomalous” behavior is defined as any human behavior which is unusual in public or crowd settings. Only the CAVIAR dataset was used for the base model but all three datasets will be used in the complete model.




THE MODEL

A two-step deep learning approach is used to address the problem. 

Step 1: The first stage of the project focuses on classifying spatial features (images) in the videos. 
Traditionally, Deep Convolutional Neural Networks (CNN) have been known to perform well for image classification. 
Therefore, a simple video classifier model is retrained on the dataset using Google’s Inception-v3 Deep Convolutional Neural Network (CNN) architecture. 

Step 2: The second stage focuses on improving the base model using the temporal features in videos. Image sequences will be created from image features in the final pooling layer of the CNN model and fed into a Long-Short Term Memory (LSTM) layer, followed by two fully-connected layers.   



RE-TRAINING THE MODEL

To re-train the inception model for normal / anomalous behavior data, follow the following steps:

1. In a linux terminal, install git and tensorflow. Other requirements are in the requirements.txt file.

        apt-get install git

2. Download the smart surveillance repo

        git clone git:https://github.com/GGYIMAH1031/Smart_Surveillance.git
        

3. Download the dataset and inception model from Google Drive into the repo folder.

        https://drive.google.com/drive/folders/0B6xlvliuTL1_UVhFWVhPdzNkY28?usp=sharing
        

4. In the linux terminal, launch tensorboard to visualize the training:

        tensorboard --logdir training_summaries &
        

   If tensorboard is already launched, stop it and restart:
   
        pkill -f "tensorboard"
        tensorboard --logdir training_summaries &
        

5. Begin model training:

         python retrain.py \
          --bottleneck_dir=bottlenecks \
          --how_many_training_steps=(e.g. 1000) \
          --model_dir=inception \
          --summaries_dir=(e.g. training_summaries/stage1) \
          --output_graph=(e.g. retrained_graph_stage1.pb) \
          --output_labels=(e.g. trained_labels_stage1.txt) \
          --image_dir=(i.e. the root directory for all train images)
