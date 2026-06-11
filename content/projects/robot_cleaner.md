+++
date = '2026-06-11T09:55:25+08:00'
draft = false
title = 'Robot Cleaner'
+++
This page contains the explanation for the code used to solve the custom robot cleaner environment in isaac sim using Proximal Policy Optimization. 
![Simulation layout](https://kwquan.github.io/rl-journal/images/simulation_layout.png)
The problem is as follows:  
Robot needs to navigate from start position through this L-shaped room without colliding with the walls while covering as much ground as possible[revisiting previously-covered coordinates is discouraged] in N number of steps[default 512].   

How to run:  
1) Ensure isaac sim installed.  
2) Clone repo[https://github.com/kwquan/robot_cleaner]  
3) If train agent, set train_agent = True.[Warning: will take quite a while]    
4) Cd to repo directory and run "C:\Program Files\isaac-sim\python.bat" robot_cleaner.py in command prompt. Change front part to match YOUR isaac sim python path.  
5) Observe agent in action.

Initial Aim:  
Wanted to add in a battery level component and a recharge pad. Every movement costs the robot 0.5 battery and if robot runs out of battery, it incurs a large negative reward. If robot moves to charging pad, battery level becomes full.  

What didn't work:  
1) Hard to train the agent for 3 competing objectives[periodic charging, no wall collision, max coverage].  
2) Wanted to separate wheel actions into 2 separate actions[left & right]. Then realized it's better to group them together for simplicity.  
3) Tried using isaac sim's collision detection but realized it's quite complex and not accurate.  
4) Tried headless = True and render = False mode but physics does not match actual rendering environment.  
5) Initially tried no rendering step but realized that it's required for physics to settle.  

What I did instead:  
1) Reduced to 2 objectives instead[no wall collision + max coverage]  
2) Grouped actions into 5 possible actions[see below].  
3) Used coordinates check for collision check[see below].  
4) Trained using rendering[took more than 1 day](I'm using nvidia 3080 10GB GPU).  
5) Set rendering to 20 steps.

Robot design:  
Even the design faced some initial difficulties. For starters, does everyone know how a robot cleaner moves? On initial observation, it seems to use 2 wheels[left & right] but upon initial design, these 2 wheels are insufficient to maintain balance. Robot will either tilt forward or backward even when it's stationary. On closer inspection, I realized that most robots have a 3rd wheel[a.k.a caster] for maintaining balance. After adding the caster wheel[the round thing at the end], robot's balance is much better though still slightly awkward.  

5 movements:  
1) Do nothing
2) Forward
3) Backward
4) Rotate clockwise
5) Rotate anticlockwise

State:  
Mostly from robot.get_world_pose() which gives xyz coordinates and xyz orientation. Here, we use only the xy coordinates and z orientation(yaw)[to know where robot is facing]. For each xy coordinate pair, we add them to route_taken list if it's not in there.

Also used robot.get_joint_velocities() to get left wheel and right wheel velocities. 

Added a coverage component to measure how robot has managed to cover new ground: coverage = round(route_len / self.steps,2). Maximum is 1[if robot covers new coordinate every step].

We return a total of 7 variables to describe the state at every step:   
[round(position[0],1), round(position[1],1), round(float(np.sin(yaw)),2), round(float(np.cos(yaw)),2), left_wheel_vel, right_wheel_vel, coverage].  

![Room layout](https://kwquan.github.io/rl-journal/images/room_layout.png)
The L-shaped room is described as above.  

Collision check:  
in_bottom = (2 < state[0] < 28) and (2 < state[1] < 10)  
in_top = (22 < state[0] < 28) and (14 < state[1] < 28)

where state[0] is the x-coordinate and state[1] is the y-coordinate.
The robot chasis radius is 2cm. As long as x and y coordinates fall in the above, it's not colliding with any of the walls. Else, collision is set to True.  

Reward check:  
3 sections[collision, new ground covered and an additional coverage component]. 
If collide, immediately returns -5 reward.  
Else, check xy coordinate covered. If it has been covered before, add -0.5 reward, else add +1 reward. Then add a reward += round(len(self.route_taken)/5400 * 0.01,2) to reward based on total new ground covered.  

Why 5400? Because each coordinate is rounded to 1dp. Thus, there are 5400 possible coordinates in total[total area of top and bottom sections of the room].

Reset:  
We call this at start of every episode.  
Resets robot to original position, truncate and terminate to False.[In fact, terminate isn't used anymore since battery component is removed.] route_taken set to empty list and steps to 1.

Step:  
We call this for robot to take action and enter new state.  
Code is quite self-explanatory except the following:  
robot.apply_action(action) will apply the action[1 of 5 possible actions].  
Need to add a world.step(render=True) for physics to settle.  
In the end, return new state and reward.

Code:  
Largely following what I did for lunar lander environment[please refer to it for PPO explanation]. Only differences are the number of state variables passed into the model and number of action variables outputted. Also added a tensorboard for recording episodic rewards. 

In simple terms:  
For each episode, get state of robot > pass to model to get action > robot implements action to get new state and reward > record all the relevant stuff for backprop later > run backprop at end of episode for 4 times.  

What you should see:  
Robot moves forward slightly > turns counterclockwise[to face up] > moves all the way up > then does some weird zigzag pattern while trying to move down

Conclusion:  
Simulation is hard. Trying it on a prebuilt env[farama gym] removes the complexity of building an env from scratch. Here, we built a custom env and with it comes a whole slew of new problems. Not only do you need a good GPU, your code needs to accurately reflect the physics as well. In addition, robot design is a whole new challenge. I hope to use this project as a basis for future simulation RL projects.






