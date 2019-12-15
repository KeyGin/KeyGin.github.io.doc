---
title: ARTS Week 3
date: 2019-12-09 14:09:52
index_img: /img/geektime/arts/continously.jpg
tags:
- geektime
- arts
categories:
- geektime
- arts
---
ARTS 打卡第三周
<!-- more -->
# ARTS 打卡第三周(2019.12.09 - 2019.12.15)
## Algrithms
### A 9. Palindrome Number
```c++
class Solution {
public:
    bool isPalindrome(int x) {
        if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }
        int j = 0;
        while (x > j) {
            j = j * 10 + x % 10;
            x = x / 10;
        }
        return j == x || x == (j / 10);
    }
};
```
### B 13. Roman to Integer
```c++
class Solution {
public:
    int romanToInt(string s) {
        int sizes = s.size();
        char c;
        int j, prev = 0, value = 0;
        for (int i = 0; i < sizes; i++) {
            c = s.at(i);
            switch (c) {
                case 'I':
                    j = 1;
                    break;
                case 'V':
                    j = 5;
                    break;
                case 'X':
                    j = 10;
                    break;
                case 'L':
                    j = 50;
                    break;
                case 'C':
                    j = 100;
                    break;
                case 'D':
                    j = 500;
                    break;
                case 'M':
                    j = 1000;
                    break;
                default:
                    break;
            }
            if (value < j) {
                value = j - value;
            } else {
                if (prev < j) {
                    j = j - prev - prev;
                }
                value = value + j;
            }
            prev = j;
        }
        return value;
    }
};
```

## Reviews
* [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS) 

    Hypermedia As The Engine Of Application State, 在我看来，就是在一个Restful请求返回中，体现出可以利用已知数据做什么后续操作的结构，这样可以减少一些重复开发。


## Tips
* IP地址和32位整数的相互转换，因为IP地址每一位区间都是8位无符号整数，即（0 ~ 255）：
    1. IP转32位整数，假设有IP为192.168.0.28，可以转换为，长度不足8位的，前面补0得到8位：
        * 192 = 11000000
        * 168 = 10101000
        * &nbsp;&nbsp;&nbsp; 0 = 00000000
        * &nbsp; 28 = 00011100
    
         计算后，合并后得到二级制数据11000000101010000000000000011100，转换为十进制得到3232235548。这个数就是IP的32位整数表示法。
    2. 32位整数转IP，假设有整数678941235，我们可以转换二进制得到101000011101111101001000110011，其中二进制数字长度不足24位，所以我们在前面补2个0，可以转换为：
        * 00101000 = 40
        * 01110111 = 119
        * 11010010 = 210
        * 00110011 = 51

        这样我们就算得了IP是40.119.210.51。
    3. 注意点：IP地址最大不超过255.255.255.255，也就意味着二进制数据不能大于24个1。

## Share

* 摘自 << Redis Microservices For Dummies >> Chapter1：When breaking down (decomposing) a monolithic architecture, you have worry about many factors:
    
    1. <B>Gradualizing into services:</B> Moving from a single deliverable to many small services is a radical shift and requires careful planning.
    2. <B>Splitting at the right places:</B> When breaking a monolith, you have to take a step back and really think about where the most logical places are to divide responsibilities into services. At the same time, you want to avoid getting stuck in how your current application is built; instead, rethink without the technical debt of the past.
    3. <B>Transitioning a team:</B> Making changes to monoliths is really easy — perhaps too easy — in code. While you may need to touch dozens of function calls to deploy a single feature, your team can do this easily (testing and deploying is another issue). Isolating teams to build services also isolates teams from making changes in places for which they aren’t responsible. A double-edged sword!

    逐步服务化、在正确的地方划分服务和团队过渡，这三个点都很重要，一个巨无霸的项目不可能一朝一夕就完成全部服务化，肯定要有一个过程，这就需要我们在进行服务化的时候更加的小心，在逐步服务化的过程中，肯定会碰到一些逻辑巨复杂的功能，这个时候就需要我们抛去已有逻辑处理和技术实现给我们带来的影响，站在一个更高的角度来看待服务怎么划分；服务化的过程中还会碰到团队过渡的问题，以往的一个人多功能修改的，甚至一个功能多个人修改的情况，出现问题会很难进行责任划分，在逐步服务化的过程中，也要把团队内每个人负责的模块设置好，不要出现责任划分不清晰的情况。