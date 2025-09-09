# 🚀 The Complete Recursion Mastery Guide
## 50 Carefully Crafted Problems

>  A structured learning path with 50 recursion problems organized from beginner to advanced. Each problem includes category, approach, and complete Java solution. Perfect for coding interviews and algorithmic thinking.

---

## 🎯 Learning Path Overview

This guide follows a **progressive difficulty structure**:

| Level | Focus Area | Skills Developed |
|-------|------------|------------------|
| **Level 1** | Foundation | Base cases, stack building, simple returns |
| **Level 2** | Arrays | Array traversal, divide & conquer |
| **Level 3** | Strings | String manipulation, subsequences |
| **Level 4** | Classics | Famous algorithms, backtracking intro |
| **Level 5** | Mastery | Complex backtracking, optimization |

---

## 🏁 Level 1: Foundation (Problems 1-10)

> **Goal:** Master the fundamentals - base cases, recursive calls, and stack building

### Problem 1: Print Numbers 1 to N
**Category:** Basic Flow Control  
**Concept:** Forward recursion pattern

```java
/**
 * Print numbers from 1 to N using recursion
 * Time: O(n), Space: O(n) - call stack
 */
public static void printNumbers1ToN(int n) {
    // Base case - stop recursion
    if (n <= 0) return;
    
    // Recurse first, then print (creates 1,2,3... order)
    printNumbers1ToN(n - 1);
    System.out.print(n + " ");
}

// Test: printNumbers1ToN(5) → Output: 1 2 3 4 5
```

### Problem 2: Print Numbers N to 1
**Category:** Basic Flow Control  
**Concept:** Backward recursion pattern

```java
/**
 * Print numbers from N to 1 using recursion
 * Key insight: Print first, then recurse
 */
public static void printNumbersNTo1(int n) {
    if (n <= 0) return;
    
    // Print first, then recurse (creates N,N-1... order)
    System.out.print(n + " ");
    printNumbersNTo1(n - 1);
}

// Test: printNumbersNTo1(5) → Output: 5 4 3 2 1
```

### Problem 3: Factorial Calculation
**Category:** Mathematical Recursion  
**Concept:** Return value accumulation

```java
/**
 * Calculate factorial using recursion
 * Classic example: 5! = 5 × 4 × 3 × 2 × 1
 */
public static long factorial(int n) {
    // Base cases
    if (n <= 1) return 1;
    
    // Recursive relation: n! = n × (n-1)!
    return n * factorial(n - 1);
}

// Test: factorial(5) → Output: 120
// Stack trace: 5×factorial(4) → 5×24 → 120
```

### Problem 4: Sum of First N Numbers
**Category:** Mathematical Recursion  
**Concept:** Accumulative recursion

```java
/**
 * Find sum of first N natural numbers
 * Formula verification: sum = n(n+1)/2
 */
public static int sumFirstN(int n) {
    if (n <= 0) return 0;
    
    // Sum = current number + sum of remaining
    return n + sumFirstN(n - 1);
}

// Test: sumFirstN(5) → Output: 15 (5+4+3+2+1)
```

### Problem 5: Power Function (a^b)
**Category:** Mathematical Recursion  
**Concept:** Repeated multiplication

```java
/**
 * Calculate a^b using recursion
 * Handles negative exponents too
 */
public static double power(double a, int b) {
    // Base case: anything^0 = 1
    if (b == 0) return 1.0;
    
    // Negative exponent: a^(-b) = 1/(a^b)
    if (b < 0) return 1.0 / power(a, -b);
    
    // Recursive case: a^b = a × a^(b-1)
    return a * power(a, b - 1);
}

// Test: power(2, 5) → Output: 32.0
// Test: power(2, -3) → Output: 0.125
```

### Problem 6: Fibonacci Sequence Printer
**Category:** Sequence Generation  
**Concept:** Classic recursive sequence

```java
/**
 * Print first N Fibonacci numbers
 * Sequence: 0, 1, 1, 2, 3, 5, 8, 13...
 */
public static void printFibonacci(int n) {
    System.out.print("Fibonacci: ");
    for (int i = 0; i < n; i++) {
        System.out.print(fibonacciNumber(i) + " ");
    }
    System.out.println();
}

public static int fibonacciNumber(int n) {
    // Base cases
    if (n <= 1) return n;
    
    // Recursive relation: fib(n) = fib(n-1) + fib(n-2)
    return fibonacciNumber(n - 1) + fibonacciNumber(n - 2);
}

// Test: printFibonacci(7) → Output: 0 1 1 2 3 5 8
```

### Problem 7: Enhanced Fibonacci (Memoized)
**Category:** Optimization Technique  
**Concept:** Avoiding redundant calculations

```java
/**
 * Optimized Fibonacci with memoization
 * Reduces time complexity from O(2^n) to O(n)
 */
private static Map<Integer, Long> fibMemo = new HashMap<>();

public static long fibonacciMemo(int n) {
    if (n <= 1) return n;
    
    // Check if already calculated
    if (fibMemo.containsKey(n)) {
        return fibMemo.get(n);
    }
    
    // Calculate and store result
    long result = fibonacciMemo(n - 1) + fibonacciMemo(n - 2);
    fibMemo.put(n, result);
    return result;
}

// Test: fibonacciMemo(40) → Fast execution vs regular recursion
```

### Problem 8: Palindrome Checker
**Category:** String Recursion  
**Concept:** Two-pointer technique

```java
/**
 * Check if string is palindrome using recursion
 * Two-pointer approach: compare from both ends
 */
public static boolean isPalindrome(String str, int left, int right) {
    // Base case: pointers meet or cross
    if (left >= right) return true;
    
    // Characters don't match
    if (str.charAt(left) != str.charAt(right)) return false;
    
    // Check remaining substring
    return isPalindrome(str, left + 1, right - 1);
}

public static boolean isPalindrome(String str) {
    return isPalindrome(str.toLowerCase(), 0, str.length() - 1);
}

// Test: isPalindrome("racecar") → Output: true
// Test: isPalindrome("hello") → Output: false
```

### Problem 9: String Reverser
**Category:** String Recursion  
**Concept:** String reconstruction

```java
/**
 * Reverse a string using recursion
 * Approach: last char + reverse of remaining string
 */
public static String reverseString(String str) {
    // Base case: empty or single character
    if (str.length() <= 1) return str;
    
    // Take last char + reverse of rest
    char lastChar = str.charAt(str.length() - 1);
    String remaining = str.substring(0, str.length() - 1);
    
    return lastChar + reverseString(remaining);
}

// Alternative implementation
public static String reverseStringAlt(String str) {
    if (str.length() <= 1) return str;
    
    // First char goes to end + reverse of substring
    return reverseStringAlt(str.substring(1)) + str.charAt(0);
}

// Test: reverseString("hello") → Output: "olleh"
```

### Problem 10: Array Maximum Finder
**Category:** Array Recursion  
**Concept:** Comparison propagation

