# Neural Network Control System: Identifier and Controller

## Overview
This project implements a neural network–based control system made of two components:

1. **Identifier Network** – Learns to model the plant’s nonlinear dynamics  
2. **Controller Network** – Generates control signals so the plant output tracks a desired reference  

Both networks are trained **online** using backpropagation.  
The control architecture is based on **Model Reference Adaptive Control (MRAC)**.

---

# System Architecture
- The **Identifier** learns the mapping between inputs and plant outputs.  
- The **Controller** uses this internal model to compute appropriate control actions.  
- Both networks are:
  - 2-layer feedforward networks  
  - Hidden layer activation: **logsig**  
  - Output layer activation: **purelin**  
  - Updated online using gradient descent  

---

# 1. Identifier Network

## Purpose
The Identifier neural network approximates the nonlinear plant by learning its forward dynamics.

## Neural Network Structure
- 2-layer feedforward network  
- Hidden layer: **logsig**  
- Output layer: **purelin**  
- User selects number of hidden neurons  

## Plant Dynamics Modeled
```matlab
yp(k+1) = ((-0.8*yp(k) - 0.14*yp(k-1)) / denominator) ...
          + u(k-1) - 0.5*u(k-2);

denominator = 1 + (0.6*yp(k) + 0.4*yp(k-1)) * yp(k-1);
