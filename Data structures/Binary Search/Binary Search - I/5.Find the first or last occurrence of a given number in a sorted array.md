Given a sorted array of **N integers**, write a program to find the index of the last occurrence of the target key. If the target is not found then return -1.

**Note:** Consider 0 based indexing

**Examples:**

**Example 1:**
**Input:** N = 7, target=13, array[] = {3,4,13,13,13,20,40}
**Output**: 4
**Explanation:** As the target value is 13 , it appears for the first time at index number 2.

**Example 2:**
**Input:** N = 7, target=60, array[] = {3,4,13,13,13,20,40}
**Output**: -1
**Explanation:** Target value 60 is not present in the array 

**_Disclaimer_**: _Don't jump directly to the solution, try it out yourself first._

### **Solution 1: Naive solution** 

- As the array is already sorted, start traversing the array from the back using a for loop and check whether the element is present or not.
- If the target element is present, break out of the loop and print the resulting index.
- If the target element is not present inside the array, then print -1

**Code**:

C++Java

```java
public class tUf {
  public static int solve(int n, int key, int[] v) {
    int res = -1;
    for (int i = n - 1; i >= 0; i--) {
      if (v[i] == key) {
        res = i;
        break;
      }
    }
    return res;
  }
  public static void main(String args[]) {
    int n = 7;
    int key = 13;
    int[] v = {3,4,13,13,13,20,40};

    // returning the last occurrence index if the element is present otherwise -1
    System.out.println(solve(n, key, v));
  }
}
```

**Output:** 4

**Time Complexity: O(n)** 

**Space Complexity: O(1)** not considering the given array

### **Solution 2: Binary search solution (optimised)** 

As given in the question, the array is already sorted

Whenever the word “sorted” or other similar terminologies are used in an array question, BINARY SEARCH **can** be one of the approaches.

Initially consider the start=0 and the end=n-1 pointers and the result as -1.

Till start does not crossover end pointer compare the mid element 

- If the mid element is equal to the key value, store the mid-value in the result and move the start pointer to mid+1(move leftward)
- Else if the key value is less than the mid element then end= mid-1(move leftward)
- Else do start = mid+1 (move rightwards)

**Code:**

C++Java

```java
public class tUf {
  public static int solve(int n, int key, int[] v) {
    int start = 0;
    int end = n - 1;
    int res = -1;

    while (start <= end) {
      int mid = start + (end - start) / 2;
      if (v[mid] == key) {
        res = mid;
        start = mid + 1;

      } else if (key < v[mid]) {
        end = mid - 1;
      } else {
        start = mid + 1;
      }
    }
    return res;
  }
  public static void main(String args[]) {
    int n = 7;
    int key = 13;
    int[] v = {3,4,13,13,13,20,40};

    // returning the last occurrence index if the element is present otherwise -1
    System.out.println(solve(n, key, v));
  }
}
```

**Output:** **4**

**Time Complexity: O(log n)**

**Space Complexity: O(1)**

