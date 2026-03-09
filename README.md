# PropellerData
A generative propeller design and ranking system powered by aerodynamic data and machine learning.


This project explores how machine learning can learn the relationship between propeller blade geometry and aerodynamic performance.

The model is trained using the UIUC Propeller Database. This dataset contains experimental aerodynamic measurements and blade geometry for a large set of propellers tested under different operating conditions.

The goal is to build a neural network that can predict aerodynamic coefficients from blade geometry and flight conditions. Once trained, the model acts as a fast aerodynamic surrogate that can evaluate propeller performance without running physical experiments or detailed simulations.


# Project Overview

The workflow in this repository follows a structured pipeline.

First, the aerodynamic dataset and geometry dataset are loaded from the UIUC propeller database.

Next, only propellers that exist in both datasets are kept. This ensures that every aerodynamic measurement can be paired with the geometry that produced it.

The blade geometry is then resampled so that every propeller is represented by the same number of radial stations. This step converts irregular geometry measurements into a consistent feature representation.

These geometry features are combined with experimental operating conditions such as advance ratio and RPM. The resulting dataset becomes the training input for the neural network.

The model is then trained to predict aerodynamic performance values.


# Dataset

The project uses two files from the UIUC Propeller Database.

### Aerodynamic Data

This dataset contains wind tunnel measurements for propellers across different operating conditions.

Key parameters include:

- prop_key  
- advance ratio (J)  
- thrust coefficient (CT)  
- power coefficient (CP)  
- propeller diameter  
- pitch  
- RPM  

Each row represents one experimental test point.

### Geometry Data

This dataset describes the physical blade shape of each propeller.

Key parameters include:

- prop_key  
- radial position along the blade (r_over_R)  
- chord distribution (c_over_R)  
- blade twist angle (beta_deg)  

Each propeller is defined by several measurements along the blade span.


# Data Processing

Blade geometry is converted into a machine learning friendly representation.

Each propeller blade is resampled to a fixed number of radial stations. Linear interpolation is used to generate chord and twist values along the span.

For example:

- chord values become features `c_00` to `c_59`
- twist values become features `beta_00` to `beta_59`

This converts each propeller geometry into a consistent numerical vector.

The geometry features are then merged with the aerodynamic dataset using `prop_key`.


# Feature Set

The neural network input contains both geometry and operating parameters.

Base parameters:

- number of blades  
- propeller diameter  
- propeller pitch  
- advance ratio J  
- RPM  
- tip speed  

Geometry parameters:

- chord distribution along the blade  
- twist distribution along the blade  

The final feature vector includes all geometry stations along with the operating conditions.


# Model

The model is a feed forward neural network implemented using TensorFlow and Keras.

Architecture overview:

- dense input layer  
- hidden layers with ReLU activation  
- dropout regularization  
- regression output layer

The model predicts two aerodynamic coefficients:

- CT (thrust coefficient)  
- CP (power coefficient)

Training uses scaled input features and a regression loss function.


# Repository Structure
