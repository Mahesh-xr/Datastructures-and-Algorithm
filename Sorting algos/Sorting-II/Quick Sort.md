```java
import java.util.*;

class Solution {
    static int partition(List<Integer> arr, int low, int high) {
        int pivot = arr.get(low);
        int i = low;
        int j = high;

        while (i < j) {
            while (arr.get(i) <= pivot && i <= high - 1) {
                i++;
            }

            while (arr.get(j) > pivot && j >= low + 1) {
                j--;
            }
            if (i < j) {
                int temp = arr.get(i);
                arr.set(i, arr.get(j));
                arr.set(j, temp);
            }
        }
        int temp = arr.get(low);
        arr.set(low, arr.get(j));
        arr.set(j, temp);
        return j;
    }

    static void qs(List<Integer> arr, int low, int high) {
        if (low < high) {
            int pIndex = partition(arr, low, high);
            qs(arr, low, pIndex - 1);
            qs(arr, pIndex + 1, high);
        }
    }
    public static List<Integer> quickSort(List<Integer> arr) {
        // Write your code here.
        qs(arr, 0, arr.size() - 1);
        return arr;
    }
}

public class tUf {
    public static void main(String args[]) {
        List<Integer> arr = new ArrayList<>();
        arr = Arrays.asList(new Integer[] {4, 6, 2, 5, 7, 9, 1, 3});
        int n = arr.size();
        System.out.println("Before Using insertion Sort: ");
        for (int i = 0; i < n; i++) {
            System.out.print(arr.get(i) + " ");
        }
        System.out.println();
        arr = Solution.quickSort(arr);
        System.out.println("After insertion sort: ");
        for (int i = 0; i < n; i++) {
            System.out.print(arr.get(i) + " ");
        }
        System.out.println();
    }

} 
```

**Output:**Before Using quick Sort:  
4 6 2 5 7 9 1 3  
After Using quick sort:  
1 2 3 4 5 6 7 9 

**Time Complexity:** O(N*logN), where N = size of the array.

**Reason:** At each step, we divide the whole array, for that logN and n steps are taken for partition() function, so overall time complexity will be N*logN.

The following recurrence relation can be written for Quick sort : 

F(n) = F(k) + F(n-1-k) 

Here k is the number of elements smaller or equal to the pivot and n-1-k denotes elements greater than the pivot.

There can be 2 cases :

**Worst Case** – This case occurs when the pivot is the greatest or smallest element of the array. If the partition is done and the last element is the pivot, then the worst case would be either in the increasing order of the array or in the decreasing order of the array. 

**Recurrence:**  
**F(n) = F(0)+F(n-1)  or  F(n) = F(n-1) + F(0)** 

**Worst Case Time complexity: O(n****2****)** 

Best Case – This case occurs when the pivot is the middle element or near to middle element of the array.  
Recurrence :  
F(n) = 2F(n/2)  
  
Time Complexity for the best and average case: O(N*logN)

**Space Complexity:** O(1) + O(N) auxiliary stack space.



Approach:

, let’s understand how we are going to implement the logic of the above steps. In the intuition, we have seen that the given array should be broken down into subarrays. But while implementing, we are not going to break down and create any new arrays. Instead, we will specify the range of the subarrays using two indices or pointers(i.e. **low** pointer and **high** pointer) each time while breaking down the array.

The algorithm steps are the following for the **quickSort()** function:

1. Initially, the **low** points to the first index and the **high** points to the last index(as the range is n i.e. the size of the array). 
2. After that, we will get the index(_where the pivot should be placed after sorting_) while shifting the smaller elements on the left and the larger ones on the right using a partition() function discussed later.  
    Now, this index can be called the **partition index** as it creates a partition between the left and the right unsorted subarrays.
3. After placing the pivot in the partition index(within the partition() function specified), we need to call the function quickSort() for the left and the right subarray recursively. So, **the range of the left subarray will be [low to (partition index - 1)]** and **the range of the right subarray will be [(partition index + 1) to high].** 
4. This is how the recursion will continue until the range becomes 1.
