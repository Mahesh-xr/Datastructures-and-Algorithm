
Algorithm / Intuition

### **Optimizing the Brute Force Approach (Using two loops)**: 

### **Approach:**

The steps are as follows:

1. First, we will run a loop(_say i_) that will select every possible starting index of the subarray. The possible starting indices can vary from index 0 to index n-1(n = array size).
2. Inside the loop, we will run another loop(say j) that will signify the ending index as well as the current element of the subarray. For every subarray starting from the index i, the possible ending index can vary from index i to n-1(n = size of the array).
3. Inside loop j, we will add the current element to the sum of the previous subarray i.e. **sum = sum + arr[j]**. 
4. If the sum is equal to k, we will consider its length i.e. (j-i+1). Among all such subarrays with sum k, we will consider the one with the maximum length by comparing all the lengths.

**Intuition:** If we carefully observe, we can notice that to get the sum of the current subarray we just need to add the current element(i.e. **arr[j]**) to the sum of the previous subarray i.e. **arr[i….j-1]**.

Assume previous subarray = **arr[i……j-1] **current subarray = **arr[i…..j]**Sum of **arr[i….j]** = **(sum of arr[i….j-1]) + arr[j]**

This is how we can remove the third loop and while moving the j pointer, we can calculate the sum.

**Note:** _For a better understanding of intuition, please watch the video at the bottom of the page._

Code



```java



import java.util.*;

public class tUf {
    public static int getLongestSubarray(int []a, long k) {
        int n = a.length; // size of the array.

        int len = 0;
        for (int i = 0; i < n; i++) { // starting index
            long s = 0; // Sum variable
            for (int j = i; j < n; j++) { // ending index
                // add the current element to
                // the subarray a[i...j-1]:
                s += a[j];

                if (s == k)
                    len = Math.max(len, j - i + 1);
            }
        }
        return len;
    }

    public static void main(String[] args) {
        int[] a = {2, 3, 5, 1, 9};
        long k = 10;
        int len = getLongestSubarray(a, k);
        System.out.println("The length of the longest subarray is: " + len);
    }
}
```

Output: The length of the longest subarray is: 3

Complexity Analysis

**Time Complexity:** O(N2) approx., where N = size of the array.  
**Reason:** We are using two nested loops, each running approximately N times.

**Space Complexity:** O(1) as we are not using any extra space.



### **Better Approach(Using Hashing)**: 

### **Approach:**

The steps are as follows:

1. First, we will declare a map to store the prefix sums and the indices.
2. Then we will run a loop(say i) from index 0 to n-1(n = size of the array).
3. For each index i, we will do the following:
    1. We will add the current element i.e. a[i] to the prefix sum.
    2. If the sum is equal to k, we should consider the length of the current subarray i.e. i+1. We will compare this length with the existing length and consider the maximum one.
    3. We will calculate the prefix sum i.e. x-k, of the remaining subarray.
    4. If that sum of the remaining part i.e. x-k exists in the map, we will calculate the length i.e. i-preSumMap[x-k], and consider the maximum one comparing it with the existing length we have achieved until now.
    5. If the sum, we got after step 3.1, does not exist in the map we will add that with the current index into the map. We are checking the map before insertion because we want the index to be as minimum as possible and so we will consider the earliest index where the sum x-k has occurred. [_Detailed discussion in the edge case section_]

In this approach, we are using the concept of the prefix sum to solve this problem. Here, the prefix sum of a subarray ending at index i, simply means the sum of all the elements of that subarray.

**Observation:** Assume, the prefix sum of a subarray ending at index i is **x**. In that subarray, we will search for another subarray ending at index i, whose sum equals **k**. Here, we need to observe that if there exists another subarray ending at index i with sum k, then the prefix sum of the rest of the subarray will be **x-k**. The below image will clarify the concept:

