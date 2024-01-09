# Analysis Report

## Overview of the Analysis

### Purpose

- The purpose of this analysis is to support a nonprofit foundation called `Alphabet Soup` to help support them in selecting applicants with the best chances of being successful and using the funding effectively.

### Data Information and Model Goal

- The initial dataset contained historical data on approx. 34,000 previous organizations that have receieved funding.
- For each organization the dataset contained information on (but not limited to):
  - Affiliated sector.
  - Government classification.
  - Usage for the funding.
  - Income amount.
  - Funding request.
  - If the organization used the funding effectively.
- Using this data, I was tasked with creating a neural network model that could predict an applicants chances of being successful.

## Results

### Data Preprocessing

- What variable(s) are the target(s) for your model?
    - The target variable in my model is the `IS_SUCCESSFUL` column from the dataset.
        - This defines if `1` - the organization used the funding effectively or `0` - the organization did not use the funding effectively
    - There's another potential target which I haven't focussed on with my models `STATUS`:
        - The `STATUS` column of the dataset signifies if the organization is active or not
        - If I was to complete this analysis again, I would look to use both the `IS_SUCCESSFUL` and `STATUS` as target variables
        - This could provide a more comprehensive model creation for the nonprofit foundation to not only determine if the funding will be used effectively but also if the organization would remain active too
- What variable(s) are the features for your model?
    - The features of the model contain the following columns:
        - `APPLICATION_TYPE` - Due to the number of differing values within this column, the values have been 'binned' into an `Other` category if the value is `< 528`.
        - `AFFILIATION`
        - `CLASSIFICATION` - Due to the numebr of differing values within this column, the values have been 'binned' into an `Other` category if the value is `< 1883`.
        - `USE_CASE`
        - `ORGANIZATION`
        - `STATUS`
        - `INCOME_AMT`
        - `SPECIAL_CONSIDERATIONS`
        - `ASK_AMT`
- What variable(s) should be removed from the input data because they are neither targets nor features?
    - There are two variables that were removed from the dataset due to being neither targets nor features. These columns are:
        - `EIN` - A unique reference number given to each organization requesting/receiving funding
        - `NAME` - The name of the organization making a request/receiving funding from the foundation

### Compiling, Training, and Evaluating the Model

- How many neurons, layers, and activation functions did you select for your neural network model, and why?
    - `AlphabetSoupCharity_Model_V1.H5`
        - For the initial model I chose to run three layers:
            - Input Layer - `80` Nodes - `Relu` Activation
                - I chose to run 80 nodes for this layer due to the number of dimensions within the dataset (42)
            - Hidden Layer - `30` Nodes - `Relu` Activation
                - 30 Nodes was selected for this layer, again due to the number of dimensions within the dataset
            - Output Layer - `1` Node - `Sigmoid` Activation
        - The initial model resulted in:
            - Accuracy: `72.48%`
            - Loss: `57.95%`
    - Model Optimization:
        - My attempt to optimize the model made use of the `keras_tuner` library
        - This provides the ability to test a number of different options for the model, including:
            - The number of different hidden layers within the model
            - How many neurons per layer
            - Varying activation functions
            - Ultimatley outputting the parameters which worked best for model accuracy
        - I ran three different optimization models:
            - `AlphabetSoupCharity_Optimized_V1.h5` - This tuner ran the following options:
                - 1-5 Hidden Layers
                - Up to 80 nodes in the input layer, and up to 40 nodes in the hidden layers
                - The best model when ran with 100 epochs produced:
                    - Accuracy: `72.61%`
                    - Loss: `56.2%`
            - `AlphabetSoupCharity_Optimized_V2.h5`
                - 6-10 Hidden Layers
                - Up to 80 nodes in the input layer, and up to 40 nodes in the hidden layers
                - The best model when ran with 100 epochs produced:
                    - Accuracy: `72.33%`
                    - Loss: `56.08%`
            - `AlphabetSoupCharity_Optimized_V3.h5`
                - 1-5 Hidden Layers - Due to `V2` model having a slightly worse accuracy score compared to `V1`, I've reverted back to only 1-5 layers.
                - Up to 160 nodes in the input layer, and up to 100 nodes in the hidden layers
                    - I've increased the maximum size of nodes per layer to see if this improves the accuracy performance.
                - The best model when ran with 100 epochs produced:
                    - Accuracy: ``
                    - Loss: ``
- Were you able to achieve the target model performance?
- What steps did you take in your attempts to increase model performance?

## Summary

- In my opinion the second model appears to perform best and would be my recommendation as the model to use.
- It's evident that both models do a good job of predicting the `0` labels on both occasions. However, within the lending market the focus has to be on identifying those that fall into the `1` class.
  - This is where problems can arise and the company could end up losing money rather than making it.
- The confusion matrix for the second model shows:
  - Capability to correctly predict 18,668 `True Negatives`, and 623 `True Positives`.
  - The model predicted 91 `False Positives`, which is an increase of 11 (12%) compared to the previous model.
    - This does unfortunatley mean that some borrowers are incorrectly labeled as `1` (high-risk), but does protect the lending company's assets.
  - The main improvement for this model comes from it's number of `False Negatives`. Down to just 2 borrowers being falsely labeled as `0` (healthy).
    - This is a reduction of 65 `False Negatives` or a 97% decrease over the previous model.
    - A managable figure for a lending company, providing a much more accurate prediction on those borrowers who are in the `1` (high-risk) label.
