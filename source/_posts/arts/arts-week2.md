---
title: ARTS Week 2
date: 2019-12-07 01:52:06
index_img: /img/geektime/arts/continously.jpg
tags:
- geektime
- arts
categories: 
- geektime
- arts
---
ARTS 打卡第二周
<!-- more -->
# ARTS打卡第二周(2019.12.02 - 2019.12.08)
## Algrithms
### A. LeetCode 8. String to Integer (atoi)

```c++
using namespace std;

class Solution {
public:
    int myAtoi(string str) {
        int first_is_digital = -1;
        int sign = 1;
        int temp = 0;
        int digital;
        int low_max = INT32_MAX / 10;
        int low_min = INT32_MIN / 10;
        char ch;
        for (int i = 0; i < str.size(); i++) {
            ch = str[i];
            if (ch > 57 || ch < 48) {
                if (first_is_digital < 0) {
                    if (ch == 32) {
                        continue;
                    } else if (ch == 45) {
                        sign = -1;
                        first_is_digital = 0;
                    } else if (ch == 43) {
                        first_is_digital = 0;
                    } else {
                        return temp;
                    }
                } else {
                    return temp;
                }
                digital = 0;
            } else {
                first_is_digital = 1;
                digital = ch - 48;
                digital = digital * sign;
            }
            if (temp > low_max || (temp == low_max && digital > 7)) {
                return INT32_MAX;
            }
            if (temp < low_min || (temp == low_min && digital < -8)) {
                return INT32_MIN;
            }
            temp = temp * 10 + digital;
        }
        return temp;
    }
};
```

## Reviews
* [What Developers Need to Know About Java Security](https://dzone.com/articles/what-developers-need-to-know-about-java-security)
重复造轮子，也许你写的代码在当时是很好的，很安全的，随着时间的流逝，你不能保证你的代码不会随着业务变更而被其他人修改，从而变的不安全，最好的方式是用一些大家常用的库，团队内部做安全审查就好了。

## Tips
* 最近公司想要抓取微博热搜上的数据，以前没有接触过爬虫，这周用python试过，后面发现和java集成不太好弄，于是换成了用java去抓取网页，经过这样一折腾，我发现其实爬虫就是抓取网页数据，分析网页数据的一个过程，和用什么语言去实现并没有太大关系

## Share

* [Understanding Linux Runlevels, the Right Way](https://dzone.com/articles/understanding-linux-runlevels-the-right-way)
在这篇文章学到了Linux运行等级runlevels，同时文章也延展到了现在常用的systemd。