![](https://static.takeuforward.org/wp/uploads/2023/04/Screenshot-2023-04-13-004939.png)

Now, _for a subarray ending at index i with the prefix sum x, if we remove the part with the prefix sum x-k, we will be left with the part whose sum is equal to k. And that is what we want._

That is why, instead of searching the subarrays with sum k, we will keep track of the prefix sum of the subarrays generated at every index using a map data structure. 

In the map, we will store every prefix sum calculated, with its index in a <key, value> pair. Now, at index i, we just need to check the map data structure to get the index i.e. preSumMap[x-k] where the subarrays with the prefix sum x-k occur. Then we will simply subtract the index i.e. preSumMap[x-k] from the current index i to get the length of the subarray i.e. len = i -preSumMap[x-k].

We will apply the above process for all possible indices of the given array. The possible values of the index i can be from 0 to n-1(where n = size of the array).

**Edge Case: Why do we need to check the map if the prefix sum already exists?** In the algorithm, we have seen that at step 3.4, we are checking the map if the prefix sum already exists, and if it does we are not updating it. Let’s understand the reason by considering the following example:  
Assume the given array is {2, 0, 0, 3}. If we apply the algorithm to the given array without checking, it will be like the following:

![](https://static.takeuforward.org/wp/uploads/2023/04/Screenshot-2023-04-13-005443.png)

In steps 2 and 3 the element at index i is 0. So, in those steps, the prefix sum remains the same but the index is getting updated in the map. Now, when index i reaches the end, it calculates the length i.e. i-preSumMap[rem] = 3-2 = 1. Here it is considering only the subarray [3] which is incorrect as the longest subarray we can get is [0, 0, 3] and hence the length should be 3.

Now, to avoid this edge case i.e. to maximize the calculated length, we need to observe the formula we are using to calculate the length i.e. len = i - preSumMap[rem].

Now, if we minimize the term preSumMap[rem] (_i.e. the index where the subarray with sum x-k ends_), we will get the maximum length. That is why we will consider only the first or the leftmost index where the subarray with sum x-k ends. After that, we will not update that particular index even if we get a similar subarray ending at a later index.

So, we will check the map before inserting the prefix sum. If it already exists in the map, we will not update it but if it is not present, we will insert it for the first time.

```java
import java.util.*;

public class tUf {
    public static int getLongestSubarray(int []a, long k) {
        int n = a.length; // size of the array.

        Map<Long, Integer> preSumMap = new HashMap<>();
        long sum = 0;
        int maxLen = 0;
        for (int i = 0; i < n; i++) {
            //calculate the prefix sum till index i:
            sum += a[i];

            // if the sum = k, update the maxLen:
            if (sum == k) {
                maxLen = Math.max(maxLen, i + 1);
            }

            // calculate the sum of remaining part i.e. x-k:
            long rem = sum - k;

            //Calculate the length and update maxLen:
            if (preSumMap.containsKey(rem)) {
                int len = i - preSumMap.get(rem);
                maxLen = Math.max(maxLen, len);
            }

            //Finally, update the map checking the conditions:
            if (!preSumMap.containsKey(sum)) {
                preSumMap.put(sum, i);
            }
        }

        return maxLen;
    }

    public static void main(String[] args) {
        int[] a = {2, 3, 5, 1, 9};
        long k = 10;
        int len = getLongestSubarray(a, k);
        System.out.println("The length of the longest subarray is: " + len);
    }
}
```


**Time Complexity:** O(N) or O(N*logN) depending on which map data structure we are using, where N = size of the array.  
**Reason:** For example, if we are using an unordered_map data structure in C++ the time complexity will be O(N)(_though in the worst case, unordered_map takes O(N) to find an element and the time complexity becomes O(N2)_) but if we are using a map data structure, the time complexity will be O(N*logN). The least complexity will be O(N) as we are using a loop to traverse the array.

**Space Complexity:** O(N) as we are using a map data structure.


### **Optimal Approach (Using 2 pointers)**: 

### **Approach:**

The steps are as follows:

1. First, we will take two pointers: left and right, initially pointing to the index 0.
2. The sum is set to a[0] i.e. the first element initially.
3. Now we will run a while loop until the right pointer crosses the last index i.e. n-1.
4. Inside the loop, we will do the following:
    1. We will use another while loop and it will run until the sum is lesser or equal to k.
        1. Inside this second loop, from the sum, we will subtract the element that is pointed by the left pointer and increase the left pointer by 1.
    2. After this loop gets completed, we will check if the sum is equal to k. If it is, we will compare the length of the current subarray i.e. (right-left+1) with the existing one and consider the maximum one.
    3. Then we will move forward the right pointer by 1. If the right pointer is pointing to a valid index(< n) after the increment, we will add the element i.e. a[right] to the sum.
5. Finally, we will return the maximum length.

**Intuition:** We are using two pointers i.e. left and right. The left pointer denotes the starting index of the subarray and the right pointer denotes the ending index. Now as we want the longest subarray, we will move the right pointer in a forward direction every time adding the element i.e. a[right] to the sum. But when the sum of the subarray crosses k, we will move the left pointer in the forward direction as well to shrink the size of the subarray as well as to decrease the sum. Thus, we will consider the length of the subarray whenever the sum becomes equal to k.  
The below dry run will clarify the intuition.

### **Dry run:**

![](https://static.takeuforward.org/wp/uploads/2023/04/Screenshot-2023-04-13-012512.png)


```java
import java.util.*;

public class Main {
    public static int getLongestSubarray(int []a, long k) {
        int n = a.length; // size of the array.

        int left = 0, right = 0; // 2 pointers
        long sum = a[0];
        int maxLen = 0;
        while (right < n) {
            // if sum > k, reduce the subarray from left
            // until sum becomes less or equal to k:
            while (left <= right && sum > k) {
                sum -= a[left];
                left++;
            }

            // if sum = k, update the maxLen i.e. answer:
            if (sum == k) {
                maxLen = Math.max(maxLen, right - left + 1);
            }

            // Move forward thw right pointer:
            right++;
            if (right < n) sum += a[right];
        }

        return maxLen;
    }

    public static void main(String[] args) {
        int[] a = {2, 3, 5, 1, 9};
        long k = 10;
        int len = getLongestSubarray(a, k);
        System.out.println("The length of the longest subarray is: " + len);
    }
}
```

Complexity Analysis

**Time Complexity:** O(2*N), where N = size of the given array.  
**Reason:** The outer while loop i.e. the right pointer can move up to index n-1(_the last index_). Now, the inner while loop i.e. the left pointer can move up to the right pointer at most. So, every time the inner loop does not run for n times rather it can run for n times in total. So, the time complexity will be O(2*N) instead of O(N2).

**Space Complexity:** O(1) as we are not using any extra space.

**Space Complexity:** O(1) as we are not using any extra space.