+++
date = '2026-05-17T17:35:16+08:00'
draft = false
title = 'Sutton Ch9'
+++

Function Approximation  
So far, we have considered methods that operate in gridworld environments whose state/action values are stored in memory. However in reality, such gridworlds are rare & environments are usually represented by continuous state variables. If such variables were to be stored in tabular manner, it will definitely require too much memory.  

Instead, we introduce function approximation. This uses certain methods[neural networks, linear methods etc] for estimating state/action values by using state and/or action pairs as model inputs, and having a differentiable weight vector w.

Mean Squared Value Error
![ve_error](/images/ve_error.png)
Here, the value function is estimated using a machine-learning model represented by ˆv(s,w). We can then differentiate w.r.t weight vector w for the difference between the predicted & "actual" state values. If a neural network is used, backpropagation can be used to update the neural network weights to minimize this error for future predictions.

