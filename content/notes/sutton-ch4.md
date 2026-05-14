+++
date = '2026-05-14T09:41:25+08:00'
draft = false
title = 'Sutton Ch4'
+++
Dynamic Programming. 
Dynamic programming involves breaking down a problem into subproblems, whose solutions can be combined to become the solution to the main problem. Although it's uses are limited due to assumption of a perfect model & great computational expense[curse of dimensionality], nonetheless it serves as a great introduction to RL problems.  

Prediction
![Value Function](/images/value_function.png) 
This refers to computing the state-value function V(s) for a policy π.  
Recall that the value function is calculated as shown above. 

![Value Function Update Rule](/images/value_function_update_rule.png) 
To modify it for updating state-values, we simply replace the π on the left side with k+1 & the π on the right side with k. 

Control.  
This refers to updating current policy to the optimal policy. 

Policy Iteration
![Policy Iteration](/images/policy_iteration.png) 
This is the iterative process of calculating state-value from the given policy, updating to new policy & calculating the state-value for new policy until optimal policy is found.   

The simplest example to illustrate this will be the frozen lake problem[refer to https://github.com/kwquan/farama-FrozenLake] where the agent learns to navigate in a gridworld environment from the start to goal state without falling into any of the holes. By doing a number of policy iteration updates, the optimal path from start to goal state can be found. 

Value Iteration
![Value Iteration](/images/value_iteration.png) 
Instead of doing policy evaluation & improvement separately, one can truncate the above by updating the state-value to the action which maximizes it, with the optimal policy determined after a number of sweeps by following the action that leads to the grid with the max state-value at every state. 

Generalized Policy Iteration
![Generalized Policy Iteration](/images/generalized_policy_iteration.png) 
The continuous interaction between policy evaluation & improvement can be shown by the diagram above. One starts with an initial policy π with it's state-value V(S) which gets continuously updated & improved until both reaches optimality. 