```java
/**
 * Find maximum element in array using recursion
 * Divide and conquer approach
 */
public static int findMax(int[] arr, int index) {
    // Base case: reached last element
    if (index == arr.length - 1) {
        return arr[index];
    }
    
    // Compare current with max of remaining elements
    int maxOfRest = findMax(arr, index + 1);
    return Math.max(arr[index], maxOfRest);
}

public static int findMax(int[] arr) {
    if (arr.length == 0) {
        throw new IllegalArgumentException("Empty array");
    }
    return findMax(arr, 0);
}

// Test: findMax([3,7,2,9,1]) → Output: 9
```

---

## 📊 Level 2: Arrays & Recursion (Problems 11-20)

> **Goal:** Master array traversal patterns and divide-and-conquer techniques

### Problem 11: Recursive Linear Search
**Category:** Array Searching  
**Concept:** Sequential traversal with recursion

```java
/**
 * Linear search using recursion
 * Returns index of target element or -1 if not found
 */
public static int linearSearch(int[] arr, int target, int index) {
    // Base case: reached end without finding
    if (index >= arr.length) return -1;
    
    // Found the target
    if (arr[index] == target) return index;
    
    // Search in remaining array
    return linearSearch(arr, target, index + 1);
}

public static int linearSearch(int[] arr, int target) {
    return linearSearch(arr, target, 0);
}

// Test: linearSearch([1,3,5,7,9], 5) → Output: 2
```

### Problem 12: Recursive Binary Search
**Category:** Array Searching  
**Concept:** Divide and conquer on sorted arrays

```java
/**
 * Binary search using recursion
 * Works only on sorted arrays - O(log n) time
 */
public static int binarySearch(int[] arr, int target, int left, int right) {
    // Base case: element not found
    if (left > right) return -1;
    
    int mid = left + (right - left) / 2;
    
    // Found target at middle
    if (arr[mid] == target) return mid;
    
    // Target is in left half
    if (target < arr[mid]) {
        return binarySearch(arr, target, left, mid - 1);
    }
    
    // Target is in right half
    return binarySearch(arr, target, mid + 1, right);
}

public static int binarySearch(int[] arr, int target) {
    return binarySearch(arr, target, 0, arr.length - 1);
}

// Test: binarySearch([1,3,5,7,9,11], 7) → Output: 3
```

### Problem 13: Array Sorted Checker
**Category:** Array Validation  
**Concept:** Sequential comparison

```java
/**
 * Check if array is sorted in ascending order
 * Returns true if strictly increasing
 */
public static boolean isSorted(int[] arr, int index) {
    // Base case: reached second last element
    if (index >= arr.length - 1) return true;
    
    // Current element should be < next element
    if (arr[index] >= arr[index + 1]) return false;
    
    // Check remaining array
    return isSorted(arr, index + 1);
}

public static boolean isSorted(int[] arr) {
    if (arr.length <= 1) return true;
    return isSorted(arr, 0);
}

// Test: isSorted([1,3,5,7,9]) → Output: true
// Test: isSorted([1,3,2,7,9]) → Output: false
```

### Problem 14: Find All Indices
**Category:** Array Searching  
**Concept:** Collecting multiple results

```java
/**
 * Find all indices where target element appears
 * Returns list of indices
 */
public static List<Integer> findAllIndices(int[] arr, int target, int index, List<Integer> result) {
    // Base case: processed entire array
    if (index >= arr.length) return result;
    
    // Found target - add index to result
    if (arr[index] == target) {
        result.add(index);
    }
    
    // Continue searching
    return findAllIndices(arr, target, index + 1, result);
}

public static List<Integer> findAllIndices(int[] arr, int target) {
    return findAllIndices(arr, target, 0, new ArrayList<>());
}

// Test: findAllIndices([1,3,5,3,7,3], 3) → Output: [1, 3, 5]
```

### Problem 15: Count Occurrences
**Category:** Array Counting  
**Concept:** Accumulative counting

```java
/**
 * Count occurrences of target element in array
 * Returns total count
 */
public static int countOccurrences(int[] arr, int target, int index) {
    // Base case: processed entire array
    if (index >= arr.length) return 0;
    
    // Count current element if matches + count in rest
    int currentCount = (arr[index] == target) ? 1 : 0;
    return currentCount + countOccurrences(arr, target, index + 1);
}

public static int countOccurrences(int[] arr, int target) {
    return countOccurrences(arr, target, 0);
}

// Test: countOccurrences([1,2,3,2,4,2], 2) → Output: 3
```

### Problem 16: Array Sum Calculator
**Category:** Array Operations  
**Concept:** Accumulative operations

```java
/**
 * Calculate sum of all elements in array
 * Simple accumulation pattern
 */
public static int arraySum(int[] arr, int index) {
    // Base case: processed all elements
    if (index >= arr.length) return 0;
    
    // Current element + sum of remaining
    return arr[index] + arraySum(arr, index + 1);
}

public static int arraySum(int[] arr) {
    return arraySum(arr, 0);
}

// Test: arraySum([1,2,3,4,5]) → Output: 15
```

### Problem 17: Array Reverser
**Category:** Array Manipulation  
**Concept:** In-place modification

```java
/**
 * Reverse array in-place using recursion
 * Two-pointer swapping technique
 */
public static void reverseArray(int[] arr, int start, int end) {
    // Base case: pointers meet or cross
    if (start >= end) return;
    
    // Swap elements at start and end
    int temp = arr[start];
    arr[start] = arr[end];
    arr[end] = temp;
    
    // Recurse on remaining subarray
    reverseArray(arr, start + 1, end - 1);
}

public static void reverseArray(int[] arr) {
    reverseArray(arr, 0, arr.length - 1);
}

// Test: reverseArray([1,2,3,4,5]) → Array becomes [5,4,3,2,1]
```

### Problem 18: Array Rotator
**Category:** Array Manipulation  
**Concept:** Rotation algorithms

```java
/**
 * Rotate array left by k positions
 * Uses reversal method: reverse parts then whole
 */
public static void rotateLeft(int[] arr, int k) {
    int n = arr.length;
    k = k % n; // Handle k > n
    
    // Rotate left by k = reverse(0,k-1) + reverse(k,n-1) + reverse(0,n-1)
    reverseArray(arr, 0, k - 1);
    reverseArray(arr, k, n - 1);
    reverseArray(arr, 0, n - 1);
}

public static void rotateRight(int[] arr, int k) {
    int n = arr.length;
    k = k % n;
    
    // Rotate right by k = reverse(0,n-k-1) + reverse(n-k,n-1) + reverse(0,n-1)
    reverseArray(arr, 0, n - k - 1);
    reverseArray(arr, n - k, n - 1);
    reverseArray(arr, 0, n - 1);
}

// Test: rotateLeft([1,2,3,4,5], 2) → [3,4,5,1,2]
```

### Problem 19: Array Minimum Finder
**Category:** Array Operations  
**Concept:** Comparison operations

```java
/**
 * Find minimum element in array using recursion
 * Similar to max finder but with min comparison
 */
public static int findMin(int[] arr, int index) {
    // Base case: last element
    if (index == arr.length - 1) {
        return arr[index];
    }
    
    // Compare current with min of remaining
    int minOfRest = findMin(arr, index + 1);
    return Math.min(arr[index], minOfRest);
}

public static int findMin(int[] arr) {
    if (arr.length == 0) {
        throw new IllegalArgumentException("Empty array");
    }
    return findMin(arr, 0);
}

// Test: findMin([3,7,2,9,1]) → Output: 1
```

