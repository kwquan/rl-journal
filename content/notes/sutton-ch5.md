+++
date = '2026-05-16T14:30:16+08:00'
draft = false
title = 'Sutton Ch5'
+++
Model Free Prediction  
In reality, perfect model of the environment is rarely known[reward,transition probability, policy etc]. Thus, there is a need to estimate the value function of an unknown MDP.  

Monte-Carlo
One of the methods we can use to estimate an unknown MDP is monte-carlo. As the term implies, monte-carlo learns directly from experience by estimating the value of each state as simply the average of episodic rewards from that state onwards. The idea is that as each state is visited enough times, the average reward for each state should approach it's real state-value accordingly to the law of large numbers. We can divide it into first-visit & every-visit monte-carlo.

Incremental Update
![Incremental update](/images/incremental_update.png)
Recall the equation for incremental mean update as shown above.  
This avoids the need to store every single return everytime the state-value is updated. In fact, this equation is central to RL & will ve use as the update rule for future chapters as well.

First Visit
![First-visit monte-carlo](/images/first_monte_carlo.png)
For first-visit, the rewards from each FIRST visit to each state are recorded. The mean value is incrementally-updated following each episode termination. 

Every Visit
![Every-visit monte-carlo](/images/every_monte_carlo.png)
For every-visit, the idea is similar, except we consider every rewards following every single visit from that state & update it's mean incrementally.

Shortcomings  
Of course, with such methods, there comes with a number of shortcomings as listed below:  
1) Episodes MUST terminate
2) State-values are only updated at the end of each episode  
3) Some states may not be visited enough[or at all]


