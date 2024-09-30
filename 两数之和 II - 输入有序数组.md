![image-20240928161226433](https://gitee.com/Black_aura/picture/raw/master/img/image-20240928161226433-1727573667634-1.png)

> ✏️ 关于专栏：专栏用于记录 [LeetCode](https://leetcode.cn/) 中做题与总结  
> 😭 补救 😭

[TOC]

## 两数之和 II - 输入有序数组

### 题目描述

题目链接：[167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)

给你一个下标从 **1** 开始的整数数组 `numbers` ，该数组已按 **非递减顺序排列** ，请你从数组中找出满足相加之和等于目标数 `target` 的两个数。如果设这两个数分别是 `numbers[index1]` 和 `numbers[index2]` ，则 `1 <= index1 < index2 <= numbers.length` 。

以长度为 2 的整数数组 `[index1, index2]` 的形式返回这两个整数的下标 `index1` 和 `index2`。

你可以假设每个输入 **只对应唯一的答案** ，而且你 **不可以** 重复使用相同的元素。

你所设计的解决方案必须只使用常量级的额外空间。

### 题目示例

**示例 1：**

```
输入：numbers = [2,7,11,15], target = 9
输出：[1,2]
解释：2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```

**示例 2：**

```
输入：numbers = [2,3,4], target = 6
输出：[1,3]
解释：2 与 4 之和等于目标数 6 。因此 index1 = 1, index2 = 3 。返回 [1, 3] 。
```

**示例 3：**

```
输入：numbers = [-1,0], target = -1
输出：[1,2]
解释：-1 与 0 之和等于目标数 -1 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。
```

### 题目提示

- `2 <= numbers.length <= 3 * 104`
- `-1000 <= numbers[i] <= 1000`
- `numbers` 按 **非递减顺序** 排列
- `-1000 <= target <= 1000`
- **仅存在一个有效答案**

### 思路&代码

这道题与之前遇到的[1. 两数之和](https://leetcode.cn/problems/two-sum/)的基础上测试用例的范围增大，且数组改为有序数组。那么之前的遍历枚举就会出现超时情况，利用哈希的还是可以用，但不推荐，毕竟这是一个**有序数组**。

#### **1.哈希表**

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        unordered_map<int,int> mp;
        for(int i = 0;i < numbers.size();i++){
            auto it = mp.find(target - numbers[i]);
            if(it != mp.end()){
                return {it->second+1,i+1};
            }
            mp[numbers[i]] = i;
        }
        return {};
    }
};
```

需要注意的是这里题目要求下标**从1开始**，所以返回时候需要加1，具体的思路可以查看https://blog.csdn.net/m0_74014989/article/details/142617788?fromshare=blogdetail&sharetype=blogdetail&sharerId=142617788&sharerefer=PC&sharesource=m0_74014989&sharefrom=from_link

#### **2.双指针**

首先我们先来清楚一下什么是双指针。

**双指针（Two Pointers）**：指的是在遍历元素的过程中，不是使用单个指针进行访问，而是使用两个指针进行访问，从而达到相应的目的。如果两个指针方向相反，则称为「对撞指针」。如果两个指针方向相同，则称为「快慢指针」。如果两个指针分别属于不同的数组 / 链表，则称为「分离双指针」。

解题思路：我们定义left = 0,right = numbers.size() - 1两个指针，即分别指向numbers数组的左右两边，每次计算两个指针指向的两个元素之和，并和目标值比较。如果两个元素之和等于目标值，则发现了唯一解。如果两个元素之和小于目标值，则将左侧指针右移一位。如果两个元素之和大于目标值，则将右侧指针左移一位。移动指针之后，重复上述操作，直到找到答案。

<img src="https://gitee.com/Black_aura/picture/raw/master/img/image-20240929123912109.png" alt="image-20240929123912109" style="zoom: 50%;" />

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int left = 0,right = numbers.size() - 1;
        while(left < right){
            if(numbers[left] + numbers[right] == target){
                return {left+1,right+1};
            }else if(numbers[left] + numbers[right] < target){
                left++;
            }else{
                right--;
            }
        }
        return {};
    }
};
```

#### **3.二分查找**

首先固定第一个数，然后寻找第二个数，第二个数等于目标值减去第一个数的差。利用数组的有序性质，可以通过二分查找的方法寻找第二个数。为了避免重复寻找，在寻找第二个数时，只在第一个数的右侧寻找。

<img src="https://gitee.com/Black_aura/picture/raw/master/img/image-20240929131454497.png" alt="image-20240929131454497" style="zoom: 50%;" />

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        for(int i = 0;i < numbers.size();i++){
            int left = i + 1,right = numbers.size() - 1;
            while(left <= right){
                int mid = (left + right) / 2;
                if(numbers[mid] == target - numbers[i]){
                    return {i + 1,mid + 1};
                }else if(numbers[mid] < target - numbers[i]){
                    left = mid + 1;
                }else{
                    right = mid - 1;
                }
            }
        }
        return {};
    }
};
```

### 知识整理

#### 双指针简介

> **双指针（Two Pointers）**：指的是在遍历元素的过程中，不是使用单个指针进行访问，而是使用两个指针进行访问，从而达到相应的目的。如果两个指针方向相反，则称为「对撞指针」。如果两个指针方向相同，则称为「快慢指针」。如果两个指针分别属于不同的数组 / 链表，则称为「分离双指针」。

在数组的区间问题上，暴力算法的时间复杂度往往是 $O(n^2)$。而双指针利用了区间「单调性」的性质，可以将时间复杂度降$ O(n)$。

#### 对撞指针

**对撞指针**：指的是两个指针 $left$、$right$分别指向序列第一个元素和最后一个元素，然后  $left$指针不断递增， $right$ 不断递减，直到两个指针的值相撞（即 $left == right$​），或者满足其他要求的特殊条件为止。

![对撞指针](https://gitee.com/Black_aura/picture/raw/master/img/202405092155032.png)

##### 对撞指针求解步骤

1. 使用两个指针$left$，$right$。$left$指向序列第一个元素，即：$left = 0 $，$right$指向序列最后一个元素，即：$right = len(nums)−1$。
2. 在循环体中将左右指针相向移动，当满足一定条件时，将左指针右移，$left += 1$。当满足另外一定条件时，将右指针左移，$right -= 1。
3. 直到两指针相撞（即$left == right$​​），或者满足其他要求的特殊条件时，跳出循环体。

##### 对撞指针伪代码模板

```c++
int left = 0,right = nums.size() - 1;
while left < right:
    if 满足要求的特殊条件:
        return 符合条件的值 
    else if 一定条件 1:
        left += 1
    else if 一定条件 2:
        right -= 1

return 没找到 或 找到对应值
```

##### 对撞指针适用范围

对撞指针一般用来解决有序数组或者字符串问题：

- 查找有序数组中满足某些约束条件的一组元素问题：比如二分查找、数字之和等问题。
- 字符串反转问题：反转字符串、回文数、颠倒二进制等问题。

![d1fded34d21c16a44c687a112865ab0](https://gitee.com/Black_aura/picture/raw/master/img/d1fded34d21c16a44c687a112865ab0-1727508219022-1-1727583261315-1.jpg)