# Overview
    The goal of this project is to predict whether or not Alphabet Soup should fund a project based on historical data of over 34,000 past funding instances.  As we are given a binary measure of success on the previous projects, we are building a neural network to use to test future applicants against. 


# Results
    The best result I was able to achieve in any of the three attempts was 0.7445945739746094 on my third (submitted) workbook. Unfortunatly a hair short of our goal of .75.

    ## first notebook: Starter_Code_
    
    * Target Variable for our model was the "IS_SUCCESSFUL" column of our supplied CSV.
    * I removed the 'EIN','NAME' coumns from the data as they were extraneous.
    * Noted outliers in the 'ASK_AMT' column- version 1 of the test set I left them in.
    * Binned the 'INCOME_AMT' into two bins since +70% didnt have any data, as known or not known income
    * Binned the 'APPLICATION_TYPE' into two bins since +80% were of one type
    * Binned the 'CLASSIFICATION' into four bins
    * After dummies here, scaled data and made super small model of three layers and not a lot of units, just to see where something small with not a lot of neurons would do.
        ###Loss: 0.5894071459770203, Accuracy: 0.701924204826355
    * Dropped everything from the "ASK_AMT" ouside of the 95th percentile to remove some outliers, rescaled and ran same small simple model as above with slighhtly better results. 
        ###Loss: 0.5781846642494202, Accuracy: 0.7139700651168823
    *Using Keras_Tuner that created as many as 5 layers and maxed at 20 epochs and ran it on both sets
        * On the initial set (which included outliers)
        ### Best val_accuracy So Far: 0.7082215547561646
        * On the second set (no outliers)
        ### Best val_accuracy So Far: 0.7196170091629028

    ### (Did not export HDF5 file here because I'm going to keep working to improve performance)

    ## second notebook: AlphabetSoupCharity_Optimization
    * Goal here is to give the model more data (less bins) and more layers/neurons
    * Target Variable for our model was the "IS_SUCCESSFUL" column of our supplied CSV.
    * I removed the 'EIN','NAME' and 'STATUS' coumns from the data as they were extraneous.
    * Filtered the 99th quartile out here, just to get extreme outliers
    * Did not bin 'INCOME_AMT' 
    * Binned the 'APPLICATION_TYPE' into nine bins
    * Binned the 'CLASSIFICATION' into 12 bins
    * After dummies here, scaled data and made model of five layers with 5X the neurons as first round for 100 epochs.
        ### Loss: 0.5492253303527832, Accuracy: 0.7327129244804382
    *Using Keras_Tuner that created as many as 9 hidden layers with between 10-61 nodes maxed at 20 epochs and ran (went down in epochs because I noticed that in my above test things stabilized pretty early in the 100 adding keras.callbacks.EarlyStopping saved considerable time)
        ### Best val_accuracy So Far: 0.7409588694572449
    
    ### (Did not export HDF5 file here because I'm going to keep working to improve performance)
    
    ## third notebook: AlphabetSoupCharity_Optimization_v2
    * Target Variable for our model was the "IS_SUCCESSFUL" column of our supplied CSV.
    * I removed the 'EIN','NAME', 'SPECIAL_CONSIDERATIONS' and 'STATUS' coumns from the data as they were extraneous.
    * Additionally, I removed the 27 entries marked as "SPECIAL_CONSIDERATIONS".  Without knowing why they were labeled that it already indicates they get some sort of different status and at less than .001% of the total data it seemd like a safe move. (That one had an ASK_AMT of 36,500,179 also skewed the data.)
    * Again, trimmed off high outliers in the 'ASK_AMT' category, this time to the 90th percentile.
    * Did not bin 'INCOME_AMT' 
    * Binned the 'APPLICATION_TYPE' into thirteen bins
    * Binned the 'CLASSIFICATION' into sixteen bins
    * After dummies here, scaled data and made model of 8 layers with even more neurons the neurons as first round for 100 epochs.
        ### Loss: 0.5583467483520508, Accuracy: 0.7414004802703857
    *Using Keras_Tuner that created as many as 12 hidden layers with between 1-71 nodes maxed at 50 epochs and ran (went down in epochs because I noticed that in my above test things stabilized pmidway in the 100 epochs, again adding keras.callbacks.EarlyStopping with my patience set at 3 saved considerable time)
        ### Loss: 0.5377137660980225, Accuracy: 0.7520425319671631

    
    * Lessons learned: more is not always more- in some variations (not submitted) I was using the tuner to add as many as 15 layers but not enough nodes- to little to no positive effect and a lot of time iterating. Less is also not more as sometimes results were hurt by too small of bins and too few options remaining.  When the nuance of the data is smoothed out, the model doesn't work.

# Summary
    My results mirror what I know of real life funding decisions, there will always be many factors to consider!  Perhaps a logistic regression  or KNN model would provide additional insight as both are classifiers for labeled data. 