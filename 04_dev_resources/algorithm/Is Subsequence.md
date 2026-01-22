[392. Is Subsequence](https://leetcode.com/problems/is-subsequence/)

Easy

Topics

![premium lock icon](https://leetcode.com/_next/static/images/lock-a6627e2c7fa0ce8bc117c109fb4e567d.svg)Companies

Given two strings `s` and `t`, return `true` _if_ `s` _is a **subsequence** of_ `t`_, or_ `false` _otherwise_.

A **subsequence** of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

**Example 1:**

**Input:** s = "abc", t = "ahbgdc"
**Output:** true

**Example 2:**

**Input:** s = "axc", t = "ahbgdc"
**Output:** false

**Constraints:**

- `0 <= s.length <= 100`
- `0 <= t.length <= 104`
- `s` and `t` consist only of lowercase English letters.

**Follow up:** Suppose there are lots of incoming `s`, say `s1, s2, ..., sk` where `k >= 109`, and you want to check one by one to see if `t` has its subsequence. In this scenario, how would you change your code?


## 접근
- 2개의 시퀀스가 입력으로 주어졌다.
- 하나의 시퀀스가 다른 시퀀스의 서브시퀀스다.
	- 즉, 하나의 시퀀스의 요소가 다른 시퀀스의 요소에 속해있는지 알아야하므로, 비교(확인? 표현을 잘 모르겠다.)를 해야한다.
			- bruth force방법을 사용하면 n^2
			- (옵시디언에서 위의 표현을 잘 모르겠다.) two pointer를 사용하면 n의 시간복잡도로 끝낼 수 있을 듯하다. 
#leetcode #twopointer
