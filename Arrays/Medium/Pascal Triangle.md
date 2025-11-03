
Program to generate Pascal's Triangle

**Problem Statement:** Write a program to generate Pascal's triangle. In Pascal’s triangle, each number is the sum of the two numbers directly above it as shown in the figure below:

![](https://static.takeuforward.org/content/pascal-psu9jUm3)

**Examples**

Input: N = 5, r = 5, c = 3 
Output: Element at position (r, c): 6
N-th row of Pascal’s triangle: 1 4 6 4 1
First n rows of Pascal’s triangle:
1 
1 1 
1 2 1 
1 3 3 1 
1 4 6 4 1  
Explanation: Pascal triangle for first 5 rows is shown above.

Input: N = 1, r = 1, c = 1
Output: Element at position (r, c): 1
N-th row of Pascal’s triangle: 1
First n rows of Pascal’s triangle:
1  
Explanation: N = 1 is the base case fof a pascal's triangle.
            

**_Disclaimer_**: _Here is the [practice link](https://takeuforward.org/plus/dsa/problems/pascals-triangle-i) to help you assess your knowledge better. It's highly recommend trying to solve it before looking at the solution._

Practice:[Solve Problem](https://takeuforward.org/plus/dsa/problems/pascals-triangle-i)

Approach - 1

Algorithm

To generate the entire Pascal’s Triangle for the first N rows, we can start with the first row containing a single 1 and iteratively build each subsequent row using the property that every element (except the first and last) is the sum of the two elements directly above it from the previous row. The first and last elements of each row are always 1. By storing the previous row, we can calculate the next row easily. This process continues until we have constructed all N rows, resulting in the complete Pascal’s Triangle structure.

![Image 1](https://static.takeuforward.org/content/1.png-xujeFZ-p)

![Image 2](https://static.takeuforward.org/content/2.png-IqQzt9a7)

![Image 3](https://static.takeuforward.org/content/3.png-fcyNE8wW)

![Image 4](https://static.takeuforward.org/content/4.png-Q0tXOz-H)

Code



```java
import java.util.*;

// Class containing Pascal's Triangle generation logic
class Solution {
    // Function to generate Pascal's Triangle up to numRows
    public List<List<Integer>> generate(int numRows) {
        // Result list to hold all rows
        List<List<Integer>> triangle = new ArrayList<>();

        // Loop for each row
        for (int i = 0; i < numRows; i++) {
            // Create a row with size (i+1)
            List<Integer> row = new ArrayList<>(Collections.nCopies(i + 1, 1));

            // Fill elements from index 1 to i-1 (middle values)
            for (int j = 1; j < i; j++) {
                // Each element = sum of two elements above it
                row.set(j, triangle.get(i - 1).get(j - 1) +
                           triangle.get(i - 1).get(j));
            }

            // Add current row to the triangle
            triangle.add(row);
        }
        return triangle;
    }
}

public class Main {
    public static void main(String[] args) {
        Solution obj = new Solution();
        int n = 5;

        // Generate and print Pascal's Triangle
        List<List<Integer>> result = obj.generate(n);
        for (List<Integer> row : result) {
            for (Integer val : row) System.out.print(val + " ");
            System.out.println();
        }
    }
}
```

```java
class Solution {

    public List<Integer> generateRow(int row){

        List<Integer> temp = new ArrayList<>();

        int ans = 1;

        temp.add(ans);

        for(int column =1;column<row;column++){

            ans = ans*(row-column);

            ans = ans / column;

            temp.add(ans);

        }

        return temp;

    }

    public List<List<Integer>> generate(int numRows) {

        List<List<Integer>> ans = new ArrayList<>();

        for(int i=1; i<=numRows; i++){

            ans.add(generateRow(i));      

            }

        return ans;

        }        

}```
Complexity Analysis**Time Complexity: O(N^2)**, we generate all the elements in first N rows sequentially one by one.  
**Space Complexity: O(N^2)**, additional space used for storing the entire pascal triangle.

Approach - 2

Algorithm

To print the Nth row of the pascal triangle we can take advantage of the relationship between Nth element and binomial coefficients.  
  
In a pascal's triangle, the Nth row contains the binomial coefficients C(N-1, 0), C(N-1, 1) and so on till C(N-1, N-1). Thus we can simply calculate all these values to return the Nth row of pascal triangle.  
  
Instead of computing full factorials, we can start with the first value as 1, and use the relation C(n, k) = C(n, k−1) × (n−k+1) / k to compute the next value from the previous one in constant time.

Code


```java
import java.util.*;

// Class containing Pascal's Triangle row generation logic
class Solution {
    // Function to generate the Nth row of Pascal's Triangle
    public List<Long> getNthRow(int N) {
        // Result list to store the row
        List<Long> row = new ArrayList<>();
        
        // First value of the row is always 1
        long val = 1;
        row.add(val);
        
        // Compute remaining values using the relation:
        // C(n, k) = C(n, k-1) * (n-k) / k
        for (int k = 1; k < N; k++) {
            val = val * (N - k) / k;
            row.add(val);
        }
        
        return row;
    }
}

public class Main {
    public static void main(String[] args) {
        int N = 5; // Example: 5th row
        Solution sol = new Solution();
        List<Long> result = sol.getNthRow(N);

        // Print the row
        for (long num : result) {
            System.out.print(num + " ");
        }
    }
}
```
Complexity Analysis**Time Complexity: O(N)**, we iterate N times to compute each element of the row in O(1) time using the direct relation.  
**Space Complexity: O(N)**, additional space used for storing the Nth row.

Approach - 3

Algorithm

To find the element at the coordinates (R,C) where R is the row number and C is the Column number, we can simply simulate the generation of pascal's triangle for R rows. In Pascal’s Triangle, the element at row R and column C corresponds to the binomial coefficient (r-1)C(c-1). To calculate this binomial coefficient, we can simply apply the formula of binomial coefficient i.e. (r-1)!/(c-1)!(r-c)!.  
  
Instead of computing full factorials (which can overflow and be slow), we can multiply and divide in a loop to compute the coefficient efficiently.

Code

CppJavaPythonJavascript

```java
// Solution class to find the (r, c) element of Pascal's Triangle
class Solution {
    // Function to compute binomial coefficient (nCr)
    public long findPascalElement(int r, int c) {
        // Element is C(r-1, c-1)
        int n = r - 1;
        int k = c - 1;

        long result = 1;

        // Compute C(n, k) using iterative formula
        for (int i = 0; i < k; i++) {
            result *= (n - i);
            result /= (i + 1);
        }

        return result;
    }
}

// Main class to test the solution
public class Main {
    public static void main(String[] args) {
        Solution sol = new Solution();
        int r = 5, c = 3;
        System.out.println(sol.findPascalElement(r, c));
    }
}
```

Complexity Analysis**Time Complexity: O(min(c,r−c))**, The loop runs for min(c−1,r−c) iterations because binomial coefficients are symmetric.  
**Space Complexity: O(1)**, constant additional space is used.

