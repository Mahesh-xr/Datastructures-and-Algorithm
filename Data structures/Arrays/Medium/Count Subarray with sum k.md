
**Problem Statement:** Given an array of integers and an integer k, return the total number of subarrays whose sum equals k.

A subarray is a contiguous non-empty sequence of elements within an array.

**Pre-requisite:** [Longest subarray with given sum](https://takeuforward.org/data-structure/longest-subarray-with-given-sum-k/)

**Examples**

**Example 1:**
**Input Format:** N = 4, array[] = {3, 1, 2, 4}, k = 6
**Result:** 2
**Explanation:** The subarrays that sum up to 6 are [3, 1, 2] and [2, 4].

**Example 2:**
**Input Format:** N = 3, array[] = {1,2,3}, k = 3
**Result:** 2
**Explanation:** The subarrays that sum up to 3 are [1, 2], and [3].

Brute Force Approach

Algorithm / Intuition

The steps are as follows:

1. First, we will run a loop(**_say i_**) that will select every possible starting index of the subarray. The possible starting indices can vary from index 0 to index n-1(n = size of the array).
2. Inside the loop, we will run another loop(say j) that will signify the ending index of the subarray. For every subarray starting from the index i, the possible ending index can vary from index i to n-1(n = size of the array).
3. After that for each subarray starting from index i and ending at index j **(i.e. arr[i….j]),** we will run another loop to calculate the sum of all the elements(of that particular subarray).
4. After calculating the sum, we will check if the sum is equal to the given k. If it is, we will increase the value of the count.

**Intuition:** We will check the sum of every possible subarray and count how many of them are equal to k. To get every possible subarray sum, we will be using three nested loops. The first two loops(say i and j) will iterate over every possible starting index and ending index of a subarray. Basically, in each iteration, the subarray range will be from index i to index j. Using another loop we will get the sum of the elements of the subarray **[i…..j]**. Among all values of the sum calculated, we will only consider those that are equal to k.

**Note:** We are selecting every possible subarray using two nested loops and for each of them, we add all its elements using another loop.

**Note:** _For a better understanding of intuition, please watch the video at the bottom of the page._

Code

C++JavaPython

```java

import java.util.*;

public class tUf {
    public static int findAllSubarraysWithGivenSum(int arr[], int k) {
        int n = arr.length; // size of the given array.
        int cnt = 0; // Number of subarrays:

        for (int i = 0 ; i < n; i++) { // starting index i
            for (int j = i; j < n; j++) { // ending index j

                // calculate the sum of subarray [i...j]
                int sum = 0;
                for (int K = i; K <= j; K++)
                    sum += arr[K];

                // Increase the count if sum == k:
                if (sum == k)
                    cnt++;
            }
        }
        return cnt;
    }

    public static void main(String[] args) {
        int[] arr = {3, 1, 2, 4};
        int k = 6;
        int cnt = findAllSubarraysWithGivenSum(arr, k);
        System.out.println("The number of subarrays is: " + cnt);
    }
}
```

**Output:** The number of subarrays is: 2

Complexity Analysis

**Time Complexity:** O(N3), where N = size of the array.  
**Reason:** We are using three nested loops here. Though all are not running for exactly N times, the time complexity will be approximately O(N3).

**Space Complexity:** O(1) as we are not using any extra space.

Better Approach

Algorithm / Intuition

The steps are as follows:

1. First, we will run a loop(say i) that will select every possible starting index of the subarray. The possible starting indices can vary from index 0 to index n-1(n = array size).
2. Inside the loop, we will run another loop(say j) that will signify the ending index as well as the current element of the subarray. For every subarray starting from the index i, the possible ending index can vary from index i to n-1(n = size of the array).
3. Inside loop j, we will add the current element to the sum of the previous subarray i.e. **sum = sum + arr[j]**. 
4. After calculating the sum, we will check if the sum is equal to the given k. If it is, we will increase the value of the count.

**Intuition:** If we carefully observe, we can notice that to get the sum of the current subarray we just need to add the current element(i.e. **arr[j]**) to the sum of the previous subarray i.e. **arr[i….j-1]**.

Assume previous subarray = **arr[i……j-1]**current subarray = **arr[i…..j]**Sum of **arr[i….j]** = **(sum of arr[i….j-1]) + arr[j]**

This is how we can remove the third loop and while moving j pointer, we can calculate the sum.

**Note:** _For a better understanding of intuition, please watch the video at the bottom of the page._

Code

C++JavaPython

```java

import java.util.*;

public class tUf {
    public static int findAllSubarraysWithGivenSum(int arr[], int k) {
        int n = arr.length; // size of the given array.
        int cnt = 0; // Number of subarrays:

        for (int i = 0 ; i < n; i++) { // starting index i
            int sum = 0;
            for (int j = i; j < n; j++) { // ending index j
                // calculate the sum of subarray [i...j]
                // sum of [i..j-1] + arr[j]
                sum += arr[j];

                // Increase the count if sum == k:
                if (sum == k)
                    cnt++;
            }
        }
        return cnt;
    }

    public static void main(String[] args) {
        int[] arr = {3, 1, 2, 4};
        int k = 6;
        int cnt = findAllSubarraysWithGivenSum(arr, k);
        System.out.println("The number of subarrays is: " + cnt);
    }
}  
```

**Output:** The number of subarrays is: 2

Complexity Analysis

**Time Complexity:** O(N2), where N = size of the array.  
**Reason:** We are using two nested loops here. As each of them is running for exactly N times, the time complexity will be approximately O(N2).

**Space Complexity:** O(1) as we are not using any extra space.

Optimal Approach

Algorithm / Intuition

In this approach, we are going to use the concept of the prefix sum to solve this problem. Here, the prefix sum of a subarray ending at index i simply means the sum of all the elements of that subarray.

**Observation:**

Assume, the prefix sum of a subarray ending at index i is **x**. In that subarray, we will search for another subarray ending at index i, whose sum equals **k**. Here, we need to observe that if there exists another subarray ending at index i with sum k, then the prefix sum of the rest of the subarray will be **x-k**. The below image will clarify the concept:

![](https://static.takeuforward.org/wp/uploads/2023/04/countSubarray.jpg)

Now, _for a subarray ending at index i with the prefix sum x, if we remove the part with the prefix sum x-k, we will be left with the part whose sum is equal to k. And that is what we want._

Now, there may exist multiple subarrays with the prefix sum x-k. So, the number of subarrays with sum k that we can generate from the entire subarray ending at index i, is exactly equal to the number of subarrays with the prefix sum x-k, that we can remove from the entire subarray.

That is why, instead of searching the subarrays with sum k, we will keep the occurrence of the prefix sum of the subarrays using a map data structure. 

In the map, we will store every prefix sum calculated, with its occurrence in a <key, value> pair. Now, at index i, we just need to check the map data structure to get the number of times that the subarrays with the prefix sum x-k occur. Then we will simply add that number to our answer.

We will apply the above process for all possible indices of the given array. The possible values of the index i can be from 0 to n-1(where n = size of the array).

**Approach:**

The steps are as follows:

1. First, we will declare a map to store the prefix sums and their counts.
2. Then, we will set the value of 0 as 1 on the map.
3. Then we will run a loop(say i) from index 0 to n-1(n = size of the array).
4. For each index i, we will do the following:
    1. We will add the current element i.e. arr[i] to the prefix sum.
    2. We will calculate the prefix sum i.e. x-k, for which we need the occurrence.
    3. We will add the occurrence of the prefix sum x-k i.e. mpp[x-k] to our answer.
    4. Then we will store the current prefix sum in the map increasing its occurrence by 1.

**Question: Why do we need to set the value of 0?**Let’s understand this using an example. Assume the given array is [3, -3, 1, 1, 1] and k is 3. Now, for index 0, we get the total prefix sum as 3, and k is also 3. So, the prefix sum of the remove-part should be x-k = 3-3 = 0. Now, if the value is not previously set for the key 0 in the map, we will get the default value 0 for the key 0 and we will add 0 to our answer. This will mean that we have not found any subarray with sum 3 till now. But this should not be the case as index 0 itself is a subarray with sum k i.e. 3.  
So, in order to avoid this situation we need to set the value of 0 as 1 on the map beforehand.

**Note:** _For a better understanding of intuition, please watch the video at the bottom of the page._

Code

C++JavaPython

```java

import java.util.*;

public class tUf {
    public static int findAllSubarraysWithGivenSum(int arr[], int k) {
        int n = arr.length; // size of the given array.
        Map mpp = new HashMap();
        int preSum = 0, cnt = 0;

        mpp.put(0, 1); // Setting 0 in the map.
        for (int i = 0; i < n; i++) {
            // add current element to prefix Sum:
            preSum += arr[i];

            // Calculate x-k:
            int remove = preSum - k;

            // Add the number of subarrays to be removed:
            cnt += mpp.getOrDefault(remove, 0);

            // Update the count of prefix sum
            // in the map.
            mpp.put(preSum, mpp.getOrDefault(preSum, 0) + 1);
        }
        return cnt;
    }

    public static void main(String[] args) {
        int[] arr = {3, 1, 2, 4};
        int k = 6;
        int cnt = findAllSubarraysWithGivenSum(arr, k);
        System.out.println("The number of subarrays is: " + cnt);
    }
}
```

**Output:** The number of subarrays is: 2

Complexity Analysis

**Time Complexity:** O(N) or O(N*logN) depending on which map data structure we are using, where N = size of the array.  
**Reason:** For example, if we are using an unordered_map data structure in C++ the time complexity will be O(N) but if we are using a map data structure, the time complexity will be O(N*logN). The least complexity will be O(N) as we are using a loop to traverse the array.

**Note:** _To know more about maps, please refer to this:_ [_Hashing | Maps | Time Complexity | Collisions | Division Rule of Hashing | Strivers A2Z DSA Course_](https://takeuforward.org/hashing/hashing-maps-time-complexity-collisions-division-rule-of-hashing-strivers-a2z-dsa-course/)_._

**Space Complexity:** O(N) as we are using a map data structure.

Video Explanation

> _Special thanks to **[**KRITIDIPTA GHOSH**](https://www.linkedin.com/in/kriti39/)** for contributing to this article on takeUforward._

![TakeUForward Ad](https://static.takeuforward.org/content/v3_banner.png)