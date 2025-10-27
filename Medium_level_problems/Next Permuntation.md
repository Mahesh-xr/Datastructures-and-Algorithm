

## Next_permutation : find next lexicographically greater permutation



**Problem Statement:** Given an array Arr[] of integers, rearrange the numbers of the given array into the lexicographically next greater permutation of numbers.  
  
If such an arrangement is not possible, it must rearrange to the lowest possible order (i.e., sorted in ascending order).

**Examples**

Input: Arr[] = {1,3,2}
Output: {2,1,3}
Explanation: All permutations of {1,2,3} are {{1,2,3} , {1,3,2}, {2,13} , {2,3,1} , {3,1,2} , {3,2,1}}. So, the next permutation just after {1,3,2} is {2,1,3}.

Input : Arr[] = {3,2,1}
Output: {1,2,3}
Explanation : As we see all permutations of {1,2,3}, we find {3,2,1} at the last position. So, we have to return the lowest permutation.
            

**_Disclaimer_**: _Here is the [practice link](https://takeuforward.org/plus/dsa/problems/next-permutation) to help you assess your knowledge better. It's highly recommend trying to solve it before looking at the solution._

Brute-Force Approach

Algorithm

The brute force approach to find the next permutation is to find all possible permutations of the array and then look for next permutation.

- Find all possible permutations of elements present and store them.
Sort the permutations and search input from all possible permutations. Print the next permutation present right after it.- If the current permutation is the last, return the first permutation in the list.

Code

CppJavaPythonJavascript

```java
import java.util.*;

class Solution {
    // Function to find the next permutation
    public List<Integer> nextPermutation(int[] nums) {
        // List to hold all permutations
        List<List<Integer>> all = new ArrayList<>();

        // Sort the input to start from the smallest
        Arrays.sort(nums);

        // Generate all permutations
        permute(nums, 0, all);

        // Convert array to list for comparison
        List<Integer> current = new ArrayList<>();
        for (int num : nums)
            current.add(num);

        // Find and return the next permutation
        for (int i = 0; i < all.size(); i++) {
            if (all.get(i).equals(current)) {
                if (i == all.size() - 1)
                    return all.get(0);
                return all.get(i + 1);
            }
        }

        return current;
    }

    // Backtracking permutation generator
    private void permute(int[] nums, int start, List<List<Integer>> all) {
        if (start == nums.length) {
            List<Integer> temp = new ArrayList<>();
            for (int num : nums) temp.add(num);
            all.add(new ArrayList<>(temp));
            return;
        }
        for (int i = start; i < nums.length; i++) {
            swap(nums, i, start);
            permute(nums, start + 1, all);
            swap(nums, i, start);
        }
    }

    // Swap helper
    private void swap(int[] arr, int i, int j) {
        int tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
    }
}

public class Main {
    public static void main(String[] args) {
        Solution sol = new Solution();
        int[] nums = {1, 2, 3};

        List<Integer> result = sol.nextPermutation(nums);
        for (int x : result)
            System.out.print(x + " ");
        System.out.println();
    }
}
```
Complexity Analysis**Time Complexity: O(N!*N)**, since we are generating all possible permutations, it takes N! time.  
**Space Complexity: O(N!)**, storing all permutations.

Optimal Approach

Algorithm

We want to rearrange the array to form the next greater permutation. If that's not possible (i.e., it's the last permutation), we return the smallest one (i.e., sorted ascendingly).  
  
To find this next permutation with minimal change, we need to find a digit that can be increased slightly to make the number bigger and then rearrange the remaining part to be the smallest possible.

- Traverse from the end and find the first index where the current digit is smaller than the next one (this is the "breaking point").
- Then again traverse from the end to find the first digit greater than the breaking point digit and swap them.
- Finally, reverse the part of the array to the right of the breaking point to get the smallest next permutation.
- If no such breaking point exists (entire array is descending), just reverse the whole array.

![Image 1](https://static.takeuforward.org/content/1.png-naoQiFDp)

![Image 2](https://static.takeuforward.org/content/2.png-1jxTkb7A)

![Image 3](https://static.takeuforward.org/content/3.png-S9cd9DSu)

![Image 4](https://static.takeuforward.org/content/4.png-_2CyHKOp)

Code

CppJavaPythonJavascript

```java
import java.util.*;

// Solution class
class Solution {
    // Function to find next permutation
    public void nextPermutation(int[] nums) {
        // Set index to -1
        int index = -1;

        // Find the first decreasing element from end
        for (int i = nums.length - 2; i >= 0; i--) {
            // If smaller found
            if (nums[i] < nums[i + 1]) {
                // Store index
                index = i;
                break;
            }
        }

        // If no index found
        if (index == -1) {
            // Reverse the entire array
            reverse(nums, 0, nums.length - 1);
            return;
        }

        // Find just larger element
        for (int i = nums.length - 1; i > index; i--) {
            // Swap them
            if (nums[i] > nums[index]) {
                swap(nums, i, index);
                break;
            }
        }

        // Reverse part after index
        reverse(nums, index + 1, nums.length - 1);
    }

    // Helper to reverse array
    private void reverse(int[] arr, int start, int end) {
        while (start < end) {
            swap(arr, start, end);
            start++;
            end--;
        }
    }

    // Helper to swap
    private void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}

// Main class
public class Main {
    public static void main(String[] args) {
        // Input array
        int[] nums = {1, 2, 3};

        // Create object
        Solution sol = new Solution();

        // Call function
        sol.nextPermutation(nums);

        // Print result
        for (int num : nums) {
            System.out.print(num + " ");
        }
        System.out.println();
    }
}
```
Complexity Analysis**Time Complexity: O(N)**, we find the breaking point and reverse the subarray in linear time.  
**Space Complexity: O(1)**, constant additional space is used.

