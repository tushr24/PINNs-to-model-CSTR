# Objective
Improve the performance of Neural Networks that model reactions in a CSTR.

# Brief Description
1. *Industrial Significance:*
   Chemical reactors, particularly Continuous Stirred-Tank Reactors (_CSTRs_), play a crucial role in various industrial processes. Despite their advantages in simplicity and efficiency, modelling the dynamic behaviour of CSTRs for reaction series poses challenges due to inherent nonlinearities and dynamic complexities.

2. *Assumptions:*
   The key assumptions to simplify and focus on the dynamic aspects of CSTRs are:
   * Unsteady state - outlet concentrations vary with time initially
   * Uniform mixing - concentration of species in the reactor is the same as its outlet concentration
   * Isothermal conditions - rate constants don't change with time

4. *Neural Networks:*
   The aim is to develop predictive models that capture intricate dynamics, paving the way for more efficient and accurate modelling in chemical engineering. Integrating domain knowledge and ML capabilities using Physics-Informed Neural Networks (_PINN_) aims to contribute to process control and optimisation advancements within the chemical engineering domain. The model should handle the complexities of the reactions and be robust enough to provide accurate predictions even in the face of uncertainties or changes in the system parameters.

# Methodology

![Different Stages of the Project](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/pinn_cstrdrawio.png)

[Group 1](https://github.com/AswinBalamurugan/PINN-CSTR/tree/main/Data%20generation%20and%20Inital%20model)
   * Data Generation
   * Creating a reference model (without PINN) 

[Group 2](https://github.com/AswinBalamurugan/PINN-CSTR/tree/main/Simplify%20modelling)
   * Simplifying dataset for better interpretation of results

[Group 3](https://github.com/AswinBalamurugan/PINN-CSTR/tree/main/PINN%20for%20flow%20rate%20and%20final%20model)
   * Modelling 'flow rate' varied dataset using PINN
   * Create the final model with PINN

(_Links provide more reasoning and analysis for each step._)

# Interesting Insights
### Data Generation
* It is essential to verify that the method for data generation is accurate. (compared results with research paper)
* Total time for simulation varies as we change the parameter (assumed to be 4*space-time)
* Adding uniform noise for all concentrations isn't feasible since some vary more other others. (noise is of the order of change in concentrations)
* Making time dimensionless helps the model to learn the dynamics better.

### Initial Model
* Common options like `train_test_split` do not work all the time. In this case, it leads to indeterminate loss and accuracy curves. Both the **train** and **test** curves were identical. (created new column `ID` to split the data)
* The most commonly used _ReLU_ (Rectified Linear Unit) activation led to highly fluctuating curves. (_Tanh_ was chosen)
* Unable to take advantage of _**GPU**_ while training due to the bottleneck being the huge data transfer from CPU -> GPU.
* Even though the final accuracy of the trained model is very high, the actual predictions barely matched the actual values. The R2 score doesn't fully explain the performance of the model.

### Simplified datasets
* Varying several parameters hyperparameters simultaneously makes it difficult to infer the resulting plots of model performance. It is almost impossible to figure out the fluctuations in the visualised plots without reducing the number of parameters that were varied.
* Identify which parameter leads to the fluctuations for further analysis.

### PINN for flow rate
* While computing gradients, the volume parameter is in the denominator. Hence, the variables must be rescaled to avoid division by zero, which would lead to infinite PINN loss.
* Since the forward difference method is used, remember to skip the first set of concentrations (_at t=0_) while computing _actual gradients_.
* To ensure that the gradient values are accurate, the batch size has to be set to 500, i.e., the number of data points for each simulation. This is because the gradient is calculated wrt **time**.

### Final Modelling
* What works for varying flow rates may or may not work for the entire dataset. This includes parameters like the model architecture, activations, learning rates and the coefficient for the PINN loss (currently 100).
* Any change in the above parameters drastically affects the model's performance.

|Neural Network **without** PINN loss|Neural Network **with** PINN loss|
|--------|--------|
|![w/opinns](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/ref_model.png)|![pinns](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/final_pinn.png)|

_Implementing PINN has increased the performance of the Neural Network model and led to smoother learning curves._

# Conclusion
1. *Comprehensive Neural Network Understanding:*
   I delved into various Neural Network concepts throughout this project, including activation functions, optimisers, and result interpretation. This foundational knowledge laid the groundwork for the subsequent incorporation of advanced concepts.

2. *Enhanced Learning with PINNs:*
   The successful integration of the Physics-Informed Neural Networks (PINNs) loss concept was a significant achievement. This enhancement facilitated better learning for the model and contributed to its robustness in handling the complexities of dynamic CSTR systems.

3. *Model Application and Future Scalability:*
   The trained model showcases its capability to predict output concentrations of species in an unsteady state within an ideal CSTR. Hyper-parameter tuning and incorporating additional data could further enhance the model's scalability, making it suitable for deployment at an industry level.

|Reference Model Predictions|PINN model predictions|
|--------|--------|
|![w/opinns](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/bad_pred.png)|![pinns](https://github.com/AswinBalamurugan/PINN-CSTR/blob/main/images/final_pred.png)|


