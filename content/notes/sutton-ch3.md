+++
date = '2026-05-13T10:54:25+08:00'
draft = false
title = 'Sutton Ch3'
+++
Markov Decision Process
![MDP](/images/mdp.png)
Markov decision process consists of an agent interacting with an environment.  
 The agent implements action A in state S at time t, with the environment returning State S and reward R at time t+1.  
 Current state contains enough information about the past.  
 In otherwords, the future is independent of the past given the present.

 Backup Diagram
 ![Backup Diagram](/images/link.png)
 This diagram shows how state-action values and state-values are linked.  
 Starting from a given state S, the agent takes different actions A with probabilities defined by policy π,  
 which leads to different states S' & reward defined by transition probabilities p(S',r|s,a).

 State Value 
![Value Function](/images/value_function.png) 
State Value[or Value function] measures how good it is to be in a particular state S.  
It consists of the following: Summation of all probabilities of action A, multiplied to summation of all transition probabilities to different states S',  
multiplied to sum of reward + discounted value of next state value V(S').

State-Action Value
![Action-Value Function](/images/action_value_function.png) 
State-Action Value measures how good it is to be in a particular state S AND taking action A. 
It is. a shortened version of the State value[without the summation over all action probabilities]: Summation of all transition probabilities to different states S',  multiplied to sum of reward + discounted value of next state value V(S').

Note that the above equations for both V(S) & Q(S,A) are known as bellman equations, which will be used as update rules in the next chapter.