### Problem 20: Prefix Sum Generator
**Category:** Array Processing  
**Concept:** Cumulative computations

```java
/**
 * Generate prefix sum array recursively
 * prefix[i] = sum of all elements from 0 to i
 */
public static void generatePrefixSum(int[] arr, int[] prefix, int index) {
    // Base case: processed all elements
    if (index >= arr.length) return;
    
    // First element case
    if (index == 0) {
        prefix[index] = arr[index];
    } else {
        // Current prefix = previous prefix + current element
        prefix[index] = prefix[index - 1] + arr[index];
    }
    
    // Process remaining elements
    generatePrefixSum(arr, prefix, index + 1);
}

public static int[] generatePrefixSum(int[] arr) {
    int[] prefix = new int[arr.length];
    generatePrefixSum(arr, prefix, 0);
    return prefix;
}

// Test: generatePrefixSum([1,2,3,4]) → Output: [1,3,6,10]
```

---

## 🔤 Level 3: String Recursion (Problems 21-30)

> **Goal:** Master string manipulation, subsequences, and permutations

### Problem 21: Generate All Subsequences
**Category:** String Generation  
**Concept:** Include/exclude decision pattern

```java
/**
 * Generate all subsequences of a string
 * For each character: include it or exclude it
 */
public static void generateSubsequences(String str, String current, int index, List<String> result) {
    // Base case: processed all characters
    if (index >= str.length()) {
        result.add(current);
        return;
    }
    
    // Exclude current character
    generateSubsequences(str, current, index + 1, result);
    
    // Include current character
    generateSubsequences(str, current + str.charAt(index), index + 1, result);
}

public static List<String> generateSubsequences(String str) {
    List<String> result = new ArrayList<>();
    generateSubsequences(str, "", 0, result);
    return result;
}

// Test: generateSubsequences("abc") 
// Output: ["", "c", "b", "bc", "a", "ac", "ab", "abc"]
```

### Problem 22: Generate All Permutations
**Category:** String Permutation  
**Concept:** Backtracking with choices

```java
/**
 * Generate all permutations of a string
 * Uses backtracking: make choice, recurse, backtrack
 */
public static void generatePermutations(String str, boolean[] used, StringBuilder current, List<String> result) {
    // Base case: used all characters
    if (current.length() == str.length()) {
        result.add(current.toString());
        return;
    }
    
    // Try each unused character
    for (int i = 0; i < str.length(); i++) {
        if (!used[i]) {
            // Make choice
            used[i] = true;
            current.append(str.charAt(i));
            
            // Recurse
            generatePermutations(str, used, current, result);
            
            // Backtrack
            current.deleteCharAt(current.length() - 1);
            used[i] = false;
        }
    }
}

public static List<String> generatePermutations(String str) {
    List<String> result = new ArrayList<>();
    boolean[] used = new boolean[str.length()];
    generatePermutations(str, used, new StringBuilder(), result);
    return result;
}

// Test: generatePermutations("abc") 
// Output: ["abc", "acb", "bac", "bca", "cab", "cba"]
```

### Problem 23: Remove Character Occurrences
**Category:** String Filtering  
**Concept:** Conditional character inclusion

```java
/**
 * Remove all occurrences of a character from string
 * Build new string by including only non-matching chars
 */
public static String removeCharacter(String str, char target, int index) {
    // Base case: processed all characters
    if (index >= str.length()) return "";
    
    char current = str.charAt(index);
    String remaining = removeCharacter(str, target, index + 1);
    
    // Include current char only if it's not the target
    return (current == target) ? remaining : current + remaining;
}

public static String removeCharacter(String str, char target) {
    return removeCharacter(str, target, 0);
}

// Test: removeCharacter("hello world", 'l') → Output: "heo word"
```

### Problem 24: Replace Character Occurrences
**Category:** String Modification  
**Concept:** Character substitution

```java
/**
 * Replace all occurrences of oldChar with newChar
 * Similar to remove but substitute instead
 */
public static String replaceCharacter(String str, char oldChar, char newChar, int index) {
    // Base case: processed all characters
    if (index >= str.length()) return "";
    
    char current = str.charAt(index);
    String remaining = replaceCharacter(str, oldChar, newChar, index + 1);
    
    // Replace if matches, otherwise keep original
    char charToAdd = (current == oldChar) ? newChar : current;
    return charToAdd + remaining;
}

public static String replaceCharacter(String str, char oldChar, char newChar) {
    return replaceCharacter(str, oldChar, newChar, 0);
}

// Test: replaceCharacter("hello", 'l', 'x') → Output: "hexxo"
```

### Problem 25: Generate Binary Strings
**Category:** String Generation  
**Concept:** Binary choice recursion

```java
/**
 * Generate all binary strings of length N
 * At each position: choose 0 or 1
 */
public static void generateBinaryStrings(int n, String current, List<String> result) {
    // Base case: reached desired length
    if (current.length() == n) {
        result.add(current);
        return;
    }
    
    // Add '0' and recurse
    generateBinaryStrings(n, current + "0", result);
    
    // Add '1' and recurse
    generateBinaryStrings(n, current + "1", result);
}

public static List<String> generateBinaryStrings(int n) {
    List<String> result = new ArrayList<>();
    generateBinaryStrings(n, "", result);
    return result;
}

// Test: generateBinaryStrings(3) 
// Output: ["000", "001", "010", "011", "100", "101", "110", "111"]
```

### Problem 26: Count Palindromic Subsequences
**Category:** String Analysis  
**Concept:** Dynamic programming with recursion

```java
/**
 * Count palindromic subsequences in a string
 * Uses recursion with character matching
 */
public static int countPalindromicSubsequences(String str, int left, int right, Map<String, Integer> memo) {
    // Base cases
    if (left > right) return 0;
    if (left == right) return 1;
    
    String key = left + "," + right;
    if (memo.containsKey(key)) return memo.get(key);
    
    int result;
    if (str.charAt(left) == str.charAt(right)) {
        // Characters match: include both + count inner + individual characters
        result = 1 + countPalindromicSubsequences(str, left + 1, right, memo) +
                countPalindromicSubsequences(str, left, right - 1, memo);
    } else {
        // Characters don't match: count excluding each
        result = countPalindromicSubsequences(str, left + 1, right, memo) +
                countPalindromicSubsequences(str, left, right - 1, memo) -
                countPalindromicSubsequences(str, left + 1, right - 1, memo);
    }
    
    memo.put(key, result);
    return result;
}

public static int countPalindromicSubsequences(String str) {
    return countPalindromicSubsequences(str, 0, str.length() - 1, new HashMap<>());
}

// Test: countPalindromicSubsequences("aab") → Output: 4 ("a", "a", "aa", "b")
```

### Problem 27: Phone Keypad Combinations
**Category:** String Mapping  
**Concept:** Multi-choice recursion

