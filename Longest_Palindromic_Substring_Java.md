
# 5.Longest Palindromic Substring in Java

This Java program aims to find the **longest palindromic substring** within a given string `s`. A palindrome reads the same backward as forward (e.g., "aba" or "racecar"). The program uses an approach that expands around potential centers of the palindrome.

## Overview of the Code

### Method Definition

```java
public String longestPalindrome(String s) {
```
- The `longestPalindrome` method is defined, taking a single argument `s`, which is the input string. 

### Initial Check

```java
if (s == null || s.length() < 2) return s;
```
- The method checks if the string `s` is `null` or has a length of less than 2. If either condition is true, it returns `s` immediately because:
  - An empty string or a single character is trivially a palindrome.

### Variable Initialization

```java
int start = 0, end = 0;
```
- Two integers `start` and `end` are initialized to 0. These will later store the indices of the longest palindromic substring found.

### For Loop to Iterate Over Each Character

```java
for (int i = 0; i < s.length(); i++) {
```
- A for loop begins, iterating over each character in the string `s`. The index `i` serves as the current center for potential palindromes.

## Iteration Process

### Example Input: `s = "babad"`

- The loop will iterate with `i` taking values from 0 to `s.length() - 1`, which is 4 in this case (length of "babad" is 5).

### 1st Iteration (`i = 0`)

```java
int len1 = expandAroundCenter(s, i, i);
```
- **Function Call**: `expandAroundCenter("babad", 0, 0)`
  - The function is called with `left` and `right` both set to `0`, meaning it starts checking from the center character `'b'`.

#### Inside `expandAroundCenter`

```java
while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
```
- **First Check**: `left = 0`, `right = 0`
  - The characters are equal (`'b' == 'b'`), so it enters the loop.

```java
    left--;  // left becomes -1
    right++; // right becomes 1
```
- Now `left` is `-1` and `right` is `1`.

- **Next Check**: `left = -1`, `right = 1`
  - This fails because `left` is no longer `>= 0`, so it exits the loop.

#### Return from `expandAroundCenter`

```java
return right - left - 1; // returns 1 - (-1) - 1 = 1
```
- Returns `1`, indicating the longest odd-length palindrome found centered at index 0 has a length of `1` (just the character `'b'`).

```java
int len2 = expandAroundCenter(s, i, i + 1);
```
- **Function Call**: `expandAroundCenter("babad", 0, 1)`
  - Now it checks for even-length palindromes centered between `'b'` and `'a'`.

#### Inside `expandAroundCenter` for Even-Length

```java
while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
```
- **First Check**: `left = 0`, `right = 1`
  - The characters are not equal (`'b' != 'a'`), so it skips the loop.

#### Return from `expandAroundCenter`

```java
return right - left - 1; // returns 1 - 0 - 1 = 0
```
- Returns `0`, meaning no even-length palindrome is found.

## Back to `longestPalindrome`

```java
int maxLen = Math.max(len1, len2);
```
- Calculates `maxLen = Math.max(1, 0)` which is `1`.

```java
if (maxLen > (end - start)) {
```
- Checks if `1 > (0 - 0)` → `1 > 0` (true).

```java
start = i - (maxLen - 1) / 2; // start = 0 - (1 - 1) / 2 = 0
end = i + maxLen / 2;        // end = 0 + 1 / 2 = 0
```
- Updates `start = 0`, `end = 0`. 

### 2nd Iteration (`i = 1`)

- The loop continues with `i = 1`.

```java
int len1 = expandAroundCenter(s, i, i);
```
- **Function Call**: `expandAroundCenter("babad", 1, 1)` (centered at `'a'`).

#### Inside `expandAroundCenter`

- Similar checks as before lead to finding the palindrome `"aba"`.

- After returning, `len1` gets a value of `3`.

```java
int len2 = expandAroundCenter(s, i, i + 1);
```
- **Function Call**: `expandAroundCenter("babad", 1, 2)`.
- This checks for even-length and returns `0` since characters are not equal.

## Back to `longestPalindrome`

```java
int maxLen = Math.max(len1, len2); // maxLen = Math.max(3, 0) = 3
```
- Update checks:
```java
if (maxLen > (end - start)) {
```
- `3 > (0 - 0)` (true).

Updating indices:
```java
start = i - (maxLen - 1) / 2; // start = 1 - (3 - 1) / 2 = 0
end = i + maxLen / 2;        // end = 1 + 3 / 2 = 3
```
- Now `start = 0`, `end = 3`, capturing substring `"baba"`.

### 3rd Iteration (`i = 2`)

- Continues similarly:
```java
len1 = expandAroundCenter(s, 2, 2); // finds "bab"
len2 = expandAroundCenter(s, 2, 3); // finds nothing
```
- `maxLen` is calculated but does not change `start` and `end` since it's not longer than current max.

### 4th Iteration (`i = 3`)

- Similar checks and expansions reveal `"aba"`.

### 5th Iteration (`i = 4`)

- Checks centered at last character (`'d'`), no longer palindromes.

## Final Return

```java
return s.substring(start, end + 1);
```
- Returns `s.substring(0, 4)`, yielding `"babab"` as the longest palindromic substring.

## Summary

- The program effectively checks each character in `s` as a potential center for both odd- and even-length palindromes.
- By calling `expandAroundCenter`, it explores all possible palindromes centered at each index.
- The program maintains the longest found indices through comparisons and finally returns the longest palindromic substring.

## Full Code

```java
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 2) return s;
        int start = 0, end = 0;

        for (int i = 0; i < s.length(); i++) {
            // Find the longest odd-length palindrome centered at i
            int len1 = expandAroundCenter(s, i, i);
            // Find the longest even-length palindrome centered at i and i + 1
            int len2 = expandAroundCenter(s, i, i + 1);

            int maxLen = Math.max(len1, len2);
            if (maxLen > (end - start)) {
                start = i - (maxLen - 1) / 2;
                end = i + maxLen / 2;
            }
        }
        
        return s.substring(start, end + 1);
    }

    private int expandAroundCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        return right - left - 1; // Return length of the palindrome
    }
}
```

### Conclusion

This algorithm effectively finds the longest palindromic substring by checking all possible centers and expanding outward, ensuring an optimal solution in terms of performance.
