Please refer: [My Project Collection](https://github.com/AswinBalamurugan/Machine_Learning_Projects/blob/main/README.md)

# Objective
* Implement PINN for the reference neural network model for varying flow rates.
* Implement this final model for the dataset with all parameters varying.

# PINN for varying flow rates
Computed PINN loss using the _MSE_ between the actual and obtained gradients.
1. **Actual Gradients:** This is computed by using the concentration values of the species at the inlet and outlet obtained from the inputs and outputs of the model. These values are then substituted in the ***ODEs*** for each species.
2. **Obtained Gradients:** This is computed by taking ***forward difference*** for the obtained output concentrations with time from the model.

The computed PINN loss is then added to the loss function of the neural network model.
* _Note: To ensure the computed gradients are accurate, I set the batch size as 500, i.e., the time steps for each simulation._

|PINN results in smooth learning|
|-------------|
|![smooth](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/pinn_flow_rate.png)|

# Final Model
After successfully implementing PINN for the varying flow rates, the same model was initially used for the training dataset to compare its performance with the reference neural network model.
The final model architecture is as given below:
* 4 hidden layers
* 8 input nodes and 5 output nodes
* 512 hidden nodes in each layer
* Batch size: 500
* 50 training epochs
This model took close to 5 hours to train.

|Accuracy curves for Final Model|Predictions of Final Model|
|-------------|-----------|
|![final](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/final_pinn.png)|![pred](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/final_pred.png)|

# Conclusions
* Successfully implement PINN in the case of **variable flow rate**.
* Obtained smooth accuracy curves after training.
* Except for the **final concentration of Z**, the model captured the dynamics of all the output concentrations.
* The PINN model drastically outperformed the reference model on the _training dataset_.
* PINN boosted the base model's performance, helping it better learn the physics underlying the data.