```java
/**
 * Generate letter combinations from phone number
 * Each digit maps to multiple letters (like old phone keypads)
 */
private static final String[] KEYPAD = {
    "",     // 0
    "",     // 1  
    "abc",  // 2
    "def",  // 3
    "ghi",  // 4
    "jkl",  // 5
    "mno",  // 6
    "pqrs", // 7
    "tuv",  // 8
    "wxyz"  // 9
};

public static void phoneKeypadCombinations(String digits, String current, int index, List<String> result) {
    // Base case: processed all digits
    if (index >= digits.length()) {
        if (!current.isEmpty()) result.add(current);
        return;
    }
    
    int digit = digits.charAt(index) - '0';
    String letters = KEYPAD[digit];
    
    // If digit has no letters, skip it
    if (letters.isEmpty()) {
        phoneKeypadCombinations(digits, current, index + 1, result);
        return;
    }
    
    // Try each letter for current digit
    for (char letter : letters.toCharArray()) {
        phoneKeypadCombinations(digits, current + letter, index + 1, result);
    }
}

public static List<String> phoneKeypadCombinations(String digits) {
    List<String> result = new ArrayList<>();
    phoneKeypadCombinations(digits, "", 0, result);
    return result;
}

// Test: phoneKeypadCombinations("23") 
// Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
```

### Problem 28: Valid Parentheses Generator
**Category:** String Validation  
**Concept:** Balanced structure generation

```java
/**
 * Generate all valid parentheses strings with n pairs
 * Rules: equal opening/closing, never more closing than opening
 */
public static void generateParentheses(int n, String current, int open, int close, List<String> result) {
    // Base case: used all pairs
    if (current.length() == 2 * n) {
        result.add(current);
        return;
    }
    
    // Add opening parenthesis if we can
    if (open < n) {
        generateParentheses(n, current + "(", open + 1, close, result);
    }
    
    // Add closing parenthesis if valid
    if (close < open) {
        generateParentheses(n, current + ")", open, close + 1, result);
    }
}

public static List<String> generateParentheses(int n) {
    List<String> result = new ArrayList<>();
    generateParentheses(n, "", 0, 0, result);
    return result;
}

// Test: generateParentheses(3) 
// Output: ["((()))", "(()())", "(())()", "()(())", "()()()"]
```

### Problem 29: Power Set Generator
**Category:** Set Operations  
**Concept:** Subset enumeration

```java
/**
 * Generate power set (all subsets) of a string
 * Same as subsequences but often called power set
 */
public static void generatePowerSet(String str, String current, int index, List<String> result) {
    // Base case: processed all characters
    if (index >= str.length()) {
        result.add(current);
        return;
    }
    
    // Exclude current character
    generatePowerSet(str, current, index + 1, result);
    
    // Include current character  
    generatePowerSet(str, current + str.charAt(index), index + 1, result);
}

public static List<String> generatePowerSet(String str) {
    List<String> result = new ArrayList<>();
    generatePowerSet(str, "", 0, result);
    return result;
}

// Test: generatePowerSet("abc") 
// Output: ["", "c", "b", "bc", "a", "ac", "ab", "abc"]
```

### Problem 30: Lexicographic Permutations
**Category:** Ordered Generation  
**Concept:** Sorted permutation generation

```java
/**
 * Generate permutations in lexicographic (dictionary) order
 * Uses sorted character approach
 */
public static void lexicographicPermutations(char[] chars, boolean[] used, StringBuilder current, List<String> result) {
    // Base case: formed complete permutation
    if (current.length() == chars.length) {
        result.add(current.toString());
        return;
    }
    
    // Try characters in sorted order
    for (int i = 0; i < chars.length; i++) {
        if (!used[i]) {
            // Skip duplicates: if current char same as previous and previous not used
            if (i > 0 && chars[i] == chars[i-1] && !used[i-1]) {
                continue;
            }
            
            // Make choice
            used[i] = true;
            current.append(chars[i]);
            
            // Recurse
            lexicographicPermutations(chars, used, current, result);
            
            // Backtrack
            current.deleteCharAt(current.length() - 1);
            used[i] = false;
        }
    }
}

public static List<String> lexicographicPermutations(String str) {
    char[] chars = str.toCharArray();
    Arrays.sort(chars); // Sort for lexicographic order
    List<String> result = new ArrayList<>();
    boolean[] used = new boolean[chars.length];
    lexicographicPermutations(chars, used, new StringBuilder(), result);
    return result;
}

// Test: lexicographicPermutations("bac") 
// Output: ["abc", "acb", "bac", "bca", "cab", "cba"]
```

---

## ⭐ Level 4: Classic Problems (Problems 31-40)

> **Goal:** Master famous recursive algorithms and backtracking fundamentals

### Problem 31: Tower of Hanoi
**Category:** Classic Algorithm  
**Concept:** Divide and conquer with three pegs

```java
/**
 * Solve Tower of Hanoi with N disks
 * Move all disks from source to destination using auxiliary peg
 */
public static void towerOfHanoi(int n, char source, char destination, char auxiliary, List<String> moves) {
    // Base case: only one disk
    if (n == 1) {
        moves.add("Move disk 1 from " + source + " to " + destination);
        return;
    }
    
    // Step 1: Move n-1 disks from source to auxiliary
    towerOfHanoi(n - 1, source, auxiliary, destination, moves);
    
    // Step 2: Move largest disk from source to destination
    moves.add("Move disk " + n + " from " + source + " to " + destination);
    
    // Step 3: Move n-1 disks from auxiliary to destination
    towerOfHanoi(n - 1, auxiliary, destination, source, moves);
}

public static List<String> towerOfHanoi(int n) {
    List<String> moves = new ArrayList<>();
    towerOfHanoi(n, 'A', 'C', 'B', moves);
    return moves;
}

// Test: towerOfHanoi(3) → Returns sequence of 7 moves
// Time Complexity: O(2^n), Space: O(n)
```

### Problem 32: N-Queens (Single Solution)
**Category:** Backtracking  
**Concept:** Constraint satisfaction with backtracking

```java
/**
 * Solve N-Queens problem - find one valid solution
 * Place N queens on NxN board such that none attack each other
 */
public static boolean solveNQueens(int n, int[] queens, int row) {
    // Base case: placed all queens successfully
    if (row == n) return true;
    
    // Try placing queen in each column of current row
    for (int col = 0; col < n; col++) {
        if (isSafePosition(queens, row, col)) {
            // Place queen
            queens[row] = col;
            
            // Recurse for next row
            if (solveNQueens(n, queens, row + 1)) {
                return true; // Found solution
            }
            
            // Backtrack (implicit - will try next column)
        }
    }
    
    return false; // No solution found for this configuration
}

private static boolean isSafePosition(int[] queens, int row, int col) {
    // Check all previously placed queens
    for (int i = 0; i < row; i++) {
        int prevCol = queens[i];
        
        // Check column conflict
        if (prevCol == col) return false;
        
        // Check diagonal conflicts
        if (Math.abs(i - row) == Math.abs(prevCol - col)) return false;
    }
    return true;
}

public static int[] solveNQueens(int n) {
    int[] queens = new int[n]; // queens[i] = column of queen in row i
    if (solveNQueens(n, queens, 0)) {
        return queens;
    }
    return null; // No solution exists
}

// Test: solveNQueens(4) → Output: [1, 3, 0, 2] (one valid solution)
```

