
**Problem Statement:** Given an array, print all the elements which are leaders. A Leader is an element that is greater than all of the elements on its right side in the array.

**Examples**

**Example 1:**
**Input:**
 arr = [4, 7, 1, 0]
**Output**:
 7 1 0
**Explanation:**
 Rightmost element is always a leader. 7 and 1 are greater than the elements in their right side.

**Example 2:**
**Input:**
 arr = [10, 22, 12, 3, 0, 6]
**Output:**
 22 12 6
**Explanation:**
 6 is a leader. In addition to that, 12 is greater than all the elements in its right side (3, 0, 6), also 22 is greater than 12, 3, 0, 6.

**_Disclaimer_**: _Don’t jump directly to the solution, try it out yourself first._

Practice:[Solve Problem](https://takeuforward.org/plus/dsa/problems/leaders-in-an-array)

Brute Force Approach

Algorithm / Intuition

### **Solution 1:**

### **Intuition:**

There is no special intuition needed here. Just a common fact that we need to compare elements in order to find the greatest is more than enough.

### **Approach**

- In this brute force approach, we start checking all the elements from the start of the array to the end to see if an element is greater than all the elements on its right (i.e, the leader).
- For this, we will use nested loops where the outer loop will check for each element in the array whether it is a leader or not.
- The inner loop checks if there is any element to the right that is greater than the element currently traversed by the outer loop.
- We start by initializing the outer loop pointer to the start element and setting it as the current leader.
- If any element traversed is found greater than the element currently set as a leader, it will not go to the ans array and we increment the outer loop pointer by 1 and set the next element as the current leader.
- If we don’t find any other element to the right greater than the current element, then we push the current element to the ans array stating that is it the leader element.

Code

C++JavaPythonJavaScript

```java



import java.util.*;

class TUF{

public static ArrayList<Integer> 
                     printLeadersBruteForce(int[] arr, int n){
    
  ArrayList<Integer> ans= new ArrayList<>();
  
  for (int i = 0; i < n; i++) {
    boolean leader = true;

    //Checking whether arr[i] is greater than all 
    //the elements in its right side
    for (int j = i + 1; j < n; j++)
      if (arr[j] > arr[i]) {
          
        // If any element found is greater than current leader
        // curr element is not the leader.
        leader = false;
        break;
      }

    // Push all the leaders in ans array.
    if (leader)
    ans.add(arr[i]);

  }
  
  return ans;
   
}

public static void main(String args[]) 
{
  // Array Initialization.
  int n = 6;
  int arr[]=  {10, 22, 12, 3, 0, 6};


  ArrayList<Integer> ans= printLeadersBruteForce(arr,n);
  
  for (int i = 0; i < ans.size(); i++) {
    System.out.print(ans.get(i)+" ");
  }

}
}
```

**Output:**

22 12 6

Complexity Analysis

**Time Complexity:** O(N^2) { Since there are nested loops being used, at the worst case n^2 time would be consumed }.

**Space Complexity: O(N)** { There is no extra space being used in this approach. But, a O(N) of space for ans array will be used in the worst case }.

Optimal Approach

Algorithm / Intuition

### **Solution 2:**

### **Approach**:

![](https://lh3.googleusercontent.com/q9mAQ0qpQIQ83jbG_a55XKHKTOf0jpWWZPBN6jpBLqyHZLx7o7XVCCSJWVInyFNxcBP3srruP2jrEFtpZpirH7r0KXoppkXFvTrLaiFavylbb6yc0kJt2ne4Xk25ToB5R_7CLWRm)

- In the above approach, we do a fresh traversal for each candidate. If we think carefully, we only want to compare the elements on the right side. So, what if we start from the last element?
- That is, we'll try to remember the greatest element encountered so far and we'll use that to decide whether a candidate is a leader or not.
- First, we'll start the traversal from the right. Then, we move toward the left. Whenever we encounter a new element, we check with the greatest element obtained so far.
- If the current element is greater than the greatest so far, then the current element is one of the leaders and we update the greatest element.
- Else, we proceed with the further elements on the left. This method prints the leaders in the reverse direction of their occurrences. If we are concerned about the order, we can use an extra array or a string to order.

Code

C++JavaPythonJavaScript

```java



import java.util.*;

class TUF{

public static ArrayList<Integer> 
                     printLeadersBruteForce(int[] arr, int n){
    
  ArrayList<Integer> ans= new ArrayList<>();
  
   // Last element of an array is always a leader,
   // push into ans array.
   int max = arr[n - 1];

    ans.add(arr[n-1]);
    
    // Start checking from the end whether a number is greater
    // than max no. from right, hence leader.
    for (int i = n - 2; i >= 0; i--)
      if (arr[i] > max) {
        ans.add(arr[i]);
        max = arr[i];
      }

  return ans;
   
}

public static void main(String args[]) 
{
  // Array Initialization.
  int n = 6;
  int arr[]=  {10, 22, 12, 3, 0, 6};


  ArrayList<Integer> ans= printLeadersBruteForce(arr,n);
  
  Collections.sort(ans, Collections.reverseOrder());
  
  for (int i = 0; i < ans.size(); i++) {
    System.out.print(ans.get(i)+" ");
  }

}
}
```

**Output:**

22 12 6

Complexity Analysis

**Time Complexity: O(N)** { Since the array is traversed single time back to front, it will consume O(N) of time where N = size of the array }.

**Space Complexity: O(N)** { There is no extra space being used in this approach. But, a O(N) of space for ans array will be used in the worst case }.

