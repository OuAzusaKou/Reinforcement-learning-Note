
# Summary
***Reinforcement learning*** is learning what to do—how to map situations(***state***) to actions(***policy***)—so
as to maximize a numerical reward signal.  
There are two class method,  
one: we first evaluate value of state or state-action (***value function***) condition with current policy called ***policy evaluation***(prediction), then improve the policy through value function called ***policy improvement*** ,We use the term ***generalized policy iteration (GPI)*** to refer to the general idea of letting policy evaluation and policy improvement processes interact, independent of the granularity and other details of the two processes.   
**We must say that as long as both processes continue to update all states(this is a sufficient conditions ), the ultimate result is typically the same—convergence to the optimal value function and an optimal policy.**

## So the first problem is How to evaluate the value function.  
We have some methods discribe in the graph below.  

<p align="center">
  <img src="https://github.com/OuAzusaKou/Reinforcement-learning-Note/blob/master/img-folder/reinforce_note-1.png">
</p>
First we have the  
  
  
  <p align="center">
    <img src="https://github.com/OuAzusaKou/Reinforcement-learning-Note/blob/master/img-folder/CodeCogsEqn.png">
  </p>  

From this equation,we use sample inplace the expection,and estimate value function inplace the true value function,this will generate two error.   
and then we get some different methods upto the degree of inplace from two dimensions-width and deepth(bootstrap).  

Deepth:  
+ if we update more deepth, we will elimate more error when we complete one stage update by infinity sample.  
- we will need more experience for backup when we shift deeper.

Width:   
+ it will not elimate error when we complete one stage sample compare to same Deepth method,But it will elimate more error in once sample excution. 
-  we will need more envirment and policy information for update when we shift wider.  

And we will afford more computation once update when we shift to any direction and we will need less update-times.

Then making the policy greedy with respect to the value function or greedier(*$\varepsilon-greedy$*, of course there also have other policy improvement method) to improve the policy. So we get some method to get opimal policy for special problem.  
## But because we need to *continue update all states* ,So the Second problem is How can we continue update all states.
The Method is ES(***Exploration Starts***)

## But it is impossible for infinitely selected all states or state-action to update.So our target turn to we use less time get more optimal policy.
For this problem we use some policy to exploration (decide which state-action we need update,not randomly select as ES(even randomly select state-action relative with current policy ) )to speed-up the update.  

There are two approaches to ensuring this, resulting in what we call on-policy
methods and off-policy methods.

+ On-policy methods is the exploration policy(decision policy) is policy we evaluated and improved now.  

- Off-policy methods is the exploration policy is not policy we evaluated and improved. 


We need to get ***more accurate value*** at one state on ***more optimal policy*** quickly, It need we update value on policy that more greedy respect with current value to get more optimal policy. And update the state-action value that we just selected to get more accurate value respect with current policy.  

But it's not enough,we can't arrive optimal policy, because we need to update other state-action vaue(not greedy policy) to improve policy.(get more information about other state-action ).  

This called  trade off between ***exploration and exploitation***.