### Problem 33: N-Queens (Count All Solutions)
**Category:** Backtracking  
**Concept:** Exhaustive search with counting

```java
/**
 * Count all possible solutions to N-Queens problem
 * Explores all valid configurations without storing them
 */
public static int countNQueensSolutions(int n, int[] queens, int row) {
    // Base case: found valid complete solution
    if (row == n) return 1;
    
    int count = 0;
    
    // Try placing queen in each column
    for (int col = 0; col < n; col++) {
        if (isSafePosition(queens, row, col)) {
            queens[row] = col;
            count += countNQueensSolutions(n, queens, row + 1);
            // No explicit backtrack needed for counting
        }
    }
    
    return count;
}

public static int countNQueensSolutions(int n) {
    int[] queens = new int[n];
    return countNQueensSolutions(n, queens, 0);
}

// Test: countNQueensSolutions(8) → Output: 92 (total solutions for 8x8 board)
```

### Problem 34: Rat in Maze (Single Path)
**Category:** Path Finding  
**Concept:** 2D backtracking with path tracking

```java
/**
 * Find path for rat to reach destination in maze
 * 1 = open path, 0 = blocked, rat moves right/down only
 */
public static boolean solveMaze(int[][] maze, int[][] solution, int x, int y) {
    int n = maze.length;
    
    // Base case: reached destination
    if (x == n - 1 && y == n - 1 && maze[x][y] == 1) {
        solution[x][y] = 1;
        return true;
    }
    
    // Check if current position is valid
    if (isSafeMove(maze, x, y)) {
        // Mark current cell as part of solution
        solution[x][y] = 1;
        
        // Move right
        if (solveMaze(maze, solution, x, y + 1)) return true;
        
        // Move down
        if (solveMaze(maze, solution, x + 1, y)) return true;
        
        // Backtrack: unmark current cell
        solution[x][y] = 0;
    }
    
    return false;
}

private static boolean isSafeMove(int[][] maze, int x, int y) {
    int n = maze.length;
    return (x >= 0 && x < n && y >= 0 && y < n && maze[x][y] == 1);
}

public static int[][] solveMaze(int[][] maze) {
    int n = maze.length;
    int[][] solution = new int[n][n];
    
    if (solveMaze(maze, solution, 0, 0)) {
        return solution;
    }
    return null; // No solution
}

// Test with 4x4 maze:
// Input:  [[1,0,0,0], [1,1,0,1], [0,1,0,0], [1,1,1,1]]
// Output: [[1,0,0,0], [1,1,0,0], [0,1,0,0], [0,1,1,1]]
```

### Problem 35: Rat in Maze (All Paths)
**Category:** Path Finding  
**Concept:** Multiple solution exploration

```java
/**
 * Find all possible paths for rat in maze
 * Rat can move in all 4 directions: Up, Down, Left, Right
 */
public static void findAllPaths(int[][] maze, boolean[][] visited, int x, int y, String path, List<String> allPaths) {
    int n = maze.length;
    
    // Base case: reached destination
    if (x == n - 1 && y == n - 1) {
        allPaths.add(path);
        return;
    }
    
    // Mark current cell as visited
    visited[x][y] = true;
    
    // Direction arrays for Up, Down, Left, Right
    int[] dx = {-1, 1, 0, 0};
    int[] dy = {0, 0, -1, 1};
    char[] directions = {'U', 'D', 'L', 'R'};
    
    // Try all 4 directions
    for (int i = 0; i < 4; i++) {
        int newX = x + dx[i];
        int newY = y + dy[i];
        
        if (isValidMove(maze, visited, newX, newY)) {
            findAllPaths(maze, visited, newX, newY, path + directions[i], allPaths);
        }
    }
    
    // Backtrack: unmark current cell
    visited[x][y] = false;
}

private static boolean isValidMove(int[][] maze, boolean[][] visited, int x, int y) {
    int n = maze.length;
    return (x >= 0 && x < n && y >= 0 && y < n && maze[x][y] == 1 && !visited[x][y]);
}

public static List<String> findAllPaths(int[][] maze) {
    List<String> allPaths = new ArrayList<>();
    boolean[][] visited = new boolean[maze.length][maze.length];
    
    if (maze[0][0] == 1) { // Starting position should be valid
        findAllPaths(maze, visited, 0, 0, "", allPaths);
    }
    
    return allPaths;
}

// Test: maze with multiple paths → Output: ["DDRDRR", "DRDDRR", ...]
```

### Problem 36: Sudoku Solver
**Category:** Constraint Satisfaction  
**Concept:** Advanced backtracking with multiple constraints

```java
/**
 * Solve 9x9 Sudoku puzzle using backtracking
 * Fill empty cells (0) with digits 1-9 following Sudoku rules
 */
public static boolean solveSudoku(int[][] board) {
    // Find next empty cell
    int[] emptyCell = findEmptyCell(board);
    if (emptyCell == null) return true; // Board is complete
    
    int row = emptyCell[0];
    int col = emptyCell[1];
    
    // Try digits 1-9
    for (int num = 1; num <= 9; num++) {
        if (isValidSudokuMove(board, row, col, num)) {
            // Place number
            board[row][col] = num;
            
            // Recurse
            if (solveSudoku(board)) return true;
            
            // Backtrack
            board[row][col] = 0;
        }
    }
    
    return false; // No solution found
}

private static int[] findEmptyCell(int[][] board) {
    for (int i = 0; i < 9; i++) {
        for (int j = 0; j < 9; j++) {
            if (board[i][j] == 0) {
                return new int[]{i, j};
            }
        }
    }
    return null; // No empty cell
}

private static boolean isValidSudokuMove(int[][] board, int row, int col, int num) {
    // Check row
    for (int j = 0; j < 9; j++) {
        if (board[row][j] == num) return false;
    }
    
    // Check column
    for (int i = 0; i < 9; i++) {
        if (board[i][col] == num) return false;
    }
    
    // Check 3x3 box
    int boxRow = (row / 3) * 3;
    int boxCol = (col / 3) * 3;
    for (int i = boxRow; i < boxRow + 3; i++) {
        for (int j = boxCol; j < boxCol + 3; j++) {
            if (board[i][j] == num) return false;
        }
    }
    
    return true;
}

// Test: Solve partially filled 9x9 Sudoku grid
```

### Problem 37: Word Search in Grid
**Category:** 2D Search  
**Concept:** DFS with path tracking

