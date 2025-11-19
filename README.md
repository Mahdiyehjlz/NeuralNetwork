# Neural Network Control System: Identifier and Controller

## Overview
This project implements a neural network-based control system with two main components:
1. **Identifier Network** - Learns to model the plant's nonlinear dynamics
2. **Controller Network** - Generates control signals so the plant output tracks a desired reference

Both networks are trained **online** using backpropagation in a **Model Reference Adaptive Control (MRAC)** architecture.

---

## System Architecture

The system employs a dual-network approach:
- **Identifier**: Learns the mapping between control inputs and plant outputs
- **Controller**: Uses the internal model to compute optimal control actions

**Network Specifications:**
- 2-layer feedforward architecture
- Hidden layer: logsig activation
- Output layer: purelin activation  
- Online gradient descent training

---

## 1. Identifier Network

### Purpose
Approximates the nonlinear plant dynamics by learning the forward model from input-output data.

### Neural Network Structure
- 2-layer feedforward network
- Hidden layer: logsig activation function
- Output layer: purelin activation function
- User-configurable hidden layer size

### Plant Dynamics
The system models nonlinear plant behavior with delayed inputs and outputs, incorporating both linear and nonlinear components in its dynamics.

### Training Process
1. **Forward propagation** to compute predicted output
2. **Error calculation** between actual and predicted output
3. **Backpropagation** to compute weight sensitivities
4. **Weight updates** using gradient descent
5. **Iterative refinement** over 150 training cycles

### Key Features
- Robust denominator checking for numerical stability
- Proper array sizing for handling system delays
- Progress monitoring with error display every 10 iterations
- Training convergence visualization

### Usage
User specifies hidden layer size during execution for flexible architecture configuration.

---

## 2. Controller Network

### Purpose
Generates control signals to make the plant output follow a desired reference trajectory.

### Dual-Network Architecture
- **Controller Network**: Produces control inputs
- **Model Network**: Provides Jacobian estimation for controller adaptation
- Both networks share identical architecture and online training

### Control Strategy
- **Reference tracking** with constant desired output
- **Error metrics**:
  - Control error: difference between desired and actual output
  - Modeling error: difference between plant and model output

### Adaptation Mechanism
1. Model network updates based on modeling error
2. Jacobian estimation from model network
3. Controller updates using control error and Jacobian information
4. Real-time parameter adjustment

### Key Features
- Simultaneous online adaptation of both networks
- Jacobian-based sensitivity analysis
- Comprehensive performance visualization:
  - System response comparison
  - Control error tracking

### Usage
User-defined hidden layer size allows for customized network capacity.

---

## Mathematical Foundation

### Backpropagation Algorithm
Weight and bias updates follow standard gradient descent:
- Weight update: learning rate × sensitivity × input transpose
- Bias update: learning rate × sensitivity

### Activation Functions
- **Logistic Sigmoid**: Smooth, bounded nonlinearity for hidden layer
- **Linear**: Unbounded output for final control signals
- **Derivative functions**: Implemented for backpropagation

---

## Implementation Details

### Parameter Settings
- Learning rate: 0.01 for stable convergence
- Network layers: 2 (hidden + output)
- System type: Single-input single-output (SISO)
- Training duration: 200 time samples

### Initialization Strategy
- Random weight initialization from normal distribution
- Zero initial conditions for system signals
- Proper array allocation for time-delay components

### Safety Measures
- Denominator protection against division by zero
- Numerical stability checks
- Array bounds verification

---

## Expected Results

### Identifier Performance
- Decreasing mean squared error over training iterations
- Successful learning of plant dynamics
- Stable convergence behavior

### Controller Performance
- Plant output tracking reference signal
- Decreasing control error over time
- Stable adaptive control performance

---

## Applications

This system demonstrates practical implementation of:
- Nonlinear system identification
- Adaptive control systems
- Model Reference Adaptive Control (MRAC)
- Real-time dynamic system control
- Neural network applications in control engineering

---

## Requirements

### Technical Prerequisites
- MATLAB environment
- Understanding of neural networks
- Knowledge of control theory
- Familiarity with adaptive systems

### Educational Value
Provides complete working example of:
- Neural network-based system identification
- Online adaptive control implementation
- MRAC architecture in practice
- MATLAB programming for control systems

---

## Summary

This project presents a comprehensive neural network control system that:
1. **Identifies** complex nonlinear plant dynamics through learning
2. **Controls** the plant to track desired references using adaptive strategies
3. **Adapts** online using backpropagation and gradient descent
4. **Demonstrates** practical MRAC implementation with neural networks

The complete MATLAB implementation provides a valuable educational and research tool for understanding neural network applications in adaptive control systems, showcasing real-time learning and control capabilities for nonlinear dynamical systems.
