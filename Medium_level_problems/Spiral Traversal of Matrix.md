
Spiral Traversal of Matrix

**Problem Statement:** Given a Matrix, print the given matrix in spiral order.

**Examples:**

**Example 1:**
**Input:** Matrix[][] = { { 1, 2, 3, 4 },
		      { 5, 6, 7, 8 },
		      { 9, 10, 11, 12 },
	              { 13, 14, 15, 16 } }

**Outhput:** 1, 2, 3, 4, 8, 12, 16, 15, 14, 13, 9, 5, 6, 7, 11, 10.
**Explanation:** The output of matrix in spiral form.

**Example 2:**
**Input:** Matrix[][] = { { 1, 2, 3 },
	              { 4, 5, 6 },
		      { 7, 8, 9 } }
			    
**Output:** 1, 2, 3, 6, 9, 8, 7, 4, 5.
**Explanation:** The output of matrix in spiral form.

### **Solution**

**_Disclaimer_**: _Don't jump directly to the solution, try it out yourself first._

### **Approach:**

Printing a matrix in spiral form is the same as peeling an onion layer by layer. Because we are printing the elements layer by layer starting from the outer layers.

**![](https://lh5.googleusercontent.com/hZPzGMsKGm29AE34ZtOii2D76g2_M0mXxbnceTnuxSyodzJTUSYWJz3umCeyEWMas0OtAQVWE17WEDSQ_nbg6B_7m1mFyNfmzsN34GDhQjHDa4eV20u1wG4mBjPefJyRU53fAAe5hFQ2ZYChxw)**

In this approach, we will be using **four loops** to print all four sides of the matrix.

**1st loop:** This will print the elements from left to right.

**2nd loop:** This will print the elements from top to bottom.

**3rd loop:** This will print the elements from right to left.

**4th loop:** This will print the elements from bottom to top.

**Steps:**

- Create and initialize variables top as starting row index, bottom as ending row index left as starting column index, and right as ending column index. As shown in the image given below.

![](https://lh4.googleusercontent.com/Xq3R-xwSRxUx3EEjKQIMaHTM9qGqj81nPFe2nGoSnxnd36bStjU989Sf-CsWAnFZf4jHS68xr4l49QqKXeo7o7lLF1V38SJYaxC1CWWNzvk-eBNevdWhduS6mBSX9QGXGATwQw0OkFXkP18JWw)

- In each outer loop traversal print the elements of a square in a clockwise manner.
- **Print the top row**, i.e. Print the elements of the top row from column index left to right and increase the count of the top so that it will move to the next row.
- **Print the right column**, i.e. Print the rightmost column from row index top to bottom and decrease the count of right.
- **Print the bottom row**, i.e. if top <= bottom, then print the elements of a bottom row from column right to left and decrease the count of bottom
- **Print the left column**, i.e. if left <= right, then print the elements of the left column from the bottom row to the top row and increase the count of left.
- Run a loop until all the squares of loops are printed.

**Note:** As we can see in the code snippet below, two edge conditions are being added in the last two ‘for’ loops: when we are moving from right to left and from bottom to top. 

These conditions are added to check if the matrix is a single column or a single row. So, whenever the elements in a single row are traversed they cannot be traversed again backward so the condition is checked in the right-to-left loop. When a single column is present, the condition is checked in the bottom-to-top loop as elements from bottom to top cannot be traversed again.

**Code**:

C++JavaPythonJavascript

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    
    public static List<Integer> printSpiral(int[][] mat) {
        
        // Define ans list to store the result.
        List<Integer> ans = new ArrayList<>();
        
        int n = mat.length; // no. of rows
        int m = mat[0].length; // no. of columns
        
        // Initialize the pointers required for traversal.
        int top = 0, left = 0, bottom = n - 1, right = m - 1;

        // Loop until all elements are not traversed.
        while (top <= bottom && left <= right) {

            // For moving left to right
            for (int i = left; i <= right; i++)
                ans.add(mat[top][i]);

            top++;

            // For moving top to bottom.
            for (int i = top; i <= bottom; i++)
                ans.add(mat[i][right]);

            right--;

            // For moving right to left.
            if (top <= bottom) {
                for (int i = right; i >= left; i--)
                    ans.add(mat[bottom][i]);

                bottom--;
            }

            // For moving bottom to top.
            if (left <= right) {
                for (int i = bottom; i >= top; i--)
                    ans.add(mat[i][left]);

                left++;
            }
        }
        return ans;
    }

    public static void main(String[] args) {
        
        //Matrix initialization.
        int[][] mat = {{1, 2, 3, 4},
                       {5, 6, 7, 8},
                       {9, 10, 11, 12},
                       {13, 14, 15, 16}};
        
        List<Integer> ans = printSpiral(mat);

        for(int i = 0;i<ans.size();i++){
            System.out.print(ans.get(i) + " ");
        }

        System.out.println();
    }
}
```

**Output:**

1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10

**Time Complexity: O(m x n)** { Since all the elements are being traversed once and there are total n x m elements ( m elements in each row and total n rows) so the time complexity will be O(n x m)}.

**Space Complexity: O(n)** { Extra Space used for storing traversal in the ans array }.