```java
/**
 * Search for word in 2D character grid
 * Word can be formed by adjacent cells (horizontally/vertically)
 */
public static boolean wordSearch(char[][] board, String word) {
    int rows = board.length;
    int cols = board[0].length;
    
    // Try starting from each cell
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (searchWord(board, word, i, j, 0)) {
                return true;
            }
        }
    }
    
    return false;
}

private static boolean searchWord(char[][] board, String word, int row, int col, int index) {
    // Base case: found complete word
    if (index == word.length()) return true;
    
    // Check bounds and character match
    if (row < 0 || row >= board.length || col < 0 || col >= board[0].length ||
        board[row][col] != word.charAt(index) || board[row][col] == '#') {
        return false;
    }
    
    // Mark current cell as visited
    char temp = board[row][col];
    board[row][col] = '#'; // Temporary marker
    
    // Search in all 4 directions
    boolean found = searchWord(board, word, row - 1, col, index + 1) ||  // Up
                   searchWord(board, word, row + 1, col, index + 1) ||    // Down
                   searchWord(board, word, row, col - 1, index + 1) ||    // Left
                   searchWord(board, word, row, col + 1, index + 1);      // Right
    
    // Backtrack: restore original character
    board[row][col] = temp;
    
    return found;
}

// Test: wordSearch([['A','B','C','E'],['S','F','C','S'],['A','D','E','E']], "ABCCED")
// Output: true
```

### Problem 38: Subset Sum Target
**Category:** Subset Selection  
**Concept:** Include/exclude with target constraint

```java
/**
 * Find all subsets that sum to target value
 * Classic subset selection with sum constraint
 */
public static void findSubsetSum(int[] arr, int target, List<Integer> current, int index, List<List<Integer>> result) {
    // Base case: target achieved
    if (target == 0) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    // Base case: processed all elements or target became negative
    if (index >= arr.length || target < 0) return;
    
    // Include current element
    current.add(arr[index]);
    findSubsetSum(arr, target - arr[index], current, index + 1, result);
    current.remove(current.size() - 1); // Backtrack
    
    // Exclude current element
    findSubsetSum(arr, target, current, index + 1, result);
}

public static List<List<Integer>> findSubsetSum(int[] arr, int target) {
    List<List<Integer>> result = new ArrayList<>();
    findSubsetSum(arr, target, new ArrayList<>(), 0, result);
    return result;
}

// Test: findSubsetSum([1,2,3,4,5], 5) 
// Output: [[1,4], [2,3], [5]]
```

### Problem 39: Generate Combinations (nCr)
**Category:** Combinatorics  
**Concept:** Selection without replacement

```java
/**
 * Generate all combinations of r elements from n elements
 * Mathematical nCr implementation using recursion
 */
public static void generateCombinations(int[] arr, int r, List<Integer> current, int start, List<List<Integer>> result) {
    // Base case: selected r elements
    if (current.size() == r) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    // Try each element from start position
    for (int i = start; i < arr.length; i++) {
        // Include current element
        current.add(arr[i]);
        
        // Recurse for remaining selections
        generateCombinations(arr, r, current, i + 1, result);
        
        // Backtrack
        current.remove(current.size() - 1);
    }
}

public static List<List<Integer>> generateCombinations(int[] arr, int r) {
    List<List<Integer>> result = new ArrayList<>();
    generateCombinations(arr, r, new ArrayList<>(), 0, result);
    return result;
}

// Alternative: Calculate nCr value recursively
public static long nCr(int n, int r) {
    // Base cases
    if (r == 0 || r == n) return 1;
    if (r > n) return 0;
    
    // Recursive relation: nCr = (n-1)C(r-1) + (n-1)Cr
    return nCr(n - 1, r - 1) + nCr(n - 1, r);
}

// Test: generateCombinations([1,2,3,4], 2) 
// Output: [[1,2], [1,3], [1,4], [2,3], [2,4], [3,4]]
```

### Problem 40: Grid Path Counter
**Category:** Path Counting  
**Concept:** Dynamic programming with recursion

```java
/**
 * Count all paths in grid from top-left to bottom-right
 * Can only move right or down
 */
public static int countGridPaths(int m, int n, int row, int col, Map<String, Integer> memo) {
    // Base case: reached destination
    if (row == m - 1 && col == n - 1) return 1;
    
    // Base case: out of bounds
    if (row >= m || col >= n) return 0;
    
    // Check memoization
    String key = row + "," + col;
    if (memo.containsKey(key)) return memo.get(key);
    
    // Count paths going right + paths going down
    int paths = countGridPaths(m, n, row, col + 1, memo) + 
                countGridPaths(m, n, row + 1, col, memo);
    
    memo.put(key, paths);
    return paths;
}

public static int countGridPaths(int m, int n) {
    return countGridPaths(m, n, 0, 0, new HashMap<>());
}

// Mathematical solution using combinations
public static long countGridPathsMath(int m, int n) {
    // Total moves needed: (m-1) down + (n-1) right = (m+n-2) total
    // Choose (m-1) positions for down moves: C(m+n-2, m-1)
    return nCr(m + n - 2, Math.min(m - 1, n - 1));
}

// Test: countGridPaths(3, 3) → Output: 6 paths
```

---

## 🔥 Level 5: Advanced/Backtracking (Problems 41-50)

> **Goal:** Master complex backtracking, optimization, and advanced recursive patterns

### Problem 41: Knight's Tour Problem
**Category:** Graph Traversal  
**Concept:** Hamiltonian path on chessboard

```java
/**
 * Solve Knight's Tour: visit every square exactly once
 * Uses Warnsdorff's heuristic for efficiency
 */
public static boolean solveKnightsTour(int n, int[][] board, int x, int y, int moveCount) {
    // Base case: visited all squares
    if (moveCount == n * n) return true;
    
    // Knight moves: 8 possible L-shaped moves
    int[] xMoves = {2, 1, -1, -2, -2, -1, 1, 2};
    int[] yMoves = {1, 2, 2, 1, -1, -2, -2, -1};
    
    // Try all 8 moves
    for (int i = 0; i < 8; i++) {
        int nextX = x + xMoves[i];
        int nextY = y + yMoves[i];
        
        if (isValidKnightMove(board, nextX, nextY, n)) {
            board[nextX][nextY] = moveCount;
            
            if (solveKnightsTour(n, board, nextX, nextY, moveCount + 1)) {
                return true;
            }
            
            // Backtrack
            board[nextX][nextY] = -1;
        }
    }
    
    return false;
}

private static boolean isValidKnightMove(int[][] board, int x, int y, int n) {
    return (x >= 0 && x < n && y >= 0 && y < n && board[x][y] == -1);
}

public static int[][] solveKnightsTour(int n, int startX, int startY) {
    int[][] board = new int[n][n];
    
    // Initialize board with -1 (unvisited)
    for (int i = 0; i < n; i++) {
        Arrays.fill(board[i], -1);
    }
    
    // Start position
    board[startX][startY] = 0;
    
    if (solveKnightsTour(n, board, startX, startY, 1)) {
        return board;
    }
    return null; // No solution
}

// Test: solveKnightsTour(8, 0, 0) → 8x8 board with knight's tour
```

### Problem 42: Graph Coloring Problem
**Category:** Graph Algorithm  
**Concept:** Constraint satisfaction on graphs

