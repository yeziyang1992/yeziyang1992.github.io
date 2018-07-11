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