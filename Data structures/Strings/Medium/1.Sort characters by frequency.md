
**Problem Statement:** You are given a string s. Return the array of unique characters, sorted by highest to lowest occurring characters.  
If two or more characters have same frequency then arrange them in alphabetic order.

**Examples**

**Example 1:****Input:** s = "tree"
**Output:** ['e', 'r', 't']
**Explanation:**
e → 2
r → 1
t → 1
Since 'r' and 't' have the same frequency, they are sorted alphabetically → 'r' comes before 't'.

**Example 2:****Input:** s = "raaaajj"
**Output:** ['a', 'j', 'r']
**Explanation:**
a → 4
j → 2
r → 1
Characters are sorted by decreasing frequency. In case of ties, alphabetically.
            

Approach

Algorithm

- The goal is to rank characters based on how frequently they appear in the string.
- We need a structure that can track both the character and how often it occurs.
- Sorting the characters by frequency helps surface the most significant ones first.
- To maintain consistency when frequencies match, tie-breaking is done alphabetically.
- Once sorted, the characters with non-zero occurrences form the final ranked result.

![Image 1](https://static.takeuforward.org/content/1.png-oZQccLkF)

![Image 2](https://static.takeuforward.org/content/2.png-pmQGqeI3)

![Image 1](https://static.takeuforward.org/content/1.png-oZQccLkF)![Image 2](https://static.takeuforward.org/content/2.png-pmQGqeI3)

Code

C++JavaPythonJavaScript

```java
import java.util.*;

// Class containing the main logic for frequency sorting
class Solution {
    
    // Method to sort characters by frequency
    public List<Character> frequencySort(String s) {
        // Array to hold frequency and character for 'a' to 'z'
        Pair[] freq = new Pair[26];
        
        // Initialize the frequency array
        for (int i = 0; i < 26; i++) {
            freq[i] = new Pair(0, (char)(i + 'a'));
        }

        // Count frequency of each character in the string
        for (char ch : s.toCharArray()) {
            freq[ch - 'a'].freq++;
        }

        // Sort array by frequency descending, then by character ascending
        Arrays.sort(freq, (p1, p2) -> {
            if (p1.freq != p2.freq) return p2.freq - p1.freq;
            return p1.ch - p2.ch;
        });

        // Collect characters with non-zero frequency into result list
        List<Character> result = new ArrayList<>();
        for (Pair p : freq) {
            if (p.freq > 0) result.add(p.ch);
        }

        // Return the final list
        return result;
    }

    // Inner class to store frequency and character
    class Pair {
        int freq;
        char ch;
        Pair(int f, char c) {
            this.freq = f;
            this.ch = c;
        }
    }
}

// Separate class to run the main method
public class Main {
    public static void main(String[] args) {
        // Create instance of Solution
        Solution sol = new Solution();

        // Input string
        String s = "tree";

        // Get characters sorted by frequency
        List<Character> result = sol.frequencySort(s);

        // Print the result
        System.out.println(result);  
    }
}
```
Complexity Analysis

**Time Complexity: O(n + k log k)**, where n is the length of the string and k is the constant 26 for the alphabet.  
  
**Space Complexity: O(k)** , where k is the constant 26 for the frequency array.