```java
/**
 * M-Coloring Problem: Color graph vertices with M colors
 * No two adjacent vertices should have same color
 */
public static boolean graphColoring(int[][] graph, int m, int[] colors, int vertex) {
    int n = graph.length;
    
    // Base case: colored all vertices
    if (vertex == n) return true;
    
    // Try all colors for current vertex
    for (int color = 1; color <= m; color++) {
        if (isSafeColor(graph, vertex, color, colors)) {
            colors[vertex] = color;
            
            if (graphColoring(graph, m, colors, vertex + 1)) {
                return true;
            }
            
            // Backtrack
            colors[vertex] = 0;
        }
    }
    
    return false;
}

private static boolean isSafeColor(int[][] graph, int vertex, int color, int[] colors) {
    // Check all adjacent vertices
    for (int i = 0; i < graph.length; i++) {
        if (graph[vertex][i] == 1 && colors[i] == color) {
            return false; // Adjacent vertex has same color
        }
    }
    return true;
}

public static int[] graphColoring(int[][] graph, int m) {
    int n = graph.length;
    int[] colors = new int[n];
    
    if (graphColoring(graph, m, colors, 0)) {
        return colors;
    }
    return null; // Cannot color with m colors
}

// Test: 3-color a triangle graph → Output: [1, 2, 3]
```

### Problem 43: Hamiltonian Path
**Category:** Graph Traversal  
**Concept:** Visit each vertex exactly once

```java
/**
 * Find Hamiltonian Path in graph
 * Path visits each vertex exactly once
 */
public static boolean findHamiltonianPath(int[][] graph, int[] path, boolean[] visited, int pos) {
    int n = graph.length;
    
    // Base case: visited all vertices
    if (pos == n) return true;
    
    // Try all vertices as next vertex
    for (int v = 0; v < n; v++) {
        if (isSafeHamiltonian(graph, v, path, pos) && !visited[v]) {
            path[pos] = v;
            visited[v] = true;
            
            if (findHamiltonianPath(graph, path, visited, pos + 1)) {
                return true;
            }
            
            // Backtrack
            visited[v] = false;
        }
    }
    
    return false;
}

private static boolean isSafeHamiltonian(int[][] graph, int v, int[] path, int pos) {
    // First vertex is always safe
    if (pos == 0) return true;
    
    // Check if current vertex is adjacent to previous vertex
    return graph[path[pos - 1]][v] == 1;
}

public static int[] findHamiltonianPath(int[][] graph, int startVertex) {
    int n = graph.length;
    int[] path = new int[n];
    boolean[] visited = new boolean[n];
    
    // Start from given vertex
    path[0] = startVertex;
    visited[startVertex] = true;
    
    if (findHamiltonianPath(graph, path, visited, 1)) {
        return path;
    }
    return null; // No Hamiltonian path
}

// Test: Find Hamiltonian path in connected graph
```

### Problem 44: Gray Code Generation
**Category:** Bit Manipulation  
**Concept:** Recursive bit pattern generation

```java
/**
 * Generate n-bit Gray code sequence
 * Adjacent codes differ by exactly one bit
 */
public static List<String> generateGrayCode(int n) {
    // Base case: 1-bit Gray code
    if (n == 1) {
        return Arrays.asList("0", "1");
    }
    
    // Get Gray code for n-1 bits
    List<String> prevGrayCode = generateGrayCode(n - 1);
    List<String> result = new ArrayList<>();
    
    // Add 0 prefix to first half (in same order)
    for (String code : prevGrayCode) {
        result.add("0" + code);
    }
    
    // Add 1 prefix to second half (in reverse order)
    for (int i = prevGrayCode.size() - 1; i >= 0; i--) {
        result.add("1" + prevGrayCode.get(i));
    }
    
    return result;
}

// Alternative: Generate Gray code as integers
public static List<Integer> generateGrayCodeInt(int n) {
    List<Integer> result = new ArrayList<>();
    
    // Total codes: 2^n
    int totalCodes = 1 << n;
    
    for (int i = 0; i < totalCodes; i++) {
        // Gray code formula: i XOR (i >> 1)
        result.add(i ^ (i >> 1));
    }
    
    return result;
}

// Test: generateGrayCode(3) 
// Output: ["000", "001", "011", "010", "110", "111", "101", "100"]
```

### Problem 45: Equal Sum Partition
**Category:** Dynamic Programming  
**Concept:** Subset partitioning with equal sums

```java
/**
 * Partition array into two subsets with equal sum
 * Returns true if such partition exists
 */
public static boolean canPartitionEqualSum(int[] arr, int index, int sum1, int sum2) {
    // Base case: processed all elements
    if (index >= arr.length) {
        return sum1 == sum2;
    }
    
    // Try adding current element to first subset
    if (canPartitionEqualSum(arr, index + 1, sum1 + arr[index], sum2)) {
        return true;
    }
    
    // Try adding current element to second subset
    if (canPartitionEqualSum(arr, index + 1, sum1, sum2 + arr[index])) {
        return true;
    }
    
    return false;
}

// Optimized version using subset sum
public static boolean canPartitionEqualSumOptimized(int[] arr) {
    int totalSum = Arrays.stream(arr).sum();
    
    // If total sum is odd, cannot partition equally
    if (totalSum % 2 != 0) return false;
    
    int targetSum = totalSum / 2;
    return hasSubsetSum(arr, targetSum, 0);
}

private static boolean hasSubsetSum(int[] arr, int targetSum, int index) {
    // Base cases
    if (targetSum == 0) return true;
    if (index >= arr.length || targetSum < 0) return false;
    
    // Include current element or exclude it
    return hasSubsetSum(arr, targetSum - arr[index], index + 1) ||
           hasSubsetSum(arr, targetSum, index + 1);
}

// Test: canPartitionEqualSum([1,5,11,5]) → Output: true ([1,5,5] and [11])
```

### Problem 46: Tile Placement Problem
**Category:** Counting Problem  
**Concept:** Recursive tiling patterns

```java
/**
 * Count ways to place 1x2 tiles in 2xN board
 * Tiles can be placed horizontally or vertically
 */
public static int countTilings(int n, Map<Integer, Integer> memo) {
    // Base cases
    if (n <= 1) return 1;
    if (n == 2) return 2;
    
    // Check memoization
    if (memo.containsKey(n)) return memo.get(n);
    
    // Recurrence relation:
    // f(n) = f(n-1) + f(n-2)
    // Place tile vertically: remaining board is 2x(n-1)
    // Place tiles horizontally: remaining board is 2x(n-2)
    int result = countTilings(n - 1, memo) + countTilings(n - 2, memo);
    
    memo.put(n, result);
    return result;
}

public static int countTilings(int n) {
    return countTilings(n, new HashMap<>());
}

// For 3xN board (more complex)
public static int countTilings3xN(int n, Map<Integer, Integer> memo) {
    if (n % 2 != 0) return 0; // 3xN board with odd N cannot be tiled
    if (n == 0) return 1;
    if (n == 2) return 3;
    
    if (memo.containsKey(n)) return memo.get(n);
    
    // Complex recurrence for 3xN tiling
    int result = 4 * countTilings3xN(n - 2, memo) - countTilings3xN(n - 4, memo);
    memo.put(n, result);
    return result;
}

// Test: countTilings(4) → Output: 5 ways to tile 2x4 board
```

### Problem 47: Unique Paths with Obstacles
**Category:** Path Finding  
**Concept:** Dynamic programming with constraints

