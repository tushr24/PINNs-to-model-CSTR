Please refer: [My Project Collection](https://github.com/AswinBalamurugan/Machine_Learning_Projects/blob/main/README.md)

# Objective
Simplify modelling by changing the dataset so only one variable changes at a time.

# Modelling
I was unable to interpret the results obtained from the initial model. Hence, the dataset was simplified for better interpretability by varying only one parameter at a time.
The modelling and plotting of results were repeated for each of the four datasets that were created:
1. Variable Flow rate
2. Variable Volume
3. Variable Initial Concentration of A
4. Variable Initial Concentration of B

|Fluctuating curves for **variable Flow Rate**|Smooth curves for **Concentration of A**|
|---|---|
|![flow](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/fluc_flow_rate.png)|![others](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/smooth_others.png)|

# Conclusions
The model fits well, and the test accuracy curves didn't fluctuate the datasets except for **variable flow rate**.
The model cannot capture the complex dynamics in the **variable flow rate** case. 
Hence, I have added PINN to the neural network model and expect the fluctuations to disappear, implying a stable trained model.
