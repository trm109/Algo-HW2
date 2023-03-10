### Prompt
* You decide to start a pizzeria business in your dorm.  Because there is no dining area, this will be a pick-up only business. You create an app where customers can choose their toppings and give a time when the pizza should be ready.  
* You have a **"ready on time or it is free"** guarantee, and you coded your app so that it would not let the customers enter unreasonable times. You finish class, and on the way back to the dorm, you check your business page, **and to your shock you are already flooded with orders. Too many to possibly make on time.**  
* Here is your dilemma.  **You have _n_ pizza orders.**  Each order has a **value _vi_** which is the amount you are charging for that pizza and a **pickup time _ti_** when it has to be done by or you are giving it away for free.  
* You can only make one pizza at a time (the dorm has a small oven), and it takes the same amount of time to make each pizza.  
* **Design an algorithm that tells you the order you should make the pizzas in order to minimize the total value of the pizzas you are giving away for free.**
### Algorithm
* Variables:
	* n = list of pizza orders.
	* i = pizza order i
	* vi = value of order i
	* ti = deadline of order i
* Algorithm:
	1. Sort the orders by their value in descending order.
	2. Initialize tracker variables for:
		1. `total_value_lost` you have lost
	3. For each order:
		1. if its pickup time is possible given the current state of the oven:
			1. Make the pizza
		2. else:
			1. Add the value of the pizza we skipped to `total_value_lost`
		3. move onto next order.
	4. Once all orders have been processed, return `total_value_lost`
### Proof of Correctness
* Proof via contradiction:
	* Setup:
		* Suppose there exists an optimal solution O that differs from our greedy solution G.
		* Let i and j be the first two orders that **differ** in the two schedules, where the optimal solution processes j before i, and our greedy solution processes i before j.
		* Without Loss of Generality, that i has a greater value tha j. 
		* If order i is not ready by ti, it will be given away, vice versa for j.
	* Division into cases:
		* Case 1; $ti \le tj:$
			* If $ti \le tj$, then order i can be made before order j without causing a delay.
			* $\therefore$ the greedy algorithm correctly processes order i before j.
			* There is no contradiction
		* Case 2; $ti \textgreater tj$ :
			* then order j can be made before order i without causing a delay. If the optimal solution processes j before i, then both orders will be ready by their respective deadlines. However, if the greedy alogirthm processes order i before j, then order i may not be ready in time.
			* Since vi > vj, giving away i incurs a higher cost than giving away j. $\therefore$ if the optimal solution exsists, it must process j before i, **which contradicts our assumption that G produces a suboptimal solution**
		* $\therefore$ Greedy produces an optimal solution, and it always schedules the pizza such that the total value of pizza given away is minimal.
### Running Time
* The sorting step takes O(nlogn)
* The 'For Each' step takes O(n)
* O(nlogn) + O(n) = **O(nlogn)**
