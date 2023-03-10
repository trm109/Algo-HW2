### Prompt
* You are consulting for an intelligence agency whose jobs consist monitoring and analyzing electronic signals coming from ships in coastal Atlantic waters.
* They want a fast algorithm to "untangle" a superposition of two signals. Specifically, they are picturing a situation in which each of two ships is emitting a short sequence of 0's and 1's over and over, and they want to make sure that the signal they're hearing is an interleaving of these two emissions, with nothing extra added in.
* Given a string x consisting of 0s and 1s, we write xk to denote k copies of x concatenated together. We say that a string x' is a repetition of x if it is a prefix of xk for some number k. So x'=10110110110 is a repetition of x = 101.
* We say that a string s is an interleaving of x and y if its symbols can be partitioned into two (not necessarily contiguous) sequences s' and s'' so that s' is a repetition of x and s'' is a repetition of y. For example, if x = 101 and y= 00, then s=100010101 with s' = 101101 and s''=000.
* Give an efficient algorithm that takes strings s, x, and y and decides if s is an interleaving of x and y
### Algorithm
```python
def is_interleaving(s, x, y):
    #convert strings to lists
    s = list(s)
    x = list(x)
    y = list(y)
    #initialize indices
    x_index = 0
    y_index = 0
    x_debug_array = [] #Only used for debugging
    y_debug_array = [] #Only used for debugging.
    #check if s is interleaving of x and y
    for i in range(len(s)):
        #check if the current char of s is the same as the current char of x
        if(s[i] == x[x_index]):
            x_debug_array.append(x[x_index]) #for debugging only.
            x_index += 1 #increment x_index
            #check if x_index is out of bounds
            if(x_index == len(x)):
                x_index = 0 #begin recounting from the beginning of x
        #check if the current char of s is the same as the current char of y
        elif(s[i] == y[y_index]):
            y_debug_array.append(y[y_index]) #for debugging only.
            y_index += 1 #increment y_index
            #check if y_index is out of bounds
            if(y_index == len(y)):
                y_index = 0 #begin recounting from the beginning of y
            #
        else:
            return False
    #
    print("x_debug_array: ", x_debug_array)
    print("y_debug_array: ", y_debug_array)
    return True
```
### Proof of Correctness
* This method determines whether or not two functions are interleaving assuming two things: the rate of which the bits come from the boats is inconsistent, and both x and y are fully contained in s. 
* We can break the running conditions into three cases:
	1. Only X is interleaved in S: (X is chosen without loss of generality)
		* The method will return false (optimal)
		* At some point, the method will stop, as the current iterated character does not fit into either 'debug_arrays', meaning an unexpected string was interleaved instead of Y.
		* If this does not occur, it means the interleaved string was a symmetrical division or duplication of Y, OR one of the previous assumptions was violated, in which case there is no definitive answer.
			* A symmetrical division duplication of Y = '101101' would be:
				* Y' = '101'
				* or Y'' = '101101101'
			* Not catching a symmetrical division is not a fault of the method, it is a fault of the information we are provided.
	1. Both are interleaved in S:
		* The method will return true (optimal)
		* The solution will be able to perfectly populate the 'debug_arrays', meaning that the messages were properly interleaved.
	2. Neither are interleaved in S:
		* The method will return false (optimal)
		* Unless the interleaved messages were symmetrical divisions or duplications of both X and Y respectively, the method will be unable to properly population the 'debug_arrays', resulting in a bit that is unable to fit into an array, causing the method return false.
### Running Time
* The runtime is O(n), where n is the length of the string. The comparisons of the string is O(1) runtime, so they aren't considered when calculating the runtime.