So get these algorithms.  
  
  
<table cellspacing="0" border="0">
	<colgroup width="85"></colgroup>
	<colgroup width="165"></colgroup>
	<colgroup width="203"></colgroup>
	<colgroup width="166"></colgroup>
	<colgroup width="137"></colgroup>
	<colgroup width="207"></colgroup>
	<tr>
		<td height="17" align="left"><br></td>
		<td align="left"><br></td>
		<td align="left"><font face="Liberation Sans">1-step</font></td>
		<td align="left"><font face="Liberation Sans">N-step </font></td>
		<td align="left"><font face="Liberation Sans">MC</font></td>
		<td align="left"><font face="Liberation Sans">DP</font></td>
	</tr>
	<tr>
		<td height="17" align="left"><font face="Liberation Sans">On-policy</font></td>
		<td align="left"><br></td>
		<td align="left"><font face="Liberation Sans">Sarsa(ε-greedy),Expected Sarsa</font></td>
		<td align="left"><font face="Liberation Sans">n-step Sarsa</font></td>
		<td align="left"><font face="Liberation Sans">On-policy MC control</font></td>
		<td align="left"><br></td>
	</tr>
	<tr>
		<td rowspan=2 height="34" align="left"><font face="Liberation Sans">Off-policy</font></td>
		<td align="left"><font face="Liberation Sans">importance sample</font></td>
		<td align="left"><br></td>
		<td align="left"><font face="Liberation Sans">Off-policy n-step Sarsa</font></td>
		<td align="left"><font face="Liberation Sans">Off-policy MC control</font></td>
		<td align="left"><br></td>
	</tr>
	<tr>
		<td align="left"><font face="Liberation Sans">not importance sample</font></td>
		<td align="left"><font face="Liberation Sans">Q-learning(ε-greedy)</font></td>
		<td align="left"><font face="Liberation Sans">n-step Tree Backup</font></td>
		<td align="left"><br></td>
		<td align="left"><br></td>
	</tr>
	<tr>
		<td height="17" align="left"><font face="Liberation Sans">ES</font></td>
		<td align="left"><br></td>
		<td align="left"><br></td>
		<td align="left"><br></td>
		<td align="left"><font face="Liberation Sans">MCES</font></td>
		<td align="left"><font face="Liberation Sans">Policy Iteration,Value Iteration</font></td>
</tr>  
</table> 
 
## There have other dimensions to speed up the process of getting optimal policy.  
 ### We can both update based on real experience(learning) and simulated experience(planning)(generate model by experience).  
 
  When we have some real experience, It's stupid only update knowledge for this state, we can also update knowledge(value) for any state relative with this state-   Drawing inferences about other cases from one instance.  
  
## And planning divide in two types,they have different focus.  
+ #### Background planning.  
  We planning tagert for optimal policy of entire states.  
    + So one obviously method is planning backward on the predecessors of states whose values have recently changed called ***Prioritized sweeping***.  
    Planning backward is benifit for optimizing policy on situation we have meeted.  
    + ***On-policy trajectory sampling*** focuses on states or state–action pairs that the agent is likely to encounter when controlling its environment.  
    Forward planning is benifit for optimizing policy on situation we will encounter. 
  
- #### Decision time planning  
  We planning tagert for optimal policy of one particular state (current state).  
    + The classical state-space planning methods in artificial intelligence are decision-time planning methods collectively known as ***heuristic search***.  
    + ***Rollout algorithms*** are decision-time planning algorithms based on Monte Carlo control applied to simulated trajectories that all begin at the current environment state.

# MCTS  
1. ***Selection***. Starting at the root node, a tree policy based on the action values
attached to the edges of the tree traverses the tree to select a leaf node.
2. ***Expansion***. On some iterations (depending on details of the application), the tree
is expanded from the selected leaf node by adding one or more child nodes reached
from the selected node via unexplored actions.
3. ***Simulation***. From the selected node, or from one of its newly-added child nodes (if
any), simulation of a complete episode is run with actions selected by the rollout
policy. The result is a Monte Carlo trial with actions selected first by the tree
policy and beyond the tree by the rollout policy.
4. ***Backup***. The return generated by the simulated episode is backed up to update,
or to initialize, the action values attached to the edges of the tree traversed by
the tree policy in this iteration of MCTS. No values are saved for the states and
actions visited by the rollout policy beyond the tree.  

The core idea of MCTS is that it use tree policy (usually UCB for trading off between exploration and exploitation )to explore (decide which sate we need to update and backup.). And improve the accuracy of estimated value on state which we first encounter by rollout policy.Then,it saves action-value estimates attached to the tree edges and updates them using reinforcement learning’s sample updates,this will benifit of using past experience to explore, and speed-up the update.

