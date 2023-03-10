## Prompt 
* You are scheduling a telescope to view astronomical events. Suppose the telescope can move **between -85deg; and +85deg** and the telescope can move **1deg in either direction per minute.** 

* **You are given a list of n celestial events.** Each event has a time (in minutes since the sunset) and a location in degrees in the sky. If the telescope is pointing at that location at that minute, it can take a picture of the event. Otherwise it misses the event.

* Assume that taking a picture takes 1 minute and the telescope does not move during that time. 

* Given the n events, the location and time of each event, and assuming the telescope begins the evening at position 0, give an efficient algorithm that finds a schedule that photographs the maximum number of events such that the last event in the list is photographed. Prove your algorithm is correct. State and justify the running time.

## Algorithm #
* ### Setup
	* Let be the list of celestial events, sorted by their times in ascending order.
	* Let P be the 2D array of size \[n+1]\[85+1+85]
	* P\[i]\[j] will denote the maximum number of events that can be captured from the first event i, given that the telescope is currently pointing at position j
* ### Running
	* Will we start filling in P from the bottom up:
		* For each event i, we have two cases:
			1. We do not capture event i:
				* P\[i]\[j] = p\[i-1]\[j]
			2. We capture event i:
				* First, we need to find the position j' (if it is possible to reach), such that the telescope can move from its current position, j, to the new position j' (where event i is happening).
				* Then, find the max:
					 P\[i]\[j'] = max(
						P\[i-1]\[j],
						P\[i-1]\[j'] + 1
					 )
					* This means that the max number of events that can be captured from the first i events with the telescope at position j' is the max of:
						* The max # of captured events in the first i-1 events, w/ telescope at position j, without capturing event i.
						* The max # of captured events in the firs ti-1 events, w/ telescope at pos j', WITH capturing event i 
	* Using this algorithm, the last populated index of DP Table ‘P’, P\[n]\[85] holds the max number of events you can captured that night.
	* Our algorithm works by considering each event one by one in ascending order, then for each event, considers each degree of movement.

## Proof of Correctness #
1. Proof of optimal substructure:
	* Our subproblem is finding the max # of captured events at the first i events, given that the telescope is pointing at j. The optimal solution is defined as P\[i]\[j].
	* Consider the last event in the Optimal solution, ‘k’. Since this event is in the optimal solution, we know that it must be captured. So lets split the problem into two subproblems:
		1. Find the max # of captured events from 0 to k-1:
			* Using our optimal substructure, the optimal solution is p\[k-1]\[j’] for some j’.
		2. Find the max # events that can be captured from k onwards.
			* Using our optimal substructure, the optimal substructure is P\[n]\[k.position], where k.position is the position in degrees of event k.
	* Therefore, the optimal solution is the sum of the optimal solutions of subproblems 1 & 2: P\[k-1]\[j’] + 1. The optimal value of j’ is found by iterating over all possible positions of j that satisfy |j – j’| <= t, where t = time needed to move from j to j’.
2. Greedy Choice property:
	* We must prove that the locally optimal choice leads to a globally optimal solution. In this problem, we make a locally optimal choice by choosing to either capture the event or not. If we decide to capture event i, we need to choose the position j’ that allows us to maximize the # of events that can be capture FROM the first i events. This choice is locally optimal because it maximizes the # of captured events from the first i events, without considering future events.
* Since 1 & 2 are satisfied, this dynamic programming approach will always provide an optimal solution.
## Time Complexity #
* The time complexity is O(nd^2) or O(n); For each index (nd indices), we must also check the 'degree' location of each event (d). However, d should be a constant, and also limited to 360, so basically just O(n).
