### Prompt
* Spies from the Rebel Alliance have stolen the plans to the Death Star and secretly transmitted them somewhere.  Maybe you can use these transmissions to uncover secret supporters of the rebels. 
* The rebels sent the plans in a series of n separate transmissions (cause the Death Star is, like, big), and because the transmissions were done secretly, you don't know exactly when each transmission was sent, but you have a good guess.
* For the _i_th transmission, you know it was sent at time ti +/- ei where ei is the error factor. For some transmissions, you had the spy under surveillance so the ei is rather small, but for some transmissions you only discovered about it after the fact so the ei value in those transmissions is quite large.
* Darth Vader recently captured a ship of Princess Leia. Searching the computer on board turned up no Death Star plans, but the ship suspiciously received n transmissions, and from the log you can see the exact times those transmissions arrived: x1, x2, ... xn. You would love to blow up Leia's home planet of Alderon as punishment, but there is due process in the Empire.  That means, before you blow up the planet, you must give strong evidence that she received the stolen transmissions. You will do that by matching each transmission time xi arrival on Leia's ship with some transmission time tj of the spies. Formally, xi matches to tj if
* tj -ej $\le$ x $\le$ tj + ej
* Give an algorithm (with proof and running time justification) that finds such a matching if one exists.  We want the punishment to come quickly so the algorithm should be fast: O(n^2) at worst.

### Algorithm
* Given:
	1. M = list of transmissions
	2. N = list of transmission in accurate order.
	3. i = transmission
	4. ei = error factor of transmission i
	5. ti = approximate time of transmission i
* Goal:
	* The goal is to prove that every transmission i in N is within ti +/- ei of its M counterpart.
* Algorithm:
	1. Sort the transmission times of the spies in ascending order.
	2. Initialize a bool array, keeping track of which transmissions times already have a ‘received’ counter part.
	3. For each transmission time xi found on Leia’s ship;
		1. Initialize a variable ‘found’ as false.
		2. Use Binary search to find the largest tj in the sorted list of transmission times, such that tj satisfies (tj <= xi+ej).
		3. Starting from tj, iterate backwards through the sorted list of transmission times of the spies and:
		4. If (tj-ej <= xi <= tj+ej) & (tj has not yet been matched), set found = true and break out of the loop.
		5. If found == false, there is no matching transmission time for xi, return false.
	4. If we reach this point, all transmission times have found their match, and we can return true.
### Proof of Concept
* Preventing duplicate matches:
	* We maintain an invariant ‘found’ to ensure that every transmission is matched one-to-one.
* if there is a match:
	* For each transmission, we look for the biggest tj that could match. This works best in this scenario because the transmission list is sorted in ascending order.
	* Then Iterating backwards through the sorted list, the first match must satisfy (tj-ej <= xi <= tj+ej)
* If there is no match:
	* The algorithm detects this through the use of the invariant, so that when a loop terminates because of no iterables left, it defaults to condition not satisfied.
### Running Time
* The worst case running time of the algo is O(n^2), however in practice it should be faster due to binary search optimizations and early terminations if no matching transmission is found.
