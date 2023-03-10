### Prompt
* Bob has a set, A, of n nuts and a set, B, of n bolts, such that each nut has a unique matching bolt. 
* Unfortunately, the nuts in A all look the same, and the bolts in B all look the same, as well. The only comparison that Bob can make is to take a nut-bolt pair (a, b), such that a is in A and b is in B, and test if the threads of a are larger, smaller, or a perfect match with the threads of b. 
* Describe an efficient algorithm for Bob to match up all of his nuts and bolts. What is the running time of the algorithm? Justify the running time and prove that your algorithm is correct.
### Algorithm
* In order to match up all the nuts and bolts, we can employ a modified quicksort. By selecting any pivot nut from set A, and comparing its thread with each bolt in set B, we can partition the bolts into:
	1. Bolts too small for pivot nut.
	2. Bolts too big for pivot nut.
	3. Bolts same size as pivot nut.
* Once the partitioning is done, the algorithm recursively applies the same processes to the two sets of nuts and bolts that correspond to the bolts that were too large or small for the pivot nut.
* Psuedo code:
```python
match_nuts_and_bolts(A, B):
	#check base case.
    if length(A) == 1:
        return (A[0], B[0])
	pivot_nut = select_random_element(A) #grab a random pivot point.
	smaller_bolts = [] #init smaller partition
	larger_bolts = [] #init larger partition
	matching_bolt = None #init equal sized bolt.
	#for each bolt O(B)
	for bolt in B:
		if threads(bolt) < threads(pivot_nut): #if smaller
			smaller_bolts.append(bolt)
		elif threads(bolt) > threads(pivot_nut): #if larger
			larger_bolts.append(bolt)
		else: #if just right.
			matching_bolt = bolt
	#Then check these nuts
	matching_nut = pivot_nut
	#for each nut O(A)
	for nut in A:
		if nut != pivot_nut and threads(nut) == threads(matching_bolt): #if the threads of the nut match the matching bot.
			matching_nut = nut #we found a matching nut.
			break
	#recurse.
	left_pairs = match_nuts_and_bolts(smaller_nuts, smaller_bolts)
	right_pairs = match_nuts_and_bolts(larger_nuts, larger_bolts)
	#returns matching nuts and bolts.
	return left_pairs + [(matching_nut, matching_bolt)] + right_pairs
```
### Proof of Correctness
* The algorithm produces a valid array of matching nut-bolt pairs by first partitioning the bolts based on their threads, relative to a randomly selected pivot nut. It then recurses, applying the same processes to two subsets and so on. The key observation is that if a nut matches a bolt, they must have been properly identified and placed into the same partition. This is enforced because the only way a nut and bolt can be in different partitions is if their threads are larger or smaller than the pivot nut's threads. 
* $\therefore$ After each recursion, the algorithm correctly identifies the matching nut-bolt pair within the same partition, resulting in a valid complete solution.
### Running Time
* Partitioning of the bolts takes O(n) time, and recursively calls the same processes to the smaller set of nuts and bolts in O(logn) time.
* O(n) * O(logn) = **O(nlogn)**