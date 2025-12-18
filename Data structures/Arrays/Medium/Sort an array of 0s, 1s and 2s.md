
**Problem Statement:** Given an array consisting of only 0s, 1s, and 2s. Write a program to in-place sort the array without using inbuilt sort functions. ( Expected: Single pass-O(N) and constant space)

**Examples**

**Input:** nums = [2,0,2,1,1,0]
**Output**: [0,0,1,1,2,2]

**Input:** nums = [2,0,1]
**Output:** [0,1,2]

**Input:** nums = [0]
**Output:** [0]

**_Disclaimer_**: _Don’t jump directly to the solution, try it out yourself first._

Practice:[Solve Problem](https://takeuforward.org/plus/dsa/problems/sort-an-array-of-0's-1's-and-2's)

Brute Force Approach

Algorithm / Intuition

Sorting ( even if it is not the expected solution here but it can be considered as one of the approaches). Since the [array](https://takeuforward.org/c/multidimensional-arrays-in-cpp/) contains only 3 integers, 0, 1, and 2. Simply [sorting the array](https://takeuforward.org/data-structure/merge-sort-algorithm/) would arrange the elements in increasing order.

Complexity Analysis

**Time Complexity:** O(N*logN)

**Space Complexity:** O(1)

Better Approach

Algorithm / Intuition

Keeping count of values

**Intuition:** Since in this case there are only 3 distinct values in the [array](https://takeuforward.org/java/multidimensional-array-in-java/) so it's easy to maintain the count of all, Like the count of 0, 1, and 2. This can be followed by overwriting the array based on the frequency(count) of the values.

**Approach:** 

1. Take 3 variables to maintain the count of 0, 1 and 2.
2. Travel the array once and increment the corresponding counting variables

( let's consider **count_0 = a, count_1 = b, count_2 = c** )

3. In 2nd traversal of array, we will now over write the first ‘a’ indices / positions in array with ’0’, the next ‘b’ with ‘1’ and the remaining ‘c’ with ‘2’.

Code

C++JavaPython

```java

import java.util.*;

public class Main {
    public static void sortArray(ArrayList<Integer> arr, int n) {
        int cnt0 = 0, cnt1 = 0, cnt2 = 0;

        for (int i = 0; i < n; i++) {
            if (arr.get(i) == 0) cnt0++;
            else if (arr.get(i) == 1) cnt1++;
            else cnt2++;
        }

        //Replace the places in the original array:
        for (int i = 0; i < cnt0; i++) arr.set(i, 0); // replacing 0's

        for (int i = cnt0; i < cnt0 + cnt1; i++) arr.set(i, 1); // replacing 1's

        for (int i = cnt0 + cnt1; i < n; i++) arr.set(i, 2); // replacing 2's
    }

    public static void main(String args[]) {
        int n = 6;
        ArrayList<Integer> arr = new ArrayList<>(Arrays.asList(new Integer[] {0, 2, 1, 2, 0, 1}));
        sortArray(arr, n);
        System.out.println("After sorting:");
        for (int i = 0; i < n; i++) {
            System.out.print(arr.get(i) + " ");
        }
        System.out.println();

    }

}
```

**Output:**After sorting:  
0 0 1 1 2 2 Complexity Analysis

**Time Complexity:** O(N) + O(N), where N = size of the array. First O(N) for counting the number of 0’s, 1’s, 2’s, and second O(N) for placing them correctly in the original array.

**Space Complexity:** O(1) as we are not using any extra space.

Optimal Approach

Algorithm / Intuition

_This problem is a variation of the popular_ **_Dutch National flag algorithm_**_._ 

This algorithm contains 3 pointers i.e. low, mid, and high, and 3 main rules.  The rules are the following:

- arr[0….low-1] contains 0. [Extreme left part]
- arr[low….mid-1] contains 1.
- arr[high+1….n-1] contains 2. [Extreme right part], n = size of the array

The middle part i.e. arr[mid….high] is the unsorted segment. So, hypothetically the array with different markers will look like the following:

![](https://static.takeuforward.org/wp/uploads/2023/03/Screenshot-2023-03-18-171206.png)

In our case, we can assume that the entire given array is unsorted and so we will place the pointers accordingly. For example, if the given array is: [2,0,2,1,1,0], the array with the 3 pointers will look like the following:

![](https://static.takeuforward.org/wp/uploads/2023/03/Screenshot-2023-03-18-171326.png)

Here, as the entire array is unsorted, we have placed the mid pointer in the first index and the high pointer in the last index. The low is also pointing to the first index as we have no other index before 0. Here, we are mostly interested in placing the ‘mid’ pointer and the ‘high’ pointer as they represent the unsorted part in the hypothetical array.

Now, let’s understand how the pointers will work to make the array sorted.

**Approach:**

**Note:** _Here in this tutorial we will work based on the value of the mid pointer._

The steps will be the following:

1. First, we will run a loop that will continue until **mid <= high**.
2. There can be three different values of mid pointer i.e. arr[mid]
    1. **If arr[mid] == 0,** we will swap arr[low] and arr[mid] and will increment both low and mid. Now the subarray from index 0 to (low-1) only contains 0.
    2. **If arr[mid] == 1,** we will just increment the mid pointer and then the index (mid-1) will point to 1 as it should according to the rules.
    3. **If arr[mid] == 2,** we will swap arr[mid] and arr[high] and will decrement high. Now the subarray from index high+1 to (n-1) only contains 2.  
        In this step, we will do nothing to the mid-pointer as even after swapping, the subarray from mid to high(_after decrementing high_) might be unsorted. So, we will check the value of mid again in the next iteration.
3. Finally, our array should be sorted.

**Dry Run:**

Assume the given array is [2,0,2,1,1,0]. The algorithm will be the following:

![](https://static.takeuforward.org/wp/uploads/2023/03/Screenshot-2023-03-18-171428.png)

![](https://static.takeuforward.org/wp/uploads/2023/03/Screenshot-2023-03-18-173242.png)

![](https://static.takeuforward.org/wp/uploads/2023/03/Screenshot-2023-03-18-173322.png)

In each iteration, if we check, the rules are always valid. This is how the algorithm works.

**Note:** _For a better understanding of intuition, please watch the video at the bottom of the page._

Code

C++JavaPython

```java

import java.util.*;

public class Main {
    public static void sortArray(ArrayList<Integer> arr, int n) {
        int low = 0, mid = 0, high = n - 1; // 3 pointers

        while (mid <= high) {
            if (arr.get(mid) == 0) {
                // swapping arr[low] and arr[mid]
                int temp = arr.get(low);
                arr.set(low, arr.get(mid));
                arr.set(mid, temp);

                low++;
                mid++;

            } else if (arr.get(mid) == 1) {
                mid++;

            } else {
                // swapping arr[mid] and arr[high]
                int temp = arr.get(mid);
                arr.set(mid, arr.get(high));
                arr.set(high, temp);

                high--;
            }
        }
    }

    public static void main(String args[]) {
        int n = 6;
        ArrayList<Integer> arr = new ArrayList<>(Arrays.asList(new Integer[] {0, 2, 1, 2, 0, 1}));
        sortArray(arr, n);
        System.out.println("After sorting:");
        for (int i = 0; i < n; i++) {
            System.out.print(arr.get(i) + " ");
        }
        System.out.println();

    }

}
```

**Output:**After sorting:  
0 0 1 1 2 2  Complexity Analysis

**Time Complexity:** O(N), where N = size of the given array.  
**Reason:** We are using a single loop that can run at most N times.

**Space Complexity:** O(1) as we are not using any extra space.

