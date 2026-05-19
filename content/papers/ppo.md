+++
date = '2026-05-18T17:55:25+08:00'
draft = false
title = 'Proximal Policy Optimization'
+++
Proximal Policy Optimization[PPO]  
link: https://arxiv.org/abs/1707.06347  
![Policy Gradient](https://kwquan.github.io/rl-journal/images/policy_gradient_equation.png)
As described in sutton chapter 13, policy gradient can be expressed as the product of policy & action-value function. 

![Alternative Policy Gradient](https://kwquan.github.io/rl-journal/images/alternative_policy_gradient.png)
However, the most commonly-used policy gradient uses a log form to make partial derivative easier

![Probability Ratio](https://kwquan.github.io/rl-journal/images/probability_ratio.png)
One can use the probability ratio as shown above, in place of the policy/log-policy in the objective function. The denominator represents the probability of selecting action A under old policy while the numerator represents the probability of selecting action A under new policy. The idea is that if selecting this action is seen as optimal, the ratio should be > 1.

![Advantage Function](https://kwquan.github.io/rl-journal/images/advantage_function.png)
One can also use the advantage function in place of q(s,a) as shown above. This is the difference between q(s,a) & v(s)[a.k.a the baseline] & shows how much better it is to select action A in state S compared to the average return. Idea is that if selecting action A in state S leads to higher returns, then difference should be > 1.

![TRPO Objective](https://kwquan.github.io/rl-journal/images/trpo_objective.png)
In the TRPO[trust region policy optimization] paper, the agent seeks to maximize the objective function shown above. The product of probability ratio & advantage function could be very large, hence leading to unstable weight updates. In TRPO paper, this problem is mitigated by introducing a fixed penalty coefficient β.  

![PPO Objective](https://kwquan.github.io/rl-journal/images/ppo_objective.png)
In PPO, the authors argued that this β is hard to estimate & instead propose a clipping method for stable weight updates. If product is too positive, 1+epsilon is used instead. If product is too negative, it is used in place of 1-epsilon.["With this
scheme, we only ignore the change in probability ratio when it would make the objective improve,
and we include it when it makes the objective worse."]

![Final PPO Objective](https://kwquan.github.io/rl-journal/images/final_ppo_objective.png)
In addition to the above, the authors also added the usual value function error term[difference between "true" state value & model estimate] as well as an entropy value to encourage exploration.   
For entropy portion:  
 For example, consider 4 actions. If probabilities are [0.25, 0.25, 0.25, 0.25]  → explores a lot & entropy value will be high. Conversely, if probabilities are [0.97, 0.01, 0.01, 0.01]  → barely explores & entropy value will be low. 

So main idea is to calculate policy gradient[probability ratio * advantage], value loss[difference between "true" & predicted state values] & entropy, combine them into an objective function & train an agent based on that. 






