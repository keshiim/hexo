---
title: Leetcode-717 1-bit and 2-bit Characters
date: 2017-11-22 15:11:12
categories:
    - LeetCode
tags:
    - LeetCode
    - algorithm
    - 算法
    - Array
    - Easy
---
### 题目描述
We have two special characters. The first character can be represented by one bit `0`. The second character can be represented by two bits(`10`or`11`).

Now given a string represented by several bits. Return whether the last character must be a one-bit character or not. The given string will always end with a zero.
### Example1
    Input: 
    bits = [1, 0, 0]
    Output: True
    Explanation: 
    The only way to decode it is two-bit character and one-bit character. So the last character is one-bit character.
### Example2
    Input: 
    bits = [1, 1, 1, 0]
    Output: False
    Explanation: 
    The only way to decode it is two-bit character and two-bit character. So the last character is NOT one-bit character.

### Note
* $1\leq len(bits)\leq 1000$.
* `bits[i]` is always `0` or `1`

### 解法一
**分析：**这道题说有两种特殊的字符，一种是两位字符，只能是二进制的`11`和`10`，另一种是单个位字符，只能是二进制的`0`。现在给我们了一个包含0和1的数组，问我们是否能够将其正确的分割，使得最后一个字符是个单个位字符。

这道题使用贪心散发来做，因为两种位字符互不干扰，只要我们遍历到了数字1，那么气必定是两位字符，所以后面以为也得跟着，而遍历到了数字0，那么就必定是单个位字符。

所以，我们可以用一个变量`i`来记录当前遍历到的位置，如果遇到了0，那么`i`自增1，如果遇到了1，那么`i`自增2，我们循环的条件是`i < n-1`，即留出最后一位，所以当循环退出后，当`i`正好停留在`n - 1`上，说明最后以为是单独分割开的，因为题目中限定了最后一位一定是0，就没有必要判断了，代码如下：

#### C++
```c++
class Solution {
public:
    bool isOneBitCharacter(vector<int>& bits) {
        int n = bits.size(), i = 0;
        while (i < n - 1) {
            if (bits[i] == 0) ++i;
            else i+= 2;
        }
        return i == n - 1;
    }
};
```
#### Swift

```swift
class Solution {
    func isOneBitCharacter(_ bits: [Int]) -> Bool {
        let n = bits.count
        var i = 0
        while i < n - 1 {
            if bits[i] == 0 { 
                i+= 1
            } else {
                i += 2
            }
        }
        return  i == n - 1
    }
}
```
### 解法二
下面这种解法写的更加简洁了，直接用一行代替了`if..else..`语句，相当巧妙，当bits[i]为0时，i还是相当于自增了1，当bits[i]为1时，i相当于自增了2，最后还是在循环跳出后检测i是否为n-1，参见代码如下：
#### C++

```c++
class Solution {
public:
    bool isOneBitCharacter(vector<int>& bits) {
        int n = bits.size(), i = 0;
        while (i < n - 1) {
            i += bits[i] + 1;
        }
        return i == n - 1;
    }
};
```
#### Swift
```swift
class Solution {
    func isOneBitCharacter(_ bits: [Int]) -> Bool {
        let n = bits.count
        var i = 0
        while i < n - 1 {
            i += bits[i] + 1
        }
        return  i == n - 1
    }
}
```
### 解法三
下面我们来看递归解法，用的是回溯的思想，首先判断如果`bits`为空了，直接返回`false`，因为题目初始给的`bits`是非空的，在调用递归函数中为空了说明最后一位跟倒数第二位组成了个两位字符，所以不合题意返回`false`。再判断如果`bits`大小为1了，那么返回这个数字是否为0，其实直接返回`true`也行，因为题目中说了最后一个数字一定是0。然后我们新建一个数组`t`，如果`bits`的首元素为0，则我们的`t`赋值为去掉首元素的`bits`数组；如果`bits`的首元素是1，则我们的t服之为去掉前两个元素的`bits`数组，然后返回调用递归函数的结果即可，参见代码如下：
#### C++

```c++
class Solution {
public:
    bool isOneBitCharactor(vector<int>& bits) {
        if (bits.empty()) return false;
        if (bits.size() == 1) return bits[0] == 0; 
        vector<int> t;
        if (bits[0] == 0) {
            t = vector<int>(bits.begin() + 1, bits.end());
        } else if (bits[0] == 1) {
            t = vector<int>(bits.begin() + 2, bits.end())
        }
        return isOneBitCharactor(t);
    }
}
```

#### Swift

```swift
func isOneBitCharacter1(_ bits: [Int]) -> Bool {
    if bits.isEmpty { return false }
    if bits.count == 1 { return bits[0] == 0 }
    var t = Array<Int>()
    if bits[0] == 0 {
        t = Array(bits[1..<bits.endIndex])
    } else if bits[0] == 1 {
        t = Array(bits[2..<bits.endIndex])
    }
    return isOneBitCharacter1(t)
}
print(isOneBitCharacter1([1, 1, 0, 0]))
```
### 解法四
下面这种解法也是用的递归，递归函数用的不是原函数，这样可以只用位置变量`idx`来遍历，**而不用新建数组`t`**，初始时`idx`传入0，在递归函数中，

如果`idx`为`n`了，相当于上面解法中的`bits`数组为空了情况，返回`false`；

如果`idx`为`n-1`，返回 bits[idx] == 0；

如果`bits[idx]`为0，则返回调用递归函数的结果，此时`idx`加上1；

如果`bits[idx]`为1，则返回调用递归函数的结果，此时`idx`加上2，参见代码如下：
#### C++

```c++
class Solution {
public:
    bool isOneBitCharacter(vector<int>& bits) {
        return helper(bits, 0);
    }
    bool helper(vector<int>& bits, int idx) {
        int n = bits.size();
        if (idx == n) return false;
        if (idx == n - 1) return bits[idx] == 0;
        if (bits[idx] == 0) return helper(bits, idx + 1);
        return helper(bits, idx + 2);
    }
};
```
#### Swift

```swift
func isOneBitCharacter2(_ bits: [Int]) -> Bool {
    return helper(bits, idx: 0)
}
func helper(_ bits: [Int], idx: Int) -> Bool {
    let n = bits.count
    if idx == n { return false }
    if idx == n - 1 { return bits[idx] == 0 }
    if (bits[idx] == 0) { return helper(bits, idx: idx + 1) }
    return helper(bits, idx: idx + 2)
}
print(isOneBitCharacter2([1,1,0,0]))
// Print true
```

