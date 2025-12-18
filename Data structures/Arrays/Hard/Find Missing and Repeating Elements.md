Find Missing and Repeating Elements;

**Problem Statement:** You are given a read-only array of N integers with values also in the range [1, N] both inclusive. Each integer appears exactly once except A which appears twice and B which is missing. The task is to find the repeating and missing numbers A and B where A repeats twice and B is missing.

**Examples**

**Example 1:**
**Input Format**:  array[] = {3,1,2,5,3}
**Result**: {3,4)
**Explanation**: A = 3 , B = 4 
Since 3 is appearing twice and 4 is missing

**Example 2:**
**Input Format**: array[] = {3,1,2,5,4,6,7,5}
**Result**: {5,8)
**Explanation**: A = 5 , B = 8 
Since 5 is appearing twice and 8 is missing

**_Disclaimer_**: _Don’t jump directly to the solution, try it out yourself first._

Practice:[Solve Problem](https://takeuforward.org/plus/dsa/problems/find-the-repeating-and-missing-number)

Brute Force Approach

Algorithm / Intuition

### **Solution:**

### **Naive Approach (Brute force)**: 

**Intuition:** For each number between 1 to N, we will try to count the occurrence in the given array using [linear search](https://takeuforward.org/data-structure/linear-search-in-c/). And the element with occurrence as 2 will be the repeating number and the number with 0 occurrences will be the missing number.

### **Approach:**

The steps are as follows:

1. We will run a loop(say i) from 1 to N.
2. For each integer, i, we will count its occurrence in the given array using linear search.
3. We will store those two elements that have the occurrence of 2 and 0.
4. Finally, we will return the elements.

**Note:** _For a better understanding of intuition, please watch the video at the bottom of the page._

Code

C++JavaPythonJavaScript

```java



import java.util.*;

public class tUf {

    public static int[] findMissingRepeatingNumbers(int[] a) {
        int n = a.length; // size of the array
        int repeating = -1, missing = -1;

        //Find the repeating and missing number:
        for (int i = 1; i <= n; i++) {
            //Count the occurrences:
            int cnt = 0;
            for (int j = 0; j < n; j++) {
                if (a[j] == i) cnt++;
            }

            if (cnt == 2) repeating = i;
            else if (cnt == 0) missing = i;

            if (repeating != -1 && missing != -1)
                break;
        }
        int[] ans = {repeating, missing};
        return ans;
    }

    public static void main(String[] args) {
        int[] a = {3, 1, 2, 5, 4, 6, 7, 5};
        int[] ans = findMissingRepeatingNumbers(a);
        System.out.println("The repeating and missing numbers are: {"
                           + ans[0] + ", " + ans[1] + "}");
    }
}
```

**Output:** The repeating and missing numbers are: {5, 8}

Complexity Analysis

**Time Complexity:** O(N2), where N = size of the given array.  
**Reason:** Here, we are using nested loops to count occurrences of every element between 1 to N.

**Space Complexity:** O(1) as we are not using any extra space.

Better Approach

Algorithm / Intuition

### **Better Approach (Using Hashing)**: 

### **Intuition:**

Instead of counting the occurrences every time, using the hashing technique, we will store the frequency of each element between 1 to N. Now, the element with frequency 2 will be the repeating number and the element with frequency 0 will be the missing number.

**Note:** _Here, we can solve this problem using a hash array._

### **Approach:**

The steps are as follows:

1. The range of the number is 1 to N. So, we need a hash array of size N+1 (_as we want to store the frequency of N as well_).
2. We will iterate all the elements of the given array and update the hash array accordingly i.e. hash[a[i]] = hash[a[i]]+1.
3. Now, we will iterate on the hash array and return the two elements with frequencies 2 and 0.

**Note:** _For a better understanding of intuition, please watch the video at the bottom of the page._

Code

C++JavaPythonJavaScript

```java



import java.util.*;

public class tUf {

    public static int[] findMissingRepeatingNumbers(int[] a) {
        int n = a.length; // size of the array
        int[] hash = new int[n + 1]; // hash array

        //update the hash array:
        for (int i = 0; i < n; i++) {
            hash[a[i]]++;
        }

        //Find the repeating and missing number:
        int repeating = -1, missing = -1;
        for (int i = 1; i <= n; i++) {
            if (hash[i] == 2) repeating = i;
            else if (hash[i] == 0) missing = i;

            if (repeating != -1 && missing != -1)
                break;
        }
        int[] ans = {repeating, missing};
        return ans;
    }

    public static void main(String[] args) {
        int[] a = {3, 1, 2, 5, 4, 6, 7, 5};
        int[] ans = findMissingRepeatingNumbers(a);
        System.out.println("The repeating and missing numbers are: {"
                           + ans[0] + ", " + ans[1] + "}");
    }
}

```

**Output:** The repeating and missing numbers are: {5, 8}

Complexity Analysis

**Time Complexity:** O(2N), where N = the size of the given array.  
**Reason:** We are using two loops each running for N times. So, the time complexity will be O(2N).

**Space Complexity:** O(N) as we are using a hash array to solve this problem.

Optimal Approach 1:

Algorithm / Intuition

### **Optimal Approach 1 (Using Maths)**: 

### **Intuition:**

The idea is to convert the given problem into mathematical equations. Since we have two variables i.e. missing and repeating, we will try to form two linear equations. And then we will find the values of two variables using those equations.

Assume the repeating number to be X and the missing number to be Y.

In the array, the numbers are between 1 to N, and in that range, one number is missing and one number is occurring twice.

**Step 1: Form equation 1:**

Now, we know the summation of the first N numbers is:

Sn = (N*(N+1))/2

Let’s say, **S = the summation of all the elements in the given array**.

Therefore, S - Sn = X - Y…………………equation 1

**Step 2: Form equation 2:**

Now, we know the summation of squares of the first N numbers is:

S2n = (N*(N+1)*(2N+1))/6

Let’s say, **S2 = the summation of squares of all the elements in the given array**.

Therefore, S2-S2n = X2-Y2...................equation 2

From equation 2 we can conclude,

X+Y = (S2 - S2n) / (X-Y) [From equation 1, we get the value X-Y] ………… equation 3

From equation 1 and equation 3, we can easily find the value of X and Y. And this is what we want.

**Note:** _Here, we are summing all the numbers and squares of all the numbers, so we should use a bigger data type(Like in C++, long long instead of int)._

### **Approach:**

Assume the repeating number to be X and the missing number to be Y.

The steps are as follows:

1. First, find out the values of S and Sn and then calculate S - Sn (Using the above formulas).
2. Then, find out the values of S2 and S2n and then calculate S2 - S2n.
3. After performing steps 1 and 2, we will be having the values of X + Y and X - Y. Now, by substitution of values, we can easily find the values of X and Y.

**Note:** _For a better understanding of intuition, please watch the video at the bottom of the page._

Code

C++JavaPythonJavaScript

```java



import java.util.*;

public class tUf {

    public static int[] findMissingRepeatingNumbers(int[] a) {
        long n = a.length; // size of the array
        // Find Sn and S2n:
        long SN = (n * (n + 1)) / 2;
        long S2N = (n * (n + 1) * (2 * n + 1)) / 6;

        // Calculate S and S2:
        long S = 0, S2 = 0;
        for (int i = 0; i < n; i++) {
            S += a[i];
            S2 += (long)a[i] * (long)a[i];
        }

        //S-Sn = X-Y:
        long val1 = S - SN;

        // S2-S2n = X^2-Y^2:
        long val2 = S2 - S2N;

        //Find X+Y = (X^2-Y^2)/(X-Y):
        val2 = val2 / val1;

        //Find X and Y: X = ((X+Y)+(X-Y))/2 and Y = X-(X-Y),
        // Here, X-Y = val1 and X+Y = val2:
        long x = (val1 + val2) / 2;
        long y = x - val1;

        int[] ans = {(int)x, (int)y};
        return ans;
    }

    public static void main(String[] args) {
        int[] a = {3, 1, 2, 5, 4, 6, 7, 5};
        int[] ans = findMissingRepeatingNumbers(a);
        System.out.println("The repeating and missing numbers are: {"
                           + ans[0] + ", " + ans[1] + "}");
    }
}
```

**Output:** The repeating and missing numbers are: {5, 8}

Complexity Analysis

**Time Complexity:** O(N), where N = the size of the given array.  
**Reason:** We are using only one loop running for N times. So, the time complexity will be O(N).

**Space Complexity:** O(1) as we are not using any extra space to solve this problem.

Optimal Approach 2

Algorithm / Intuition

### **Optimal Approach 2 (Using XOR)**: 

Using XOR, we are going to solve this problem using the following 3 steps.

Assume the repeating number to be X and the missing number to be Y.

**Step 1: Find the XOR of the repeating number(X) and the missing number(Y)****i.e. (X^Y) = (a[0]^a[1]^.....^a[n-1]) ^ (1^2^........^N)**

- In order to find the XOR of the repeating number and the missing number, we will first XOR all the array elements and with that value, we will again XOR all the numbers from 1 to N.  
    (X^Y) = (a[0]^a[1]^.....^a[n-1]) ^ (1^2^3^........^N)

**Step 2: Find the first different bit from right between the repeating and the missing number i.e. the first set bit from right in (X^Y)**

- By convention, the repeating and the missing number must be different and since they are different they must contain different bits. Now, our task is to find the first different bit from the right i.e. the end. We know, the XOR of two different bits always results in 1. The position of the first different bit from the end will be the first set bit(_from the right_) in (X^Y) that we have found in step 1.

**Step 3: Based on the position of the different bits we will group all the elements ( i.e. all array elements + all elements between 1 to N) into 2 different groups**

- Assume an imaginary array containing all the array elements and all the elements between 1 to N. Now, we will check that particular position for each element of that imaginary array and then if the bit is 0, we will insert the element into the 1st group otherwise, we will insert it into the 2nd group. 
- After performing this step, we will get two groups. Now, if we XOR all the elements of those 2 groups, we will get 2 numbers. One of them will be the repeating number and the other will be the missing number. But until now, we do not know which one is repeating and which one is missing.

**Last step: Figure out which one of the numbers is repeating and which one is missing**

- We will traverse the entire given array and check which one of them appears twice. And the number that appears twice is the repeating number and the other one is the missing number.

### **Approach:**

The steps are as follows:

1. For the first step, we will run a loop and calculate the XOR of all the array elements and the numbers between 1 to N. Let’s call this value xr.
2. In order to find the position of the first set bit from the right, we can either use a loop or we can perform AND of the xr and negation of (xr-1) i.e. (xr & ~(xr-1)).
3. Now, we will take two variables i.e. zero and one. Now, we will check the bit of that position for every element (array elements as well as numbers between 1 to N).
    1. **If the bit is 1:** We will XOR that element with variable one.
    2. **If the bit is 0:** We will XOR that element with variable zero.
4. Finally, we have two variables i.e. two numbers zero and one. Among them, one is repeating and the other is missing. It’s time to identify them. 
    1. We will traverse the entire array and check how many times variable zero appears. 
    2. If it appears twice, it will be the repeating number, otherwise, it will be the missing. Now, based on variable zero’s identity, we can easily identify in which category, variable one belongs.

**Note:** _For a better understanding of intuition, please watch the video at the bottom of the page._

Code

C++JavaPythonJavaScript

```java



import java.util.*;

public class tUf {

    public static int[] findMissingRepeatingNumbers(int[] a) {
        int n = a.length; // size of the array
        int xr = 0;

        //Step 1: Find XOR of all elements:
        for (int i = 0; i < n; i++) {
            xr = xr ^ a[i];
            xr = xr ^ (i + 1);
        }

        //Step 2: Find the differentiating bit number:
        int number = (xr & ~(xr - 1));

        //Step 3: Group the numbers:
        int zero = 0;
        int one = 0;
        for (int i = 0; i < n; i++) {
            //part of 1 group:
            if ((a[i] & number) != 0) {
                one = one ^ a[i];
            }
            //part of 0 group:
            else {
                zero = zero ^ a[i];
            }
        }

        for (int i = 1; i <= n; i++) {
            //part of 1 group:
            if ((i & number) != 0) {
                one = one ^ i;
            }
            //part of 0 group:
            else {
                zero = zero ^ i;
            }
        }

        // Last step: Identify the numbers:
        int cnt = 0;
        for (int i = 0; i < n; i++) {
            if (a[i] == zero) cnt++;
        }

        if (cnt == 2) return new int[] {zero, one};
        return new int[] {one, zero};
    }


    public static void main(String[] args) {
        int[] a = {3, 1, 2, 5, 4, 6, 7, 5};
        int[] ans = findMissingRepeatingNumbers(a);
        System.out.println("The repeating and missing numbers are: {"
                           + ans[0] + ", " + ans[1] + "}");
    }
}
```

**Output:** The repeating and missing numbers are: {5, 8}

Complexity Analysis

**Time Complexity:** O(N), where N = the size of the given array.  
**Reason:** We are just using some loops running for N times. So, the time complexity will be approximately O(N).

**Space Complexity:** O(1) as we are not using any extra space to solve this problem.

