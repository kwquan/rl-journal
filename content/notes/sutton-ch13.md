+++
date = '2026-05-17T19:45:16+08:00'
draft = false
title = 'Sutton Ch13(a brief summary)'
+++

Policy Gradient Methods  
If a model is used to estimate policy using a differentiable parameter, these methods are known as policy-gradient methods.

![policy_gradient](/images/policy_gradient.png)
As shown above, if state-value function is used as performance measure, it's derivative can be shown to be directly-proportional to the derivative of the policy. Thus, doing gradient descent on this can lead to an improved policy as well.

![actor_critic](/images/actor_critic.png)
Actor-Critic Methods  
Methods that estimate both value function & policy are known as actor-critic methods. The actor refers to the learned policy, which takes state as input & outputs action. The critic "critizes" the action by calculating it's action-value estimates. The loss calculated by taking the difference between "true" & estimated action values will enable gradient descent for both estimate models & gradual improvement towards optimality.

Further notes will mostly focus on different variations of this model & how they are applied to mostly continuous state environments.

