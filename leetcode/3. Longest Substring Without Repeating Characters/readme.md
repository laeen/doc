# [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/ )

## Description
Given a string s, find the length of the **longest substring** without repeating characters.



**Example 1**
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

**Example 2**
```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3**
```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```


**Constraints:**

- 0 <= s.length <= 5 * 10^4
- `s` consists of English letters, digits, symbols and spaces.


## Solution

核心思想是遍历穷举。O(n)

遍历字符串序列长度为n，当前遍历到的字符串位置记作 `i` ( 0 <= i < n), 当前的最长字符串长度设为 l , 则 `s[i]` 有两种情况：
        
        1. 在之前s[i] 已经出现过，并且在位置 s[k]。 那么最长的不重复字符串肯定是 max(l, i - k + 1) 
        2. 在之前s[i] 未出现过。那么最长的不重复字符串为 l + 1

如序列  `s = '123454'` 

`i = 4` 时， `s[i] = 5`, 最长为 `max(i-0+1, l) = 5` 其中 `l` 是 `1234` 的长度 4

`i = 5` 时， `s[i] = 4`, 最长为 `max(i-k+1, l) = 5` 其中 `l` 是 `12345` 的长度 4 , `k=3` 是第一个 `4` 出现的位置。



The core idea is to traverse exhaustion. O(n)

The length of the traversed string sequence is n, the current traversed string position is recorded as `i` ( 0 <= i < n), the current longest string length is set to l , then `s[i]` has two types condition:
        
         1. s[i] has appeared before, and is at position s[k]. Then the longest unique string must be max(l, i - k + 1)
         2. s[i] did not appear before. Then the longest unique string is l + 1

Such as the sequence `s = '123454'`

When `i = 4`, `s[i] = 5`, up to `max(i-0+1, l) = 5` where `l` is the length of `"1234"` equel 4

When `i = 5`, `s[i] = 4`, up to `max(i-k+1, l) = 5` where `l` is the length 4 of `"12345"` , `k=3` is where the first `4` occurs.

```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        
        if (s.size() <= 1) return s.size();
        
        int a[256];
        memset(a, -1, sizeof(a));
        int ret = 0;
        int start = -1;
        int i = 0;
        for(; i < s.size(); i++){
        
            if( a[s[i]] != -1 ){
                ret = max(ret, i-start-1);
                start = max(start, a[s[i]]);

               // cout<< s[i] << "\t" << ret << endl;
            }
            a[s[i]] = i;
 
        }
        
        return max(ret, i - start - 1);
    }
};
```








