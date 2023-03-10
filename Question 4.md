### Prompt
* One inefficiency in the quicksort algorithm I gave in lecture is that it placed all the values greater than or equal to the pivot after the pivot, and then we recursed on array indeces from the pivot index + 1 to the end. 
* The problem is that if there are duplicate elements to the pivot, we just left them anywhere to the right of the pivot and they were included in the recursive call.
* A better solution would be to grab all the elements equal to the pivot and place them next to the pivot in the middle of the array, and so after the partition of the array, we have all elements smaller than the pivot, all elements equal to the pivot, and then all elements larger than the pivot. 
* Then we only recurse on the elements smaller or larger than the pivot.  Here is a new partition routine that hopefully achieves that on the elements in array A between index start and index end.  Use a loop invariant proof to prove that this routine is correct.

```python
i := start 
j := start 
k := end
pivot := A[start]
    
while j <= k:
 if A[j] < pivot:
 swap A[i] and A[j]
 i ← i + 1
 j ← j + 1
else if A[j] > pivot:
 swap A[j] and A[k]
 k ← k - 1
else:
 j ← j + 1
```
### Algorithm
* At the start of each iteration of the while loop, the following holds true;
	1.	All elements in A\[start:i] are < pivot.
	2.	All elements in A\[i:j] are == pivot
	3.	All elements in A\[k+1:end] are > pivot
	4.	All elements in A\[j:k] have not yet been processed.
* Initialization:
	* At the start of the loop:
		1. A\[start:i] is empty.
		2. A\[k+1:end] is empty.
		3. A\[j:k] is unprocessed.
		4. The pivot element is he first element ‘A\[start]’, so all elements in A\[i:j] are == pivot.
* Maintenance:
	* Case 1, A\[j] < pivot:
		* I. Swap A\[i] & A\[j]
		* II. i++, j++
		* The loop invariant is maintained:
			* A\[i] is < pivot, 
			* A\[j] >= pivot, 
			* so all elements A\[i:j] == pivot.
	* Case 2, A\[j] > pivot:
		* I. Swap A\[j] & A\[k]
		* II. k--
		* The loop invariant is maintained:
			* A\[j] > pivot,
			* A\[k+1:end] > pivot,
			* A\[start:i] < pivot,
			* A\[i:j] == pivot.
	* Case 3, A\[j] == pivot:
		* I. j++
		* The loop invariant is maintained:
			* A\[start:i] < pivot,
			* A\[k+1:end] > pivot,
			* A\[i:j+1] == pivot.
* Termination:
	* The loop terminates when j > k, All elements in A\[start:i], A\[i:j], and A\[k+1:end] are properly partitioned.
	* Then, we recursively sort A\[start:i] & A\[k+1:end] to sort elements smaller and larger than pivot.
### Proof of Correctness
* Conclusion:
	* The loop invariant holds true when initialized, remains true during the iteration, and holds true when the program is terminating, so our loop invariant is valid.
### Running Time
* Not Applicable.
