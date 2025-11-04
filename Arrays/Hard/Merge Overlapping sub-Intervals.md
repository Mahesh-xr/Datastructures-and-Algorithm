
Merge Overlapping Sub-intervals

**Problem Statement:** Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals and return an array of the non-overlapping intervals that cover all the intervals in the input.

**Examples**

Input : intervals=[[1,3],[2,6],[8,10],[15,18]]
Output : [[1,6],[8,10],[15,18]]
Explanation : Since intervals [1,3] and [2,6] are overlapping we can merge them to form [1,6] intervals.

Input : [[1,4],[4,5]]
Output :  [[1,5]]
Explanation :  Since intervals [1,4] and [4,5] are overlapping we can merge them to form [1,5].
            

**_Disclaimer_**: _Here is the [practice link](https://takeuforward.org/plus/dsa/problems/merge-overlapping-subintervals) to help you assess your knowledge better. It's highly recommend trying to solve it before looking at the solution._

Brute-Force Approach

Algorithm

The main idea is to combine intervals that overlap with each other. To do this easily, we first sort the intervals by their starting point so that all overlapping intervals come next to each other. Then, for each interval, we try to see if the next ones overlap with it. If they do, we merge them into one bigger interval. We keep doing this until we find a non-overlapping interval, and then start the process again from that point.

- Sort all intervals based on their starting points. This helps bring all overlapping intervals next to each other.
- Go through each interval one by one and if the current interval is already covered by a previously merged interval, skip it. Else, pick the current interval as the starting point of a new merged interval.
- Now run another loop to check if the following intervals overlap with the current one
- If the start of next interval is less than or equal to the end of the current merged interval, it means they overlap. Therefore, extend the end of the merged interval to be the maximum of the two ends.
- Keep doing this for the next intervals as long as they overlap. As soon as you find an interval that doesn't overlap, break the inner loop and move back to the outer loop to process the next non-overlapping interval.
- Store each merged interval in the final answer list and after the loop ends, return the list of merged intervals.

Code

CppJavaPythonJavascript

```java
import java.util.*;

class Solution {
    // Function to merge overlapping intervals using brute force
    public List<List<Integer>> merge(int[][] intervals) {

        // Sort intervals based on starting point
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

        List<List<Integer>> ans = new ArrayList<>();

        int n = intervals.length;
        int i = 0;

        // Loop through intervals
        while (i < n) {
            // Start of merged interval
            int start = intervals[i][0];
            int end = intervals[i][1];

            int j = i + 1;

            // Check all overlapping intervals
            while (j < n && intervals[j][0] <= end) {
                // Extend the end of current interval
                end = Math.max(end, intervals[j][1]);
                j++;
            }

            // Add merged interval to result
            ans.add(Arrays.asList(start, end));

            // Move to next non-overlapping interval
            i = j;
        }

        return ans;
    }
}

public class Main {
    public static void main(String[] args) {
        Solution sol = new Solution();
        int[][] intervals = {{1, 3}, {2, 6}, {8, 10}, {15, 18}};
        List<List<Integer>> result = sol.merge(intervals);
        for (List<Integer> interval : result) {
            System.out.print(interval + " ");
        }
    }
}
```
Complexity Analysis**Time Complexity: O(N^2)**, for every interval we check all future intervals.  
**Space Complexity: ON)**, additonal space used to store the non-overlapping intervals.

Optimal Approach

Algorithm

Imagine laying intervals out on a number line. If two intervals overlap, we can combine them into one, like merging blocks that touch or overlap.  
  
Instead of checking each interval with every other one (as in brute-force), we first sort the intervals, so that any overlapping intervals will come one after the other. This way, we only need to compare each interval with the last one added to our answer. If they overlap, we merge them. If they don’t, we simply add the current interval as a new entry.

- Sort the intervals based on their starting points. This ensures overlapping intervals come together.
- Initialize an empty list to store the final merged intervals.
- If the list is empty or the current interval starts after the last one ends, it means there is no overlap, so just add it to the list.
- If the current interval starts before or exactly at the end of the last one, it means there is overlap. So, combine both by extending the end of the last one to the further end of the two.
- Keep doing this until all intervals have been checked. The final list will now contain only non-overlapping, merged intervals.

![](https://static.takeuforward.org/content/mosi-KSps89xM)

Code

CppJavaPythonJavascript

```java
import java.util.*;

class Solution {
    // Function to merge overlapping intervals
    public List<List<Integer>> merge(int[][] intervals) {
        // Sort intervals based on start time
        Arrays.sort(
            intervals,
            (a, b) -> Integer.compare(a[0], b[0])
        );

        // List to store merged intervals
        List<List<Integer>> merged = new ArrayList<>();

        // Traverse through all intervals
        for (int[] interval : intervals) {
            // If merged list is empty or no overlap
            if (
                merged.isEmpty() ||
                merged.get(merged.size() - 1).get(1) < interval[0]
            ) {
                // Add current interval as a new block
                merged.add(
                    Arrays.asList(interval[0], interval[1])
                );
            } else {
                // Overlapping: update end to max of both
                int last = merged.size() - 1;
                int maxEnd = Math.max(
                    merged.get(last).get(1),
                    interval[1]
                );
                merged.get(last).set(1, maxEnd);
            }
        }

        return merged;
    }
}

class Main {
    public static void main(String[] args) {
        Solution sol = new Solution();
        int[][] intervals = {
            {1, 3}, {2, 6}, {8, 10}, {15, 18}
        };

        List<List<Integer>> result = sol.merge(intervals);

        for (List<Integer> interval : result) {
            System.out.print(
                "[" + interval.get(0) + "," + interval.get(1) + "] "
            );
        }
    }
}
```

```java
class Solution {

    public int[][] merge(int[][] arr) {

        List<List<Integer>> ans = new ArrayList<>();

        Arrays.sort(arr, (a, b) -> a[0] - b[0]);

  
  

        for(int[]  num : arr){

            int current_st = num[0];

            int current_end = num[1];

            if(ans.isEmpty() || current_st > ans.get(ans.size() - 1).get(1) ){

                ans.add(Arrays.asList(current_st, current_end));

  

            }

            else{

                int last = ans.size() - 1;

                int end = Math.max(current_end, ans.get(last).get(1));

                ans.get(last).set(1, end);

  

            }

        }

  

        int[][] res = new int[ans.size()][2];

        for(int i = 0; i< ans.size(); i++){

            res[i][0] = ans.get(i).get(0);

            res[i][1] = ans.get(i).get(1);

        }

        return res;

    }

}
```
Complexity Analysis**Time Complexity: O(N*logN) + O(N)**, we sort the entire array and then merge them in a single pass.  
**Space Complexity: ON)**, additonal space used to store the non-overlapping intervals.

