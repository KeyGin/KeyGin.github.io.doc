---
title: ARTS Week4
date: 2019-12-16 23:00:52
index_img: /img/geektime/arts/continously.jpg
tags:
- geektime
- arts
categories:
- geektime
- arts
---
ARTS打卡第四周（2019.12.16 - 2019.12.22）
<!-- more -->
# Algorithms
## Problem 14. Longest Common Prefix

字符比较不可避免，快的核心在于减少遍历次数和if判断，这里有一个问题就是我没有检查两个比较字符串长度不同时数组越界。官网0ms执行速度的例子是采用了字符串删除的方式，减少了substr方法的时间开销。

```c++
using namespace std;

class Solution {
public:
    string longestCommonPrefix(vector<string> &strs) {
        string resultStr = "", str, firstStr;
        int minLength = 0;
        // if vector is empty, then return empty string
        if (strs.size() > 0) {
            firstStr = strs[0];
            minLength = firstStr.length();
        } else {
            return resultStr;
        }

        for(int i = 1; i < strs.size(); i++){
            str = strs[i];
            for(int j = 0; j < minLength; j++){
                if(str[j] != firstStr[j]){
                    minLength = j;
                }
            }
        }
        resultStr = firstStr.substr(0, minLength);
        return resultStr;
    }
};
```

## Problem 20. Valid Parentheses

我这里的问题是计算量过多，比较太多，官方0ms的例子是用swtich处理，栈里面直接存入匹配的符号，这样是可以直接比较的，不需要做计算。

