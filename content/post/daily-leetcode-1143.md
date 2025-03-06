---
title: Daily Leetcode 1143 Longest Common Subsequence
date: 2025-03-06T14:43:36+08:00
Description:
Tags: ['String', 'Dynamic-Programming']
Categories:
toc: true
DisableComments: false
---
Given two strings `text1` and `text2`, return _the length of their longest **common subsequence**._ If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

## Solution
### Solution 1: Recursion

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        def dfs(i :int, j: int) -> int:
            if len(text1) == i or len(text2) == j:
                return 0

            if text1[i] == text2[j]:
                return 1 + dfs(i+1, j+1)
            else:
                ans1 = dfs(i+1, j)
                ans2 = dfs(i, j+1)

                return ans1 if ans1 > ans2 else ans2
        return dfs(0, 0)
```

Time Complexity: $O(2^{(n+m)})$

It iterate every possible combination in the two texts. It should have binary call stack tree of at max `n + m` levels. At every level, two stack calls is created.

Space Complexity: $O(2^{(n+m)})$

The complexity is stated at $O(n + m)$. However, I do not agree because we excluded the call stack memory. Where the complexity of space should be same as time.

### Solution 2: Dynamic Programming (Bottom Up)
What is bottom up? From the later part of the inputs to the front of it. However, I could only imagine the `memo` approach in LC-1092 with behind the texts to front of the text. In the solution, an 2D `dp` array is used to store the computation. Be reminded that, because we know the end result of the string must be 0.

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        dp = [[0] * (len(text2)+1) for _ in range(len(text1)+1)]

        for i in range(len(text1)-1, -1, -1):
            for j in range(len(text2)-1, -1, -1):
                if text1[i] == text2[j]:
                    dp[i][j] = 1 + dp[i+1][j+1]
                else:
                    ans1 = dp[i+1][j]
                    ans2 = dp[i][j+1]

                    dp[i][j] = max(ans1, ans2)

        return dp[0][0]
```

Time Complexity: $O(m * n)$

Space Complexity: $O(m * n)$

### Solution 3: Dynamic Programming (Space Optimized)

It is because we only use two rows on the above `dp` 2D array technically. We can use only two array rather than a 2D array for space optimization.

```python
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        n = len(text1)
        m = len(text2)

        prev = [0] * (m+1)
        curr = [0] * (m+1)

        for i in range(n-1, -1, -1):
            for j in range(m-1, -1, -1):
                if text1[i] == text2[j]:
                    curr[j] = 1 + prev[j+1]
                else:
                    ans1 = curr[j+1]
                    ans2 = prev[j]

                    curr[j] = max(ans1, ans2)
            prev, curr = curr, prev
        return prev[0]
```

Time Complexity: $O(n * m)$

Space Complexity: $O(m)$

It is because there are only two arrays of size `m`.
