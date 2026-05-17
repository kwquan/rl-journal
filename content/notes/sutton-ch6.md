+++
date = '2026-05-17T16:30:16+08:00'
draft = false
title = 'Sutton Ch6'
+++

Temporal-Difference Learning  
Here, we combine the ideas of monte-carlo & dynamic programming to td-learning, which can be said to be the novel & central idea to reinforcement learning. Similar to monte-carlo, td-learning learns from experience. And similar to dynamic programming, TD methods bootstrap.  

TD Prediction  
![Incremental update](/images/incremental_update.png)
Recall the incremental update rule for monte-carlo methods as shown above.

![Temporal Difference Prediction](/images/td_prediction.png)
Here, we replace the return with the sum of immediate reward + discounted value of the next state-value. This sum is known as the td target. The difference between this sum & the current state-value is known as the td error. This example is also known as TD(0) or one-step TD, since we are doing 1 step look-ahead.  

Advantages 
[VS monte-carlo methods] 
1) Only need to wait 1 step[instead of end of 1 episode as required by monte-carlo methods](a.k.a do not need to wait until after you die to update)
[VS DP methods]
1) DO NOT require model of the environment[reward, transition probability etc]

TD Prediction methods  
We divide them into 2 methods:  
On-policy: Improved policy is also used to select actions  
Off-policy: Improved policy & actual policy are different  

SARSA: On-policy TD Control
![SARSA](/images/sarsa.png)
The first method is known as SARSA. This comes from the quintuple required for the update rule to work: current state, current action, reward, next state & next action[as shown above].

Q-learning: Off-policy TD Control
![Q-learning](/images/qlearning.png)
The off-policy variation is known as Q-learning. Similar to SARSA, instead of using the next action for the update, it looks at the action that maximizes the next action-value[as shown above].

Of course, there exist different variations of the above[such as expected SARSA, Deep Q-learning etc]. Nonetheless, the above 2 basic variations should provide readers enough understanding to solve environments using TD methods.