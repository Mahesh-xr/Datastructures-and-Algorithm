
Maximum Product Subarray in an Array

Problem Statement: Given an array that contains both negative and positive integers, find the maximum product subarray.

Examples
Disclaimer: Here is the practice link to help you assess your knowledge better. It's highly recommend trying to solve it before looking at the solution.

Practice:
Solve Problem
Brute-Force Approach
Algorithm
In the brute force method, we generate all possible subarrays and calculate the product of each subarray. We track the maximum product found among all. Use two nested loops, the outer loop picks the starting index of the subarray and the inner loop picks the ending index. For every subarray defined by start and end, calculate the product and update the maximum product if the current subarray's product is larger.


class Solution {
    // This function finds the maximum product
    // of any contiguous subarray using brute force
    public int maxProduct(int[] nums) {
        // Initialize the answer with the first element
        int maxProd = nums[0];

        // Outer loop picks the starting index
        for (int i = 0; i < nums.length; i++) {
            // Initialize current product to 1
            int prod = 1;

            // Inner loop picks the ending index
            for (int j = i; j < nums.length; j++) {
                // Multiply current number to product
                prod *= nums[j];

                // Update maximum product if needed
                maxProd = Math.max(maxProd, prod);
            }
        }

        // Return the result
        return maxProd;
    }
}

public class Main {
    public static void main(String[] args) {
        // Sample input
        int[] nums = {2, 3, -2, 4};

        // Create Solution object
        Solution sol = new Solution();

        // Print the result
        System.out.println(sol.maxProduct(nums));
    }
}
Complexity Analysis
Optimal Approach - 1
Algorithm
The product of elements in a subarray can become large when there are positive numbers, but negative numbers and zeros make it tricky. A negative number can flip a large product into a negative one, but if we meet another negative later, the sign flips back to positive. Therefore, to capture all possible max products, we do two things:
Traverse the array from left to right (prefix) to build cumulative product.
Traverse the array from right to left (suffix) to catch subarrays ending at the back (helpful when max product is at the end or due to even negatives).
Reset the product to 1 whenever a zero is found, as it breaks the subarray continuity.
By comparing products in both directions at each step, we ensure we don’t miss any possible maximum.
Image 1
Image 2


Code
Cpp
Java
Python
Javascript


// This class contains the function to find maximum product subarray
// using prefix and suffix traversal
class Solution {
    public int maxProductSubArray(int[] arr) {
        // Get the length of the array
        int n = arr.length;

        // Initialize prefix and suffix product
        int pre = 1, suff = 1;

        // Initialize answer with smallest integer
        int ans = Integer.MIN_VALUE;

        // Traverse from both left and right
        for (int i = 0; i < n; i++) {
            // Reset prefix if zero
            if (pre == 0) pre = 1;

            // Reset suffix if zero
            if (suff == 0) suff = 1;

            // Multiply prefix with current element from front
            pre *= arr[i];

            // Multiply suffix with current element from back
            suff *= arr[n - i - 1];

            // Update maximum value so far
            ans = Math.max(ans, Math.max(pre, suff));
        }

        // Return the final result
        return ans;
    }
}

// Separate Main class for testing
class Main {
    public static void main(String[] args) {
        // Sample input
        int[] arr = {2, 3, -2, 4};

        // Create object of Solution
        Solution sol = new Solution();

        // Call the method and print the result
        System.out.println(sol.maxProductSubArray(arr));
    }
}
Complexity Analysis
Optimal Approach - 2
Algorithm
In a product-based subarray problem, a negative number can flip the sign, turning a big minimum into a potential maximum. So, we track both the maximum and minimum products at each step. This helps handle negative numbers effectively.
Start by setting the answer, current max product, and current min product to the first number.
For each number in the array starting from the second:
If the number is negative, swap current max and min.
If the product of the current number and previous max is larger than the number itself, update current max to that product; otherwise, set it to the number.
Similarly, if the product of the current number and previous min is smaller, update current min; otherwise, set it to the number.
If the current max is greater than the answer so far, update the answer.
Return the answer at the end.

Code
Cpp
Java
Python
Javascript


class Solution {
    // This function returns the maximum product
    // of any contiguous subarray using optimized approach
    public int maxProduct(int[] nums) {
        int res = nums[0];
        int maxProd = nums[0];
        int minProd = nums[0];

        // Traverse from second element
        for (int i = 1; i < nums.length; i++) {
            int curr = nums[i];

            // Swap if current is negative
            if (curr < 0) {
                int temp = maxProd;
                maxProd = minProd;
                minProd = temp;
            }

            // Update max and min product
            maxProd = Math.max(curr, maxProd * curr);
            minProd = Math.min(curr, minProd * curr);

            // Update result
            res = Math.max(res, maxProd);
        }

        return res;
    }
}

public class Main {
    public static void main(String[] args) {
        int[] nums = {2, 3, -2, 4};
        Solution sol = new Solution();
        System.out.println(sol.maxProduct(nums));
    }
}
Complexity Analysis
Time Complexity: O(N), every element of array is visited once.
Space Complexity: O(1) , only constant variables are used.
