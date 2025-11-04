4 Sum | Find Quads that add up to a target value

Mark as Completed

160

**Problem Statement:** Given an array of N integers, your task is to find unique quads that add up to give a target value. In short, you need to return an array of all the unique quadruplets [arr[a], arr[b], arr[c], arr[d]] such that their sum is equal to a given target.

**Pre-req:** [3-sum problem](https://takeuforward.org/data-structure/3-sum-find-triplets-that-add-up-to-a-zero/) and [2-sum problem](https://takeuforward.org/data-structure/two-sum-check-if-a-pair-with-given-sum-exists-in-array/)

**Note:**

0 <= a, b, c, d < n  
a, b, c, and d are distinct.  
arr[a] + arr[b] + arr[c] + arr[d] == target

**Examples**

**Example 1:**
**Input Format:** arr[] = [1,0,-1,0,-2,2], target = 0
**Result:** [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
**Explanation:** We have to find unique quadruplets from the array such that the sum of those elements is equal to the target sum given that is 0. The result obtained is such that the sum of the quadruplets yields 0.

**Example 2:**
**Input Format:** arr[] = [4,3,3,4,4,2,1,2,1,1], target = 9
**Result:** [[1,1,3,4],[1,2,2,4],[1,2,3,3]]
**Explanation:** The sum of all the quadruplets is equal to the target i.e. 9.

**_Disclaimer_**: _Don’t jump directly to the solution, try it out yourself first._

Practice:[Solve Problem](https://takeuforward.org/plus/dsa/problems/4-sum)

Brute Force Approach

Algorithm / Intuition

### **Solution:**

In the question, it is clearly stated that for each case the picked indices i.e. a, b, c, and d must be distinct. This means [arr[1], arr[1], arr[2], arr[3]] is not valid and also remember [arr[1], arr[0], arr[2], arr[3]] and [arr[0], arr[1], arr[2], arr[3]] will be considered same.

### **Naive Approach (Brute-force)**: 

### **Approach:**

The steps are as follows:

1. First, we will declare a set data structure as we want unique quadruplets.
2. Then we will use the first loop(say i) that will run from 0 to n-1.
3. Inside it, there will be the second loop(say j) that will run from i+1 to n-1.
4. Then there will be the third loop(say k) that runs from j+1 to n-1.
5. Inside loop k, the fourth loop(say l) will run from k+1 to n-1.
6. Now, inside these four nested loops, we will check the sum i.e. arr[i]+arr[j]+arr[k]+arr[l], and if it is equal to the target we will sort this quadruplet and insert it in the set data structure.
7. Finally, we will return a list of stored quadruplets.

**Intuition:** This approach is pretty straightforward. Here, we will check all possible quadruplets using 4 loops(_as we did in the_ [_3-sum problem_](https://takeuforward.org/data-structure/3-sum-find-triplets-that-add-up-to-a-zero/)) and among them, we will consider the ones whose sum is equal to the given target. And before considering them as our answer we need to sort the quadruplets in ascending order.

**Note:** _For a better understanding of intuition, please watch the video at the bottom of the page._

Code

C++JavaPythonJavaScript

```java



import java.util.*;

public class tUf {

    public static List<List<Integer>> fourSum(int[] nums, int target) {
        int n = nums.length; // size of the array
        Set<List<Integer>> set = new HashSet<>();

        // checking all possible quadruplets:
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                for (int k = j + 1; k < n; k++) {
                    for (int l = k + 1; l < n; l++) {
                        // taking bigger data type
                        // to avoid integer overflow:
                        long sum = (long)nums[i] + nums[j];
                        sum += nums[k];
                        sum += nums[l];

                        if (sum == target) {
                            List<Integer> temp = Arrays.asList(nums[i], nums[j], nums[k], nums[l]);
                            Collections.sort(temp);
                            set.add(temp);
                        }
                    }
                }
            }
        }
        List<List<Integer>> ans = new ArrayList<>(set);
        return ans;
    }

    public static void main(String[] args) {
        int[] nums = {4, 3, 3, 4, 4, 2, 1, 2, 1, 1};
        int target = 9;
        List<List<Integer>> ans = fourSum(nums, target);
        System.out.println("The quadruplets are: ");
        for (List<Integer> it : ans) {
            System.out.print("[");
            for (int ele : it) {
                System.out.print(ele + " ");
            }
            System.out.print("] ");
        }
        System.out.println();
    }
}


```

**Output:** The quadruplets are: [1 1 3 4 ] [1 2 2 4 ] [1 2 3 3 ] Complexity Analysis

**Time Complexity:** O(N4), where N = size of the array.  
**Reason:** Here, we are mainly using 4 nested loops. But we not considering the time complexity of sorting as we are just sorting 4 elements every time.

**Space Complexity:** O(2 * no. of the quadruplets) as we are using a set data structure and a list to store the quads.

Better Approach

Algorithm / Intuition

### **Better Approach (Using 3 loops and set data structure)**: 

### **Approach:**

The steps are as follows:

1. First, we will declare a set data structure as we want unique quadruplets.
2. Then we will use the first loop(say i) that will run from 0 to n-1.
3. Inside it, there will be the second loop(say j) that will run from i+1 to n-1.
4. Before the third loop, we will declare a HashSet to store the specific array elements as we intend to search the fourth element in that HashSet.
5. Then there will be the third loop(say k) that runs from j+1 to n-1.
6. Inside the third loop, we will calculate the value of the fourth element i.e. target - (nums[i]+nums[j]+nums[k]).
7. If the fourth element exists in the HashSet, we will sort these four values i.e. nums[i], nums[j], nums[k], and the fourth element, and insert it in the set data structure declared in step 1.
8. After that, we will insert the k-th element i.e. nums[k] in the HashSet as we only want to insert those array elements that are in between indices j and k.
9. Finally, we will return a list of stored quadruplets stored in the set data structure.

### **Intuition:**

In the previous approach, we were using 4 loops but in this, we want to reduce that to 3 loops. We have to somehow manage to calculate nums[l] as we are planning to remove the fourth loop(i.e. l). In order to calculate nums[l], we can derive a formula like the following:

nums[l] = target - (nums[i]+nums[j]+nums[k])

So, we will first calculate the triplets nums[i], nums[j], and nums[k] using 3 loops and for the fourth one i.e. nums[l] we will not use another loop and instead we will look up the value target-(nums[i]+nums[j]+nums[k]) in the array. Thus we can remove the fourth loop from the algorithm.

For implementing the search operation of the fourth element, we will use a similar technique as we used in the case of the [_3-sum problem_](https://takeuforward.org/data-structure/3-sum-find-triplets-that-add-up-to-a-zero/). We will store all the elements between the indices j and k in a HashSet and then we will search for the fourth element in the HashSet.

**Why we are not inserting all the array elements in the HashSet and then searching for the fourth element:**

Let’s understand this intuition using an example. Assume the given array is {1, 2, -1, -2, 2, 0, -1} and the target = 0. Now, we will notice a situation like the following:

![](https://static.takeuforward.org/wp/uploads/2023/04/Screenshot-2023-04-23-084157.png)

Now, the fourth element should be target-(nums[i]+nums[j]+nums[k]) = 0 - (1-1+0) = 0. Now, if all the array elements are in the HashSet and we search for 0, we will end up taking the 0 at index k again. The quadruplets will be {nums[i], nums[j], nums[k], nums[k]} but this is absolutely incorrect. That is why we need to only consider the elements that are in between the indices j and k.

**Note:** _For a better understanding of intuition, please watch the video at the bottom of the page._

Code

C++JavaPythonJavaScript

```java



import java.util.*;

public class tUf {
    public static List<List<Integer>> fourSum(int[] nums, int target) {
        int n = nums.length; // size of the array
        Set<List<Integer>> st = new HashSet<>();

        // checking all possible quadruplets:
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                Set<Long> hashset = new HashSet<>();
                for (int k = j + 1; k < n; k++) {
                    // taking bigger data type
                    // to avoid integer overflow:
                    long sum = nums[i] + nums[j];
                    sum += nums[k];
                    long fourth = target - sum;
                    if (hashset.contains(fourth)) {
                        List<Integer> temp = new ArrayList<>();
                        temp.add(nums[i]);
                        temp.add(nums[j]);
                        temp.add(nums[k]);
                        temp.add((int) fourth);
                        temp.sort(Integer::compareTo);
                        st.add(temp);
                    }
                    // put the kth element into the hashset:
                    hashset.add((long) nums[k]);
                }
            }
        }
        List<List<Integer>> ans = new ArrayList<>(st);
        return ans;
    }

    public static void main(String[] args) {
        int[] nums = {4, 3, 3, 4, 4, 2, 1, 2, 1, 1};
        int target = 9;
        List<List<Integer>> ans = fourSum(nums, target);
        System.out.println("The quadruplets are: ");
        for (List<Integer> it : ans) {
            System.out.print("[");
            for (Integer ele : it) {
                System.out.print(ele + " ");
            }
            System.out.print("] ");
        }
        System.out.println();
    }
} 
```

**Output:** The quadruplets are: [1 1 3 4 ] [1 2 2 4 ] [1 2 3 3 ] Complexity Analysis

**Time Complexity:** O(N3*log(M)), where N = size of the array, M = no. of elements in the set.  
**Reason:** Here, we are mainly using 3 nested loops, and inside the loops there are some operations on the set data structure which take log(M) time complexity.

**Space Complexity:** O(2 * no. of the quadruplets)+O(N)  
**Reason:** we are using a set data structure and a list to store the quads. This results in the first term. And the second space is taken by the set data structure we are using to store the array elements. At most, the set can contain approximately all the array elements and so the space complexity is O(N).

Optimal Approach

Algorithm / Intuition

### **Optimal Approach**: 

In this approach, we intend to get rid of two things i.e. the HashSet we were using for the look-up operation and the set data structure used to store the unique quadruplets.

Again the solution to this will be similar to the optimal approach of the  [_3-sum problem_](https://takeuforward.org/data-structure/3-sum-find-triplets-that-add-up-to-a-zero/). In that approach, we had fixed a pointer i, and the rest 2 pointers were moving. Similarly, here, as we are dealing with quads instead of triplets we will fix 2 pointers i.e. i and j and the rest of the 2 pointers will be moving.

Now, we need to first understand what the HashSet and the set were doing to make our algorithm work without them. So, the set data structure was basically storing the unique quadruplets in sorted order and the HashSet was used to search for the fourth element.

**To get the quadruplets in sorted order,** we will sort the entire array in the first step and **to get the unique quads,** we will simply skip the duplicate numbers while moving the pointers.

**How to skip duplicate numbers:**As the entire array is sorted, the duplicate numbers will be in consecutive places. So, while moving a pointer, we will check the current element and the adjacent element. Until they become different, we will move the pointer by 1 place. We will follow this process for all 4 pointers. Thus, we can easily skip the duplicate elements while moving the pointers.

Now, we can also remove the HashSet as we have two moving pointers i.e. k and l that will find the appropriate value of nums[k] and nums[l]. So, we do not need the HashSet anymore.

The process will look like the following:

![](https://static.takeuforward.org/wp/uploads/2023/04/Screenshot-2023-04-23-085109.png)

Among the 4 pointers, 2 will be fixed and 2 will be moving. In each iteration, we will check if the sum i.e. nums[i]+nums[j]+nums[k]+nums[l] is equal to the target. 

- **If the sum is greater**, then we need lesser elements and so we will decrease the value of l. 
- **If the sum is lesser than the target**, we need a bigger value and so we will increase the value of k. 
- **If the sum is equal to the target,** we will simply insert the quad i.e. nums[i], nums[j], nums[k], and nums[l] into our answer and move the pointers k and l skipping the duplicate elements.

**Approach:**

The steps are as follows:

1. First, we will sort the entire array.
2. We will use a loop(say i) that will run from 0 to n-1. This i will represent one of the fixed pointers. In each iteration, this value will be fixed for all different values of the rest of the 3 pointers. Inside the loop, we will first check if the current and the previous element is the same and if it is we will do nothing and continue to the next value of i.
3. After checking inside the loop, we will introduce another fixed pointer j(runs from i+1 to n-1) using another loop. Inside this second loop, we will again check for duplicate elements and only perform any further operation if the elements are different.
4. Inside the second loop, there will be 2 moving pointers i.e. k(starts from j+1) and l(starts from the last index). The pointer k will move forward and the pointer l will move backward until they cross each other while the value of i and j will be fixed.
    1. Now we will check the sum i.e. nums[i]+nums[j]+nums[k]+nums[l].
    2. **If the sum is greater**, then we need lesser elements and so we will decrease the value of l.
    3. **If the sum is lesser than the target**, we need a bigger value and so we will increase the value of k. 
    4. **If the sum is equal to the target,** we will simply insert the quad i.e. nums[i], nums[j], nums[k], and nums[l] into our answer and move the pointers k and l skipping the duplicate elements(_i.e. by checking the adjacent elements while moving the pointers_).
5. Finally, we will have a list of unique quadruplets.

**Note:** _For a better understanding of intuition, please watch the video at the bottom of the page._

Code

C++JavaPythonJavaScript

```java



import java.util.*;

public class tUf {
    public static List<List<Integer>> fourSum(int[] nums, int target) {
        int n = nums.length; // size of the array
        List<List<Integer>> ans = new ArrayList<>();

        // sort the given array:
        Arrays.sort(nums);

        // calculating the quadruplets:
        for (int i = 0; i < n; i++) {
            // avoid the duplicates while moving i:
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            for (int j = i + 1; j < n; j++) {
                // avoid the duplicates while moving j:
                if (j > i + 1 && nums[j] == nums[j - 1]) continue;

                // 2 pointers:
                int k = j + 1;
                int l = n - 1;
                while (k < l) {
                    long sum = nums[i];
                    sum += nums[j];
                    sum += nums[k];
                    sum += nums[l];
                    if (sum == target) {
                        List<Integer> temp = new ArrayList<>();
                        temp.add(nums[i]);
                        temp.add(nums[j]);
                        temp.add(nums[k]);
                        temp.add(nums[l]);
                        ans.add(temp);
                        k++;
                        l--;

                        // skip the duplicates:
                        while (k < l && nums[k] == nums[k - 1]) k++;
                        while (k < l && nums[l] == nums[l + 1]) l--;
                    } else if (sum < target) k++;
                    else l--;
                }
            }
        }

        return ans;
    }

    public static void main(String[] args) {
        int[] nums = {4, 3, 3, 4, 4, 2, 1, 2, 1, 1};
        int target = 9;
        List<List<Integer>> ans = fourSum(nums, target);
        System.out.println("The quadruplets are: ");
        for (List<Integer> it : ans) {
            System.out.print("[");
            for (int ele : it) {
                System.out.print(ele + " ");
            }
            System.out.print("] ");
        }
        System.out.println();
    }
}
```

**Output:**The quadruplets are: [1 1 3 4 ] [1 2 2 4 ] [1 2 3 3 ] Complexity Analysis

**Time Complexity:** O(N3), where N = size of the array.  
**Reason:** Each of the pointers i and j, is running for approximately N times. And both the pointers k and l combined can run for approximately N times including the operation of skipping duplicates. So the total time complexity will be O(N3). 

**Space Complexity:** O(no. of quadruplets), **_This space is only used to store the answer. We are not using any extra space to solve this problem._** So, from that perspective, space complexity can be written as O(1).
