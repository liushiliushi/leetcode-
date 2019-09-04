
参考[link](https://github.com/liushiliushi/leetcode/blob/master/problems/53.maximum-sum-subarray-cn.md)以及众多网友的回答写出来的学习笔记
# 链接
[link](https://leetcode.com/problems/maximum-subarray/)
# 思路
- 暴力求解法，列举出所有可能的情况(最后在一组超大一坨数据的测试下time limit exceeded)
- 前缀和
# c
## 第一版
暴力解法
空间O（n^3）
时间O(1)
```c

int maxSubArray(int* nums, int numsSize){
    int i = 0;
    int j, k;
    int sum1 = 0;
    int sum = nums[0];
    for(i = 0; i <numsSize; i++)
    {
        for(j = i; j < numsSize; j++)
        {
            for(k = i; k <= j; k++)
            {
                sum1 += nums[k];
            }
            if(sum1 > sum)
            {
                sum = sum1;
            }
            sum1 = 0;
        }
    }
    return sum;
}


```
## 第二版
前缀和法
空间O(n^3)
时间O（n）

```c
int maxSubArray(int* nums, int numsSize){
    int *prefix;
    prefix = malloc(sizeof(int) * numsSize);
    int i, j;
    int sum1 = 0;
    int sum = nums[0];
    for(i = 0; i <numsSize; i++)
    {
        for(j = 0; j <= i; j++)
        {
            sum1 += nums[j];
        }
        prefix[i] = sum1;
        sum1 = 0;
    }
    for(i = 0; i < numsSize; i++)
    {
        if(prefix[i] > sum)
        {
            sum = prefix[i];
        }
        for(j = 0; j < i; j++)
        {
            sum1 = prefix[i] - prefix[j];
            if(sum1 > sum)
            {
                sum = sum1;
            }
            sum1 = 0;
        }
    }
    return sum;
}


```
## 第三版
优化前缀和
计算前缀和的时候直接找到最大前缀和和最小前缀和，二者差即为最大和
写的过程中出现了种种问题orz
比如只有一个数的情况
## 第四版
动态规划
有种递归的感觉？`dp[i] = max(dp[i - 1] + nums[i], nums[i])`
`dp[i]`表示到当前位置的最大连续子列和。同时需要另外设一个sum，每次比较，记录最大的子列和。

```c
int maxSubArray(int* nums, int numsSize){
    int current_sum = nums[0];
    int sum = nums[0];
    int i;
    for(i = 1; i < numsSize; i++)
    {
        current_sum = ((current_sum + nums[i]) > nums[i])?(current_sum + nums[i]):nums[i];
        sum = (current_sum > sum) ? current_sum : sum;
    }
    return sum;
}


```
# python 
## 第一版
优化前缀版

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        len_ = len(nums)
        min_ = 0
        max_ = nums[0]
        tem = 0
        for i in range(len_):
            tem += nums[i]
            min_ = min(min_, tem)
            max_ = max(max_, tem)
        if max_ == min_:
            return max_
        else:
            return max_ - min_
        
```
结果出错了。。。因为没有考虑到顺序。只是单纯找到最大最小
**max_必须在min_后面**
解决办法：在计算过程中即计算出来当前的`sum=最大-最小` 这样就不会出现顺序混乱的问题了（这里的最小是已经出现的，不会有还没出现的情况）
## 第二版
dp
**注意**先算sum再求min

```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        len_ = len(nums)
        min_ = 0
        tem = 0
        sum_ = nums[0]
        for i in range(len_):
            tem += nums[i]
            sum_ = max(sum_, tem - min_)
            min_ = min(min_, tem)
        return sum_
        
```
