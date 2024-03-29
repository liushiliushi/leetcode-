
# 链接
[link](https://leetcode.com/problems/valid-parentheses/submissions/)
# 思路
栈的思路
是按照书上的思路来写的，用到了链栈。因为没有现成的栈，所以自定义了栈这种数据结构。
比较容易混乱的地方在于，链表是新设立的结点在lastPtr后，而链栈的话，新设立的结点的nestPtr是上一个结点
# c
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190825195153689.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDgxNDEyMQ==,size_16,color_FFFFFF,t_70)
```c
struct listnode{
    char ch;
    struct listnode * nextPtr;
};//stack  data structure
typedef struct listnode LISTNODE;
typedef LISTNODE * LISTNODEPTR;
bool isValid(char * s){
    char ch;//a character
    int i = 0;
    ch = s[i];
    LISTNODEPTR downPtr = NULL, currentPtr = NULL, temPtr = NULL;
    downPtr = malloc(sizeof(LISTNODE));
    downPtr->nextPtr = NULL;
    int result = 1;
    while(ch && result)
    {
        if(ch == '('||ch == '['||ch == '{')//压入栈
        {
            currentPtr = malloc(sizeof(LISTNODE));
            currentPtr->ch = ch;
            currentPtr->nextPtr = downPtr;
            downPtr = currentPtr;
        }
        else//判断
        {
            if(ch == ')')
            {
                if(downPtr->ch == '(')
                {
                    temPtr = downPtr;
                    downPtr = downPtr->nextPtr;
                    free(temPtr);
                    temPtr = NULL;
                }
                else
                {
                    result = 0;
                }
            }
            else if(ch == ']')
            {
                if(downPtr->ch == '[')
                {
                    temPtr = downPtr;
                    downPtr = downPtr->nextPtr;
                    free(temPtr);
                    temPtr = NULL;
                }
                else
                {
                    result = 0;
                }
            }
            else
            {
                if(downPtr->ch == '{')
                {
                    temPtr = downPtr;
                    downPtr = downPtr->nextPtr;
                    free(temPtr);
                    temPtr = NULL;
                }
                else
                {
                    result = 0;
                }
            }
        }
        //prepare for the nest loop
        i++;
        ch = s[i];
    }
    if(result && (downPtr->nextPtr == NULL))
    {
        return 1;
    }
    else
    {
        return 0;
    }

}
```
貌似占用很多内存？试图从solution里面找到别的C语言解法失败。果然也只有我还在用C语言吧orz。![](https://img-blog.csdnimg.cn/20190825113251476.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDgxNDEyMQ==,size_16,color_FFFFFF,t_70)
# python
## 第一版
  。。。python用的太不熟练了，debug用了好久orz。第一个版本是大致看了一眼官方版本的思路后写的，代码简洁规范美观没有一点沾边hhh。还用了一个`topchar`记录最上面的字符，莫名感觉好傻啊，虽然也想不到更好地办法。不过python写起来，确实感觉比c好用很多啊，尤其是弱类型这一点。
  **注意** 
  - k in d 检查的是字典中的键，而不是值
  - 序列不能越界!!!
  ```python
  class Solution:
    def isValid(self, s):
        
        stack = ['#']
        mapping = {')':'(', ']':'[', '}':'{'}
        topchar = '#'
        for char in s:
            # if the character is a closing bracket
            if char in mapping:
                if topchar == mapping[char]:
                    # pull the top character from the stack
                    stack.pop()
                    topchar = stack[-1] if stack[-1] != '#' else '#'
                else:
                    return False
            # if the character is an opening bracket
                
            else:
                # push it onto the stack
                stack.append(char)
                topchar = char
        # if the stack is empty, return true
        if stack == ['#']:
            return True
        else:
            return False
   ```
   又占用了好多内存，是什么原因呢？![在这里插入图片描述](https://img-blog.csdnimg.cn/2019082615070653.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDgxNDEyMQ==,size_16,color_FFFFFF,t_70)
 ## 第二版
 这个是官方给出的题解
 嗯。。简洁规范美观
 思路不同的地方在于，一旦遇到了closing bracket，就直接将栈的顶部元素删除，**同时获取这个元素的值**，这样可以比对一下是否匹配。注意，删除的时候要考虑到栈是否溢出，序列不能越界。
 还有一个弱智问题。。。stack.pop()不是stack.pop

```python
class Solution(object):
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """

        # The stack to keep track of opening brackets.
        stack = []

        # Hash map for keeping track of mappings. This keeps the code very clean.
        # Also makes adding more types of parenthesis easier
        mapping = {")": "(", "}": "{", "]": "["}

        # For every bracket in the expression.
        for char in s:

            # If the character is an closing bracket
            if char in mapping:

                # Pop the topmost element from the stack, if it is non empty
                # Otherwise assign a dummy value of '#' to the top_element variable
                top_element = stack.pop() if stack else '#'

                # The mapping for the opening bracket in our hash and the top
                # element of the stack don't match, return False
                if mapping[char] != top_element:
                    return False
            else:
                # We have an opening bracket, simply push it onto the stack.
                stack.append(char)

        # In the end, if the stack is empty, then we have a valid expression.
        # The stack won't be empty for cases like ((()
        return not stack
```
## 第三版
 参照网友的写法
 其实我觉得这个思路是最容易想到的
 **注意：elif 里面判断条件是有顺序的**
 

```python
class Solution:
    def isValid(self, s):
        stack = []
        mapping = {'(':')', '[':']', '{':'}'}
        open_par = set(['(', '[', '{'])
        for char in s:
            if char in open_par:
                stack.append(char)
            elif stack and char == mapping[stack[-1]]:
                stack.pop()
            else:
                return False
        return stack == []
                
```
## 第四版
这是消消乐吗？也太强了叭2333
不过用时很长

```python
class Solution:
    def isValid(self, s):
        while '()' in s or '[]' in s or '{}' in s:
            s = s.replace('()','').replace('[]','').replace('{}','')
        return s == ''                                
```
# c++
等我学会了再来写吧