```c++
class Solution {
public:
    bool isValid(string s) {
        // we know this parentheses in ascii code is:
        // [ - 91
        // ] - 93
        // ( - 40
        // ) - 41
        // { - 123
        // } - 125
        if(s.length() > 1){
            // create a stack to store waiting for parentheses character
            int arrIndex = 0;
            int arrLength = s.length() * .5f + 1;
            int arr[arrLength];
            for (int i = 0; i < s.length(); i++) {
                int c = s[i];
                // true only the character is '[', '(' and '{', which ascii code is 91, 40, 123
                if ((c - 41) & (c - 125) & (c - 93)) {
                    // stack full, which means it can't finish parentheses
                    if(arrIndex < arrLength){
                        return false;
                    }
                    arr[arrIndex++] = c;
                } else {
                    // the calculate result is 0, means this character need to check parentheses
                    if(arrIndex < 1){
                        return false;
                    }
                    int step = c - arr[--arrIndex];
                    if (step > 2 || step < 0) {
                        return false;
                    }
                }
            }
            // if arrIndex > 0, array arr not clear, it means parentheses invalid
            return !arrIndex;
        } else {
            return !s.length();
        }
    }
};
```
## Problem 26. Remove Duplicates from Sorted Array
这个题我做了2种实现，第一种是基于数组删除元素的，第二种是基于iterator删除元素的。看完官方给出的速度快的案例后，我发现我不用去做删除动作，有点无语。
```c++
using namespace std;

class Solution {
public:
    // 最终的改进版
    int removeDuplicates(vector<int> &nums) {
        if(nums.size() > 1){
            int index = 0;
            for (int i = 1; i < nums.size(); i++) {
                if (nums.at(index) < nums.at(i)) {
                    nums[++index] = nums[i];
                }
            }
            return index + 1;
        } else {
            return nums.size();
        }
    }

    // 基于iterator做的实现
    int removeDuplicates_iterator(vector<int> &nums) {
        int index = 0;
        vector<int>::iterator it = nums.begin();
        for (int i = 1; i < nums.size(); i++) {
            if (nums.at(index) < nums.at(i)) {
                nums[++index] = nums[i];
                it++;
            }
        }
        it++;
        nums.erase(it, nums.end());
        return nums.size();
    }
    
    // 基于数组做的实现
    int removeDuplicates_arr(vector<int>& nums) {
        int index = 0;
        vector<int>::iterator it = nums.begin();
        for (int i = 1; i < nums.size(); i++) {
            if (nums.at(index) < nums.at(i)) {
                nums[++index] = nums[i];
            }
        }
        index++;
        for (int i = nums.size(); i > index; i--) {
            nums.pop_back();
        }
        return nums.size();
    }
};
```
## Problem 27. Remove Element
这道题的难点是在第一个元素的处理，如果第一个元素和要移除的值一样，用数组处理时要注意元素的索引。
```c++
using namespace std;

class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        if (nums.size() < 1) {
            return 0;
        }
        int index = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (val != nums[i]) {
                nums[index++] = nums[i];
            }
        }
        return index;
    }

    int removeElement_iterator(vector<int> &nums, int val) {
        if (nums.size() < 1) {
            return 0;
        }
        vector<int>::iterator it = nums.begin();
        while (it < nums.end()) {
            if (*it == val) {
                nums.erase(it);
                it = nums.begin();
            } else {
                it++;
            }
        }
        return nums.size();
    }
};
```
## Problem 28. Implement strStr()
```c++
using namespace std;

class Solution {
public:
    int strStr(string haystack, string needle) {
        if (needle == "") {
            return 0;
        }
        if (needle.length() > haystack.length()) {
            return -1;
        }

        int i = 0, j = 0;
        while (i < haystack.length()) {
            if (haystack[i] == needle[j]) {
                i++;
                j++;
            } else {
                if (j < needle.length()) {
                    i = i - j + 1;
                    j = 0;
                } else {
                    return i - j;
                }
            }
        }
        if (j < needle.length()) {
            return -1;
        } else {
            return i - j;
        }
    }

    // 执行时间 120ms，内存消耗646.2MB
    int strStr_v2(string haystack, string needle) {
        if (needle == "") {
            return 0;
        }
        vector<int> v;
        int needleLength = needle.length();
        for (int i = 0; i < haystack.length(); i++) {
            if (!(haystack[i] - needle[0])) {
                v.push_back(i);
            }
        }
        for (int i = 0; i < v.size(); i++) {
            string str = haystack.substr(v[i], needleLength);
            if (str == needle) {
                return v[i];
            }
        }
        return -1;
    }

    // 执行时间 1660 ms，内存消耗 9.1MB
    int strStr_v1(string haystack, string needle) {
        if (needle == "") {
            return 0;
        }
        for (int i = 0; i < haystack.length(); i++) {
            if (!(haystack[i] - needle[0])) {
                int flag = 1;
                for (int j = i + 1; j < i + needle.length(); j++) {
                    if (haystack[j] - needle[flag]) {
                        flag = 0;
                        break;
                    } else {
                        flag++;
                    }
                }
                if (flag) {
                    return i;
                }
            }
        }
        return -1;
    }
};
```
## Problem 35. Search Insert Position
```c++
using namespace std;

class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        for(int i = 0; i < nums.size(); i++){
            if(target > nums[i]){
            } else {
                return i;
            }
        }
        return nums.size();
    }
};
```
## Problem 38. Count and Say
```c++
using namespace std;

class Solution {
public:
    string countAndSay(int n){
        string result = "1", bufferResult = "";
        if(n > 1){
            int index = 1;
            char num = '0', compareNum = result[0], counter = '1';
            while(n > 1){
                if(index < result.length()){
                    // not clearly read
                    char num = result[index];
                    if(num == compareNum){
                        counter++;
                    } else {
                        bufferResult.append(1, counter);
                        bufferResult.append(1, compareNum);
                        compareNum = num;
                        counter = '1';
                    }
                    index++;
                } else {
                    bufferResult.append(1, counter);
                    bufferResult.append(1, compareNum);
                    result = bufferResult;
                    bufferResult = "";
                    counter = '1';
                    index = 1;
                    compareNum = result[0];
                    n--;
                }
            }
        }
        return result;
    }
};
```
# Reviews
* [The JVM Architecture Explained](https://dzone.com/articles/jvm-architecture-explained)

    文章解释了JVM的架构，很全面。
# Tips
* 比起用索引循环比对确定字符串是否包含另外一个字符串，使用substr要占用更多的内存。
* 在嵌套循环遍历数组的时候，使用单循环双索引的方式遍历速度比嵌套循环遍历的速度要快很多。
# Share
* 这周在做一个系统的业务分析，发现使用脑图抓住系统的核心点，再由核心点去发散到各个细节，可以快速的理清现有的业务点，前后关系以及业务的流程，还能做到查漏补缺。
* 推荐一个好的免费英语单词记忆应用【不背单词】。