---
title: leetCode
date: 2023-04-11 16:18:02
tags: ['算法']
categories: ['学习']
top_img: /img/Top_img.jpg
cover: /img/Cover_LeetCode.png
highlight_shrink: false
---

# 简单难度

## 回文数

[回文数 - 力扣](https://leetcode.cn/problems/palindrome-number/description/)

给你一个整数 `x` ，如果 `x` 是一个回文整数，返回 `true` ；否则，返回 `false` 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

> **示例 1：**
> **输入：** x = 121
> **输出：** true

### 我的思路

 求出数据是几位数，利用整除求出每一位的数字，把首位放到末尾...最后比较是否相等，表现十分差

#### 代码

```cpp
class Solution {
public:
    bool isPalindrome(int x) {
    long tempx = x;
    int newx = 0;
    if (x < 0)
    {
        return false;
    }

    int Place = 0;
    double temp0 = 1;
    int count = 0;

    do
    {
        Place =(int)(x / (temp0 * 10));
        temp0 *= 10;
        count += 1;
    } while (Place != 0);


    long j = 1;

    for (int i = count-1; i >= 0; i--)
    {
        long temp = tempx / pow(10,i);
        tempx -= temp * pow(10, i);
        newx += temp * j;
        j *= 10;
        std::cout << ("%d", newx) << "\n";
    }
    if (newx == x)
    {
 return true;
    }
    else
    {
  return false;
    }
    }
};
```

### 其他思路

#### 反转一半数字

映入脑海的第一个想法是将数字转换为字符串，并检查字符串是否为回文。但是，这需要额外的非常量空间来创建问题描述中所不允许的字符串。

第二个想法是将数字本身反转，然后将反转后的数字与原始数字进行比较，如果它们是相同的，那么这个数字就是回文。 但是，如果反转后的数字大于`int.MAX`，我们将遇到整数溢出问题。

按照第二个想法，为了避免数字反转可能导致的溢出问题，为什么不考虑只反转` int`数字的一半？毕竟，如果该数字是回文，其后半部分反转后应该与原始数字的前半部分相同。

例如，输入 1221，我们可以将数字 “1221” 的后半部分从 “21” 反转为 “12”，并将其与前半部分 “12” 进行比较，因为二者相同，我们得知数字 1221 是回文。

##### 算法

首先，我们应该处理一些临界情况。所有负数都不可能是回文，例如：-123 不是回文，因为 - 不等于 3。所以我们可以对所有负数返回 false。除了 0 以外，所有个位是 0 的数字不可能是回文，因为最高位不等于 0。所以我们可以对所有大于 0 且个位是 0 的数字返回 false。

现在，让我们来考虑如何反转后半部分的数字。

对于数字 1221，如果执行 1221 % 10，我们将得到最后一位数字 1，要得到倒数第二位数字，我们可以先通过除以 10 把最后一位数字从 1221 中移除，1221 / 10 = 122，再求出上一步结果除以 10 的余数，122 % 10 = 2，就可以得到倒数第二位数字。如果我们把最后一位数字乘以 10，再加上倒数第二位数字，1 * 10 + 2 = 12，就得到了我们想要的反转后的数字。如果继续这个过程，我们将得到更多位数的反转数字。

现在的问题是，我们如何知道反转数字的位数已经达到原始数字位数的一半？

由于整个过程我们不断将原始数字除以 10，然后给反转后的数字乘上 10，所以，当原始数字小于或等于反转后的数字时，就意味着我们已经处理了一半位数的数字了。

##### 代码

```cpp
class Solution {
public:
    bool isPalindrome(int x) {
        // 特殊情况：
        // 如上所述，当 x < 0 时，x 不是回文数。
        // 同样地，如果数的最后一位是 0，为了使该数字为回文，
        // 则其第一位数字也应该是 0
        // 只有 0 满足这一属性
        if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }

        int revertedNumber = 0;
        while (x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }

        // 当数字长度为奇数时，我们可以通过 revertedNumber/10 去除处于中位的数字。
        // 例如，当输入为 12321 时，在 while 循环的末尾我们可以得到 x = 12，revertedNumber = 123，
        // 由于处于中位的数字不影响回文（它总是与自己相等），所以我们可以简单地将其去除。
        return x == revertedNumber || x == revertedNumber / 10;
    }
};
```

## 罗马数字转整

[罗马数字转整数 - 力扣](https://leetcode.cn/problems/roman-to-integer/description/)

罗马数字包含以下七种字符: `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
**字符** **数值**
I 1
V 5
X 10
L 50
C 100
D 500
M 1000
```

例如， 罗马数字 `2` 写做 `II` ，即为两个并列的 1 。`12` 写做 `XII` ，即为 `X` + `II` 。 `27` 写做  `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

**给定一个罗马数字，将其转换成整数。**

### 我的思路

比较字符与其后面的那一个字符的大小，如果比后面的大则加，比后面的小则减

#### 代码

```cpp
 private:
    std::unordered_map<char, int> symbolValues = {
        {'I', 1},
        {'V', 5},
        {'X', 10},
        {'L',50},
        {'C', 100},
        {'D', 500},
        {'M', 1000}};

public:
    int romanToInt(string s) {
        int ans = 0;
        int n = s.length();
        for (int i = 0; i < n ; i++)
        {
            if (i == n - 1)
            {
                ans += symbolValues[s[i]];
                return ans;
            }
            if (symbolValues[s[i]] >= symbolValues[s[i + 1]])
            {
                ans += symbolValues[s[i]];
            }
            else
            {
                ans -= symbolValues[s[i]];
            }

        }
        return ans;
    }
```

### 其他思路

[力扣官方解题](https://leetcode.cn/problems/roman-to-integer/solutions/774992/luo-ma-shu-zi-zhuan-zheng-shu-by-leetcod-w55p/)

## 最长公共前缀

[14. 最长公共前缀 - 力扣（Leetcode）](https://leetcode.cn/problems/longest-common-prefix/description/)

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 `""`。

**示例 1：**

> **输入:** strs = ["flower","flow","flight"]
> 
> **输出:** "fl"

### 我的思路

这题非常简单，遍历一个字符串的所有字符的过程中判断其他的字符串是否也有这个字符或者是否达到了其他字符串的最大长度，如果所有其他字符都有这个字符则把此字符加到结果中，否则直接返回结果

#### 代码

```cpp
std::string LeetCode::longestCommonPrefix(std::vector<std::string> &strs)
{
    std::string result = "";
    for (int i = 0; i < strs[0].length(); i++)
    {
        char common = strs[0][i];
        for (int j = 1; j < strs.size(); j++)
        {
            if(strs[j][i]!=common||i>strs[j].length())
                return result;
        }
        result.push_back(common);
    }
    return result;
}
```

### 其他思路

[力扣官方解题](https://leetcode.cn/problems/longest-common-prefix/solutions/288575/zui-chang-gong-gong-qian-zhui-by-leetcode-solution/)
