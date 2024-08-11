Please refer: [My Project Collection](https://github.com/AswinBalamurugan/Machine_Learning_Projects/blob/main/README.md)

# Objective
* Generate data using ODE for reactions in CSTR to get outlet concentrations
* Create a simple neural network model on the training dataset

# Data Generation
The chosen chemical reactions for the CSTR are:
1. **A + B → X**
2. **B + X → Y**
3. **B + Y → Z**

The time required for the CSTR to achieve a steady state was assumed to be *4 * space-time*. Hence, outlet concentrations were calculated till that time in 500 time steps.
The `solve_ivp` python module was used to solve the ODEs obtained using the reaction equations to calculate the outlet concentrations.
There were **7** total input parameters:
1. Flow rate
2. Volume
3. Inlet Concentrations for A, B, X, Y and Z

The inlet concentrations for X, Y and Z were assumed to be zero. The remaining **4** parameters were varied to generate the dataset.

## Ensuring `solve_ivp` gives accurate results
Results from the linked [research paper](https://www.researchgate.net/publication/332414465_Modeling_and_Simulation_Study_of_the_CSTR_for_Complex_Reaction_by_Using_Polymath) were first reproduced.
This step is essential since any error complicates the modelling interpretation. 

|Research Paper Concentrations|Reproduced results using `solve_ivp`|
|-----------------------------|------------------------------------|
|![research](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/research_paper.png)|![reproduced](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/reproduced_results.png)|


## Generating the datasets
The **four** parameters were varied as given below:
1. Flow rate ranged from *0.001* to *0.1* in *15* steps
2. Volume ranged from *0.5* to *10* in *15* steps
3. Initial concentration of A ranged from *0.1* to *1* in *5* steps
4. Initial concentration of B ranged from *0.1* to *1* in *5* steps

Each combination can be identified using a unique `ID` allocated during generation.
So, each ID will have *500 data points*, i.e., 500 time steps.
There are 5625 (15 * 15 * 5 * 5) unique IDs, totalling ***28,12,500 data points***.
The `Time` values were scaled by dividing all values by the corresponding **space-time** value.
Noise was added to the outlet concentrations obtained using the ode solver to resemble real-world scenarios.

Additionally, *four* more datasets were created (*reason in below sub-section*) similar to the above dataset (*15 IDs for each*):
1. Varying only the **flow rate** 
2. Varying only the **volume** 
3. Varying only the **initial concentration of A**
4. Varying only the **initial concentration of B**

# Neural Network Reference Model
After generating the training data, a simple neural network model was required as a reference to understand the effects of using PINN.
The data was split into train and test data first; **4000** IDs were training, and the remaining were test data.
Initially, the `train_test_split` module of the `sklearn` python library was used, which led to identical train and test loss curves.
The inputs to the model were `Flow rate, Volume, Initial Concentrations and Time`, and the outputs were the `Final concentrations of the species`.
MSE (_Mean Squared Error_) and R2 score were used to interpret the performance of the trained model.
Hyperparameters like layers, nodes, activations and optimisers were changed to find the best model. But all combinations resulted in curves which fluctuated a lot.
Due to several varying factors, it wasn't easy to pinpoint the exact reason for the model's behaviour. So, I went back to data generation and created four new datasets to restart the modelling procedure and find out the reason behind the fluctuating behaviour of the trained model.

|Accuracy of reference model|Predictions contradicting the high accuracy|
|----------------------------|--------|
|![refModel](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/ref_model.png)|![bad](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/bad_pred.png)|

# Conclusions
* Successfully generated data for training.
* Reference model gave fluctuating results.
* Restart the modelling procedure by simplifying the dataset to reduce variability.
