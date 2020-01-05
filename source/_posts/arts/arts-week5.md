---
title: ARTS Week 5
date: 2019-12-31 23:35:10
index_img: /img/geektime/arts/continously.jpg
tags:
- geektime
- arts
categories:
- geektime
- arts
---

ARTS 打卡第五周（2019.12.30 - 2020.01.05）
<!-- more -->
# Algorithms
## Problem 4. Median of Two Sorted Arrays
```c++
#include <vector>
using namespace std;

class Solution
{
public:
    // cost memory
    double findMedianSortedArrays(vector<int> &nums1, vector<int> &nums2)
    {
        int lenNums1 = nums1.size(), lenNums2 = nums2.size(), totalLength = lenNums1 + lenNums2, j = 0, k = 0, medianPosition = totalLength / 2;
        vector<int> temp;
        if (lenNums1 < 1)
        {
            temp = vector<int>(nums2);
        }
        else if (lenNums2 < 1)
        {
            temp = vector<int>(nums1);
        }
        else
        {
            // using guard to simplify code, this code can simplify one guard.
            // int maxValue = max(nums1.at(lenNums1 - 1), nums2.at(lenNums2 - 1)) + 1;
            // nums1.push_back(maxValue);
            // nums2.push_back(maxValue);
            // for (int i = 0; i < (medianPosition + 1); i++)
            // {
            //     if (nums1.at(j) < nums2.at(k))
            //     {
            //         temp.push_back(nums1.at(j++));
            //     }
            //     else
            //     {
            //         temp.push_back(nums2.at(k++));
            //     }
            // }

            while(j < lenNums1 && k < lenNums2){
                if(nums1.at(j) < nums2.at(k)){
                    temp.push_back(nums1.at(j++));
                } else {
                    temp.push_back(nums2.at(k++));
                }
            }
            while(j < lenNums1){
                temp.push_back(nums1.at(j++));
            }
            while(k < lenNums2){
                temp.push_back(nums2.at(k++));
            }
        }
        if (totalLength & 1)
        {
            return temp.at(medianPosition);
        }
        else
        {
            return (temp.at(medianPosition) + temp.at(medianPosition - 1)) / 2.;
        }
    }

    // cost time
    double findMedianSortedArrays_v1(vector<int> &nums1, vector<int> &nums2)
    {
        int lenNum1 = nums1.size(), lenNum2 = nums2.size();
        int totalLength = lenNum1 + lenNum2;
        int middlePosition = totalLength / 2 + 1;
        int i = 0, j = 0, previousNum = 0, currentNum = 0;

        for (int k = 0; k < middlePosition; k++)
        {
            previousNum = currentNum;
            if (i < lenNum1 && j < lenNum2)
            {
                if (nums1.at(i) > nums2.at(j))
                {
                    currentNum = nums2.at(j);
                    j++;
                }
                else
                {
                    currentNum = nums1.at(i);
                    i++;
                }
            }
            else if (i < lenNum1)
            {
                currentNum = nums1.at(i);
                i++;
            }
            else
            {
                currentNum = nums2.at(j);
                j++;
            }
        }

        if (totalLength % 2)
        {
            return currentNum;
        }
        else
        {
            return ((currentNum + previousNum) / 2.f);
        }
    }
};
```
## Problem 5. Longest Palindromic Substring
```c++
using namespace std;

class Solution {
public:
    string longestPalindrome(string s) {
        // special case
        // aabbbbaa or aba, the first characters palindromic is even number, the second characters palindromic is odd number

        // if string s's length smaller than 2, means one character or empty string, so return itself.
        if (s.length() < 2) {
            return s;
        }

        int start = 0, currentLen = 0, len, lenEven, lenOdd;
        for (int i = 0; i < s.length() - 1; i++) {
            lenOdd = recursivePalindrome(s, i, i);
            lenEven = recursivePalindrome(s, i, (i + 1));
            len = max(lenOdd, lenEven);
            if (len > currentLen) {
                // when odd number is bigger, len is odd number, it means 1 / 2 = 0 equals to (1 - 1) / 2 = 0
                // when even number is bigger, len is even number, because even number length start on 2,
                // so we should subtract 1 to calc the start position, or start will be wrong position, even be -1 in string aa
                start = i - (len - 1) / 2;
                currentLen = len;
            }
        }
        return s.substr(start, currentLen);
    }

    int recursivePalindrome(string s, int start, int end) {
        while (start >= 0 && end < s.length() && s.at(start) == s.at(end)) {
            start--;
            end++;
        }
        return end - start - 1;
    }
};
```

# Reviews
* Advanced API Security, 2nd Edition. Chapter 1. APIs Rule!

    这个章节点评了没有管理的API都有一些共用的缺陷：

    一、没有合理的跟踪业务所有者或者长时间跟踪业务所有者变化；

    二、版本管理不合理，新版本发布会导致旧版本功能不能用；

    三、没有权限管控；

    四、没有做资源限制，同一个地址多次调用重复申请资源，会导致资源快速占满；

    五、没有跟踪日志；

    六、没有对使用模式做合理的切割；

    七、没有可发现的能力，调用一个接口的时候不知道还可不可以做其他操作；

    八、没有合适的文档；

    九、因为以上八点，导致没有业务模型。
    
    这几点中我对第二点、第五点和第八点有挺深的感触，特别是和第三方系统做对接时，文档的老旧导致在对接时特别难受，大家都很忙，一个字段要沟通半个多小时，还要调试才能查出来有是另外一个字段要传什么，类似这样的问题出现频率特别高，从而导致工作效率低下。

# Tips
* 在做有序数据合并时，可以利用哨兵机制来简化代码逻辑复杂度
* C++使用iterator遍历的vector时候，如果vector数据发生变化，需要重新获取iterator才能正常遍历数据
* 在做循环遍历的时候，尽量使用固定值作为条件，避免使用size()，length()之类的方法获取长度，以防粗心更改了集合数据后，发生一些意外情况，如果需求就是要动态的，那就没办法了

# Share
* 最近公司想用阿里云EDAS来做产品管理，我去学习了下，发现EDAS除了PAAS外，还包括了一些分布式服务之间的调度管理，比起PAAS又更进一布，且EDAS兼容以前的整体式服务架构的管理。