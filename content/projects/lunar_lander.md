+++
date = '2026-05-20T15:55:25+08:00'
draft = false
title = 'Lunar Lander'
+++
gymnasium documentation link: https://gymnasium.farama.org/environments/box2d/lunar_lander/

PPO reference code: https://github.com/vwxyzjn/cleanrl/blob/master/cleanrl/ppo.py

This page contains the explanation for the code[based on the cleanrl ppo repo] used to solve the lunar lander problem[V3] using Proximal Policy Optimization.

Refer to the official documentation for explanation on actions & rewards. To put it simply, the objective is to train an agent to land the lunar lander safely, within the 2 marked flags by either:
1)doing nothing 
2)firing thruster 
3)firing left thruster 
4)firing right thruster  
Agent needs to achieve safe landing using the least firing of thrusters.

INSTRUCTIONS   
1) Download "lunar_lander.py" & "lunar_lander_ppo.pth".  

2) If training of agent is desired, change "train_agent" to "True"[default value].  

3) Ensure all required packages are already installed.  

4) Run "python lunar_lander.py" in directory containing the 2 downloaded files in command prompt.  

5) Sit back & observe agent training and/or rendering in action.

CODE EXPLANATION
![Classes & Function](https://kwquan.github.io/rl-journal/images/lunar_lander_classes.png)
We introduce the Args class containing all of the required parameters for agent training:
    num_episodes: number of training episodes
    num_steps: number of training steps for each episode. We shall stick with this for now[as introduced in the referenced repo]. Else, can change to end episode upon done[either terminate or truncate] 
    gamma: for calculating delta, advantages
    gae_lambda: for calculating advantages
    batch_size: training batch size. We shall consider all of each episode for now
    minibatch_size: For splitting each training batch into minibatches
    clip_coef: for preventing ratio from becoming too big. As mentioned in the PPO paper, if ratio is too positive, we shall use 1+clip_coef. If it's too negative, we shall use 1-clip_coef.
    ent_coef: for multiplying to entropy loss
    vf_coef: for multiplying to value loss
    norm_adv: if set to True, will normalize advantage
    update_epochs: number of training passes through each training batch
    max_grad_norm: for preventing gradient updates from becoming too large
    learning_rate: model learning rate
    train_agent: if train agent, set to True. If want to load to trained weights & observe agent in action only, set to False
    render_episodes: number of episodes for observing trained agent in action

We also introduce Agent class, which inherits from nn.Module. This class contains the neural network architecture for both actor & critic models, in addition to 2 methods[get_value, get_action_and_value]. get_value method takes state as input to critic & outputs estimated state value. get_action_and_value takes states as input to actor & outputs action, logprob, entropy & estimated state value[from critic].  

Note that the logits produced from self.actor(x) are numbers. We convert them to probability distribution using Categorical(logits).  

First: initialise Args class as args  

Next: if train_agent, starts agent training. Else, skips to render portion.
![Create arrays](https://kwquan.github.io/rl-journal/images/lunar_lander_start.png)
Agent training:  
1) Turn on interactive mode for matplotlib to observe episode rewards after each episode. Ideally, we should see reward curve going up & remain steady around reward = 200. Recall that 200 is the threshold to solve this environment according to the official documentation.  

2) Initialize device, environment, agent & optimizer.

3) Create empty episode_rewards array. This is to record total rewards at end of each episode for interactive plotting. Also create zero arrays for obs, actions, logprobs, rewards, dones, values. We will use them to record the corresponding values for each STEP.  

![Agent training](https://kwquan.github.io/rl-journal/images/lunar_lander_training.png)
For each episode:  
1) Start by initializing next_obs using env.reset() & next_done with a zero array.   

For each step[within each episode]:  
1) Record next_obs & next_done in obs & dones arrays respectively.  

2) Pass next_obs to agent.get_action_and_value() to obtain action, logprob, value. Record them in their respective actions, logprobs & values arrays respectively.  

3) Pass action to env.step() to obtain new next_obs, reward, terminate & truncate. Record reward in rewards array.  

4) If terminate or truncate[refer to documentation for definitions], episode ends. In this case, the rest of the "unfilled spaces" in the respective created arrays will still be populated with zeros.  

At END of each episode:  
1) Calculate next_value. This is the estimated state value of the NEXT step after the very last step of num_steps. Create advantages array & populate with zeros.  
![Advantage Calculation](https://kwquan.github.io/rl-journal/images/advantage_calculation.png)
2) This part is abit tricky. Recall that advantage is calculated as shown above. We start by calculating advantage for very last step, then backstepping from there. After the advantage for very last step is calculated, subsequent calculations are done by taking the sum of current delta & the delta calculated 1 timestep after. 

3) Finally calculated returns as advantages + values.  Append sum of episode rewards to episode_rewards as well. If trained properly, should see a similar graph as "episode_rewards.png".   

![Epoch training](https://kwquan.github.io/rl-journal/images/lunar_lander_epoch.png)
For model update[for each epoch]:  
1) Create array of batch indices[b_inds]. In this case, it is [0,...,511]. Shuffle & seperate into minibatches. In this case, minibatch_size = 32 so each subsequent slicing will separate the minibatch into arrays of size 32 each.   

For each minibatch:
1) Get array of minibatch indices[mb_inds]. 

2) Pass minibatch obs & actions to agent.get_action_and_value() to obtain newlogprob, entropy & newvalue. Also get minibatch advantages. Recall that we can calculate probability ratio by: a) Taking the difference between newlogprob & logprobs 
b) Taking the exponential of the result  

3) Normalize advantages & calculate pg_loss. Why pgloss_1 & pgloss_2? pgloss_1 is the unclipped version & pgloss_2 is the clipped version. Using pg_loss = torch.max(pg_loss1, pg_loss2).mean() ensures we won't be using any pgloss that's too extreme in value.  

4) Calculate v_loss = 0.5 * ((newvalue - returns[mb_inds])**2).mean(). 0.5 is used to facilitate easier derivation.  

5) Calculate entropy_loss = entropy.mean().  

6) Finally, calculate loss = pg_loss - args.ent_coef * entropy_loss + v_loss * args.vf_coef

7) Backpropagation 

AFTER training completed:  
1) Save trained model weights to "lunar_lander_ppo.pth"

2) Run render portion 

Render Portion  
1) Initialize environment, device & agent. Load saved trained model weights.   

2) For each episode, observe agent in action.  Here, episode ends when terminate OR truncate[instead of possibly waiting until end of num_steps].  

3) Finally, print out number of episodes with total reward exceeding 200.  

Conclusion  
PPO is definitely not easy to digest. The above code & the "PPO" post[under "papers" section] serve to make it easier for newcomers by simplifying the code used & explaining the concepts involved.  
The trained agent is by no means the best-performing. Feel free to tweak the params & improve it further.

















