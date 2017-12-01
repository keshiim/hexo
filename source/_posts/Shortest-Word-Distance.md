---
title: Leetcode-243 Shortest Word Distance
date: 2017-11-17 14:02:38
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Array
    - Easy
    - payed
---

### 题目描述
Given a list of words and two words *word1* and *word2*, return the shortest distance between these two words in the list.

### Example

```
Assume that words=["practice", "makes", "perfect", "coding", "makes"]
Given word1 = “coding”, word2 = “practice”, return 3.
Given word1 = "makes", word2 = "coding", return 1.
```
### Note
You may **assume** that *word1* does not equal to *word2*, and *word1* and *word2* are both in the list.

- - - 
### 分析
这道题，首先是遍历一遍数组，我们把出现*word1*和*word2*的给定单词所有出现的位置分别存入两个数组里，然后我们对这两个数组进行两两比较更新结果，代码如下：
### 解法1
#### C++

```c++
class Solution {
    int shortestDistance(vector<string>& words, string word1, string word2) {
        vector<int> idx1, idx2;
        int res = INT_MAX;
        for (int i = 0; i < words.size(); i++) {
            if (words[i] == word1) idx1.push_back(i);
            else if (words[i] == word2) idx2.push_back(i);
        }
        for (int i = 0; i < idx1.size(); ++i) {
            for (int j = 0; j < idx2.size(); ++j) {
                res = min(res, abs(idx1[i] - idx2[j]));
            }
        }
        return res;
    }
}
```
#### Swift

```swift
func shortestDistance(_ words: [String], word1: String, word2: String) -> Int {
    var idx1: [Int] = [Int](), idx2 = [Int]()
    var res = Int.max
    for i in 0..<words.count {
        if words[i] == word1 { idx1.append(i) }
        else if words[i] == word2 { idx2.append(i) }
    }
    for i in 0..<idx1.count {
        for j in 0..<idx2.count {
            res = min(res, abs(idx1[i] - idx2[j]))
        }
    }
    return res
}
print(shortestDistance(["practice", "makes", "perfect", "coding", "makes"], word1: "makes", word2: "coding"))
```
### 解法2
上面的那种方法并不高效，我们其实需要遍历一次数组就可以了，我们用两个变量`p1`, `p2`初始化为`-1`，然后我们遍历数组，遇到*单词1*，就将其位置存在`p1`里，若遇到*单词2*，就将其位置存在`p2`里，如果此时`p1, p2`都不为`-1`了，那么我们更新结果，参见代码如下：
#### C++

```c++
class Solution {
public:
    int shortestDistance(vector<string>& words, string word1, string word2) {
        int p1 = -1, p2 = -1, res = INT_MAX;
        for (int i = 0; i < words.size(); ++i) {
            if (words[i] == word1) p1 = i;
            else if (words[i] == word2) p2 = i;
            if (p1 != -1 && p2 != -1) res = min(res, abs(p1 - p2));
        }
        return res;
    }
};
```
#### Swift

```swift
func shortestDistance1(_ words: [String], word1: String, word2: String) -> Int {
    var idx1: Int = -1, idx2 = -1
    var res = Int.max
    for i in 0..<words.count {
        if words[i] == word1 { idx1 = i}
        else if words[i] == word2 { idx2 = i}
        if idx1 != -1 && idx2 != -1 {
            res = min(res, abs(idx1 - idx2))
        }
    }
    return res
}
print(shortestDistance1(["practice", "makes", "perfect", "coding", "makes"], word1: "coding", word2: "practice"))
// Print "3"
```
### 解法3
下面这种方法只用一个辅助变量`idx`，初始化为`-1`，然后遍历数组，如果遇到等于两个单词中的任意一个的单词，我们在看`idx`是否为`-1`，若不为`-1`，且指向的单词和当前遍历到的单词不同，我们更新结果，参见代码如下：
#### C++

```c++
class Solution {
public:
    int shortestDistance(vector<string>& words, string word1, string word2) {
        int idx = -1, res = INT_MAX;
        for (int i = 0; i < words.size(); ++i) {
            if (words[i] == word1 || words[i] == word2) {
                if (idx != -1 && words[idx] != words[i]) {
                    res = min(res, i - idx);
                }
                idx = i;
            }
        }
        return res;
    }
};
```
#### Swift

```swift
func shortestDistance2(_ words: [String], word1: String, word2: String) -> Int {
    var idx = -1
    var res = Int.max
    for i in 0..<words.count {
        if words[i] == word1 || words[i] == word2 {
            if idx != -1 && words[i] != words[idx] {
                res = min(res, abs(i - idx))
            }
            idx = i
        }
    }
    return res
}
print(shortestDistance2(["practice", "makes", "perfect", "coding", "makes"], word1: "coding", word2: "practice"))
// Prints "3"
```
### 参考资料：

> https://leetcode.com/discuss/50234/ac-java-clean-solution
https://leetcode.com/discuss/61820/java-only-need-to-keep-one-index

