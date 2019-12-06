---
title: ARTS Week 1
date: 2019-12-06 03:14:10
index_img: /img/geektime/arts/continously.jpg
tags:
- geektime
- arts
categories:
- geektime
- arts
---

ARTS 打卡第一周

<!-- more -->
# ARTS第一周 (11.25-12.01)
## 1. Algrithm
### A. LeetCode 7. Reverse Integer

这是解题编码。

```python
class Solution:
    def reverse(self, x: int) -> int:
        if x == 0:
            return 0
        x_str = str(abs(x))
        direct_reverse_length = 9
        usigned_max_int_str = "2147483647"
        usigned_min_int_str = "2147483648"
        x_sign = int(x / abs(x))
        x_len = len(x_str)
        index = 0
        
        if x_len > direct_reverse_length:
            if x_len > 10:
                step = x_str[10:x_str]
                if int(step) > 0:
                    return 0
                else:
                    x_len = 10
            if x_sign > 0:
                limit_arr = list(usigned_max_int_str)
            else:
                limit_arr = list(usigned_min_int_str)
            last_index = x_len - 1
            x_arr = list(x_str[0:x_len])
            flag = True
            while last_index >= 0:
                if flag:
                    if x_arr[last_index] > limit_arr[index]:
                        return 0
                    elif x_arr[last_index] < limit_arr[index]:
                        flag = False
                limit_arr[index] = x_arr[last_index]
                index+=1
                last_index-=1
            x_str = ''.join(limit_arr)
        else:
            x_arr = list(x_str)
            last_index = x_len - 1
            while last_index > index:
                buff = x_arr[index]
                x_arr[index] = x_arr[last_index]
                x_arr[last_index] = buff
                index+=1
                last_index-=1
            x_str = ''.join(x_arr)
        result = int(x_str) * x_sign
        return result
```
## 2. Review
在Dzone上看到的，[3 Approaches to Storing Application Parameters and Metadata](https://dzone.com/articles/approaches-to-storing-application-parameters-metad)。

原先我以为把数据解析成csv逗号分隔符，数据可读性会很差，这个哥们给了我一个新的思路，就是可以在数据的前面加一个列名，用另外一个特殊字符隔开，这样子可读性就大大增强了。如(id:1,name:demo,passwd:123)，就是源数据(1,demo,123)的加强版，相对来说，可读性大大增强了。

## 3. Tip

* Dorado datapath的巧用，在更新数据的时候，可以用[#deleted]表示页面删除的数据，[#new]表示新建的数据。

## 4. Share

* [3 Approaches to Storing Application Parameters and Metadata](https://dzone.com/articles/approaches-to-storing-application-parameters-metad)