
# 链接
[link](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
# python
刚开始一看到题目，简直不要太开心。这不就是用集合处理一下就能去除重复元素吗？但是忘记集合是无序的。。。所以序列的元素不是按照原来的顺序排列的。不过再重新排一下就好啦~
## 第一版

```python
class Solution:
    def removeDuplicates(self, nums) :
        
        a = set(nums)
        b = len(a)
        nums.clear()
        for i in a:
            nums.append(i)
        return b
```

## 第二版
记录数组的长度，然后@！#￥￥（懒得写了
注意后面不要的切片要删除
```python
class Solution:
    def removeDuplicates(self, nums) :
        len_ = 1
        for i in range(1, len(nums)):
            if nums[i] != nums[i-1]:
                nums[len_] = nums[i]
                len_ += 1
        nums[len_:] = []        
        return len_
       
```
## 第三版
直接删除元素
**注意这里的数组长度一直在变化，所以要用len()**
```python
class Solution:
    def removeDuplicates(self, nums) :
        i = 0
        # 当还没到倒数第一个元素的时候
        while i < len(nums) - 1:
            if nums[i] == nums[i+1]:
                del nums[i]
            else:
                i += 1
        return  len(nums)
```
## 第四版
双指针版
感觉像是在原有数组里面写新数组
当然必须有序才能这样写

```python
class Solution:
    def removeDuplicates(self, nums) :
        slow = 0
        for fast in range(1, len(nums)):
            if nums[slow] == nums[fast]:
                fast += 1
            else:
                slow += 1
                nums[slow] = nums[fast]
                fast += 1
        return slow + 1
```

# c
貌似不能用c来写？oj会给一个数组，但是不知道数组的长度啊，这一点不知道怎么解决QAQ
