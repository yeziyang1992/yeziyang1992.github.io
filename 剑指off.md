### 1. 二叉搜索树与双向链表
#### 题目描述
输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。
![](http://ww1.sinaimg.cn/large/006IYRZEly1ft5wmq037pj30su06smxz.jpg)
#### 解答思路
1. 核心算法依旧是中序遍历
2. 不是从根节点开始，而是从中序遍历得到的第一个节点开始
3. 定义两个辅助节点listHead(链表头节点)、listTail(链表尾节点)。事实上，二叉树只是换了种形式的链表；listHead用于记录链表的头节点，用于最后算法的返回；listTail用于定位当前需要更改指向的节点。了解了listHead和listTail的作用，代码理解起来至少顺畅80%。
4. 提供我画的算法的过程图，有点丑，但有助于理解（帮你们画了，你们就不用画啦），另外图中右上角步骤三应该是“2”标红，“2”和“1”中间的连接为单线黑~~~ 

![](http://ww1.sinaimg.cn/large/006IYRZEly1ft5wrpa18zj30vi1b9jwa.jpg)
#### 代码：
```python
class Solution:
    def __init__(self):
        self.listHead = None
        self.listTail = None
    def Convert(self, pRootOfTree):
        if pRootOfTree==None:
            return
        self.Convert(pRootOfTree.left)
        if self.listHead==None:
            self.listHead = pRootOfTree
            self.listTail = pRootOfTree
        else:
            self.listTail.right = pRootOfTree
            pRootOfTree.left = self.listTail
            self.listTail = pRootOfTree
        self.Convert(pRootOfTree.right)
        return self.listHead
```
#### 原博客地址：
https://blog.csdn.net/u010005281/article/details/79657259

### 2. 字符串的排列
#### 题目描述
输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。 
输入描述: 
输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。
#### 解题思路
很多人使用python的permutation库去做，那么还有什么意义呢？

这个题类似于【LeetCode】46. Permutations 解题报告，要做字符串的全排列。做法是把对字符串中的每个元素都当做起始位置，把其他元素当做以后的位置，然后再同样的进行操作。这样就会得到全排列。

这个题不使用index的原因是，我们认为已经遍历过了的元素过滤掉了，只留下了未遍历的剩余的字符串当做ss。因此，所有的元素遍历完成了，ss也就是空了，就可以结束了。
#### 代码：
```python
class Solution:
    def Permutation(self, ss):
        if not ss:
            return []
        res = []
        self.helper(ss, res, '')
        return sorted(list(set(res)))

    def helper(self, ss, res, path):
        if not ss:
            res.append(path)
        else:
            for i in range(len(ss)):
                self.helper(ss[:i] + ss[i+1:], res, path + ss[i])
```
#### 原博客地址：
https://blog.csdn.net/fuxuemingzhu/article/details/79513101
### 3. 整数中1出现的次数（从1到n整数中1出现的次数）
#### 题目描述
求出1-13的整数中1出现的次数,并算出100-1300的整数中1出现的次数？为此他特别数了一下1-13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数。
#### 解题思路
思路在[这篇博客](http://www.cnblogs.com/nailperry/p/4752987.html)当中
#### 代码
```python
def NumberOf1Between1AndN_Solution(self, n):
    mult, sumTimes = 1, 0
    while n//mult > 0:
        high, mod = divmod(n, mult*10)
        curNum, low = divmod(mod, mult)
        if curNum > 1:
            sumTimes += high*mult + mult
        elif curNum == 1:
            sumTimes += high*mult + low + 1
        else:
            sumTimes += high*mult
        mult = mult*10
    return sumTimes
```
#### 原博客地址：
https://blog.csdn.net/u010005281/article/details/80085255
### 4. 把数组排成最小的数
#### 题目描述
输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。
#### 解题思路
用自定义规则的快速排序算法解决。即：和普通的快排算法相比，这道题的快排算法的排序规则不是单纯的比较两个数的大小，而是迎合题意，比较组合数的大小。
#### 代码
```python
def strSort(self, list):
    if len(list) <= 1:
        return list
    left = self.strSort([i for i in list[1:] if (i+list[0]) < (list[0]+i)])
    right = self.strSort([i for i in list[1:] if i+list[0] >= list[0] + i])
    return left+[list[0]]+right
```
为了更为直观地对比，以方便理解，文末给出普通的快排算法。
```python
def quick_sort(self, list):
    if len(list) < 2:
        return list[:]
    left = (self.quick_sort([i for i in list[1:] if i <= list[0]]))
    right = (self.quick_sort([i for i in list[1:] if i > list[0]]))
    return left + [list[0]] + right
```
快速排序的基本思想是     
>1、先从数列中取出一个数作为基准数  
>2、分区过程，将比这个数大的数全放到它的右边，小于或等于它的数全放到它的左边  
>3、再对左右区间重复第二步，直到各区间只有一个数  

#### 原博客地址
https://blog.csdn.net/u010005281/article/details/80085476     
https://blog.csdn.net/code_ac/article/details/74158681