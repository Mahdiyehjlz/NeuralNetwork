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
```
---
#Training Process

Forward propagation to compute predicted output

Compute prediction error

Backpropagation to compute sensitivities

Update weights and biases

Repeat for 150 iterations

#Features

Robust denominator checking

Correct array sizing for delayed inputs and outputs

Error displayed every 10 iterations

Convergence curve showing training error

#Usage
nPerc. in the [1th] layer is: 4

##2. Controller Network
Purpose

Generates control signals u(k) so the plant output yp(k) follows the reference signal yd(k).

Two-Network Structure

Controller Network: Generates control input

Model Network: Approximates plant Jacobian for controller adaptation

Both:

Use the same architecture

Are trained online

Control Strategy

Reference signal:

yd(k) = 1;


Error Definitions:

Control error:
e_c(k) = yd(k) - yp(k)

Modeling error:
e_m(k) = yp(k) - ym(k)

Adaptation Method

Model network is updated using modeling error

Jacobian estimated from model network

Controller updated using control error and Jacobian information

Features

Real-time adaptation

Jacobian estimation

Plots include:

System response (desired vs. actual)

Control error over time

Usage
nPerc. in the [1th] layer is: 4

Mathematical Foundation
Backpropagation
weights = weights + alpha * sensitivity * input';
bias    = bias    + alpha * sensitivity;

Activation Functions

logsig(x) = 1 / (1 + exp(-x))

purelin(x) = x

Derivatives implemented in code

Implementation Details
General Settings

Learning rate: 0.01

2-layer network

SISO system

Training length: 200 samples

Initialization

Random weights and biases

Zero initial signals

Arrays sized for delays (u(k–2), yp(k–1))

Safety Controls

Denominator protection

Numerical stability checks

Expected Outputs
Identifier Results

Training error plot

MSE decreasing over iterations

Controller Results

Plant output (red dashed) vs. desired output (blue)

Control error plot showing convergence

Applications

Nonlinear system identification

Adaptive control

Model Reference Adaptive Control (MRAC)

Real-time dynamic system control

Requirements

MATLAB

Knowledge of:

Neural networks

Control theory

Adaptive systems

This project demonstrates a complete working example of neural network-based system identification and adaptive control using MATLAB.