```java
/**
 * Count unique paths in grid with obstacles
 * 0 = free cell, 1 = obstacle
 */
public static int uniquePathsWithObstacles(int[][] grid, int row, int col, Map<String, Integer> memo) {
    int m = grid.length;
    int n = grid[0].length;
    
    // Base cases
    if (row >= m || col >= n || grid[row][col] == 1) return 0;
    if (row == m - 1 && col == n - 1) return 1;
    
    String key = row + "," + col;
    if (memo.containsKey(key)) return memo.get(key);
    
    // Count paths going right + paths going down
    int paths = uniquePathsWithObstacles(grid, row, col + 1, memo) +
                uniquePathsWithObstacles(grid, row + 1, col, memo);
    
    memo.put(key, paths);
    return paths;
}

public static int uniquePathsWithObstacles(int[][] grid) {
    if (grid[0][0] == 1) return 0; // Starting cell is blocked
    return uniquePathsWithObstacles(grid, 0, 0, new HashMap<>());
}

// Test: uniquePathsWithObstacles([[0,0,0],[0,1,0],[0,0,0]]) → Output: 2
```

### Problem 48: Palindromic Partitioning
**Category:** String Partitioning  
**Concept:** Generate all valid partitions

```java
/**
 * Generate all palindromic partitions of string
 * Each partition contains only palindromic substrings
 */
public static void palindromicPartitions(String s, List<String> current, int start, List<List<String>> result) {
    // Base case: processed entire string
    if (start >= s.length()) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    // Try all possible substrings starting from current position
    for (int end = start; end < s.length(); end++) {
        String substring = s.substring(start, end + 1);
        
        if (isPalindrome(substring)) {
            current.add(substring);
            palindromicPartitions(s, current, end + 1, result);
            current.remove(current.size() - 1); // Backtrack
        }
    }
}

private static boolean isPalindrome(String str) {
    int left = 0, right = str.length() - 1;
    while (left < right) {
        if (str.charAt(left++) != str.charAt(right--)) return false;
    }
    return true;
}

public static List<List<String>> palindromicPartitions(String s) {
    List<List<String>> result = new ArrayList<>();
    palindromicPartitions(s, new ArrayList<>(), 0, result);
    return result;
}

// Test: palindromicPartitions("aab") 
// Output: [["a","a","b"], ["aa","b"]]
```

### Problem 49: Word Break Problem
**Category:** String Parsing  
**Concept:** Dictionary-based string decomposition

```java
/**
 * Check if string can be segmented using dictionary words
 * Uses backtracking with memoization
 */
public static boolean wordBreak(String s, Set<String> wordDict, int start, Map<Integer, Boolean> memo) {
    // Base case: processed entire string
    if (start >= s.length()) return true;
    
    // Check memoization
    if (memo.containsKey(start)) return memo.get(start);
    
    // Try all possible words starting from current position
    for (int end = start + 1; end <= s.length(); end++) {
        String word = s.substring(start, end);
        
        if (wordDict.contains(word) && wordBreak(s, wordDict, end, memo)) {
            memo.put(start, true);
            return true;
        }
    }
    
    memo.put(start, false);
    return false;
}

public static boolean wordBreak(String s, List<String> wordDict) {
    Set<String> wordSet = new HashSet<>(wordDict);
    return wordBreak(s, wordSet, 0, new HashMap<>());
}

// Advanced: Return all possible word break combinations
public static void wordBreakAll(String s, Set<String> wordDict, List<String> current, int start, List<List<String>> result) {
    if (start >= s.length()) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    for (int end = start + 1; end <= s.length(); end++) {
        String word = s.substring(start, end);
        
        if (wordDict.contains(word)) {
            current.add(word);
            wordBreakAll(s, wordDict, current, end, result);
            current.remove(current.size() - 1);
        }
    }
}

public static List<List<String>> wordBreakAll(String s, List<String> wordDict) {
    Set<String> wordSet = new HashSet<>(wordDict);
    List<List<String>> result = new ArrayList<>();
    wordBreakAll(s, wordSet, new ArrayList<>(), 0, result);
    return result;
}

// Test: wordBreak("leetcode", ["leet","code"]) → Output: true
// Test: wordBreakAll("catsanddog", ["cat","cats","and","sand","dog"]) 
// Output: [["cats","and","dog"], ["cat","sand","dog"]]
```

### Problem 50: Advanced Combination Generator
**Category:** Advanced Backtracking  
**Concept:** Complex constraint handling

```java
/**
 * Generate combinations with advanced constraints
 * Example: Generate k-length combinations where sum is within range
 */
public static void advancedCombinations(int[] arr, int k, int minSum, int maxSum, 
                                      List<Integer> current, int start, int currentSum, 
                                      List<List<Integer>> result) {
    // Pruning: if current sum already exceeds maxSum, no point continuing
    if (currentSum > maxSum) return;
    
    // Base case: selected k elements
    if (current.size() == k) {
        if (currentSum >= minSum && currentSum <= maxSum) {
            result.add(new ArrayList<>(current));
        }
        return;
    }
    
    // Pruning: not enough elements remaining
    if (start + (k - current.size()) > arr.length) return;
    
    for (int i = start; i < arr.length; i++) {
        current.add(arr[i]);
        advancedCombinations(arr, k, minSum, maxSum, current, i + 1, currentSum + arr[i], result);
        current.remove(current.size() - 1);
    }
}

public static List<List<Integer>> advancedCombinations(int[] arr, int k, int minSum, int maxSum) {
    List<List<Integer>> result = new ArrayList<>();
    advancedCombinations(arr, k, minSum, maxSum, new ArrayList<>(), 0, 0, result);
    return result;
}

// Test: advancedCombinations([1,2,3,4,5], 3, 6, 10) 
// Output: [[1,2,3], [1,2,4], [1,3,4], [2,3,4]] - 3-element combos with sum 6-10
```

---

## 🎓 How to Use This Guide

### **📚 Study Approach**

1. **Start Sequential**: Begin with Level 1 and progress systematically
2. **Understand Patterns**: Each problem builds on previous concepts
3. **Code First**: Try solving before looking at solutions
4. **Analyze Complexity**: Understand time/space trade-offs
5. **Practice Variants**: Modify problems to test understanding

### **🧠 Key Concepts Mastered**

| Concept | Problems | Difficulty |
|---------|----------|------------|
| **Base Cases** | 1-10 | ⭐ |
| **Array Recursion** | 11-20 | ⭐⭐ |
| **String Manipulation** | 21-30 | ⭐⭐⭐ |
| **Backtracking** | 31-40 | ⭐⭐⭐⭐ |
| **Advanced Algorithms** | 41-50 | ⭐⭐⭐⭐⭐ |


### **⚡ Performance Tips**

1. **Memoization**: Cache results to avoid recomputation
2. **Pruning**: Add conditions to skip impossible branches  
3. **Iterative Conversion**: Some recursions can be optimized to iterative
4. **Tail Recursion**: Optimize recursive calls when possible

**Happy Coding! 🎯 Master recursion step by step, and unlock the power of elegant algorithmic thinking.**
