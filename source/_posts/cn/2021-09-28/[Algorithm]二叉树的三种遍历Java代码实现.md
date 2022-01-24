---
title: '[Algorithm]二叉树的三种遍历Java代码实现'
catalog: true
date: 2020-09-28
subtitle: Binary tree | Three traversal methods 
lang: cn
header-img: 0.png
tags:
- Java
- Data Structures
- Algorithms
- Tree
- Binary tree
categories:
- Data Structures and Algorithms
---

---
### 概念
**二叉树**(binary tree)是一颗树，其中每个节点都不能多于两个的儿子。 

![image.png](1.png)

#### 前序遍历
前序遍历：根节点+左子树+右子树。
在遍历左子树和右子树时，仍然先访问根节点，然后遍历左子树，最后遍历右子树。

结果: GEDACHS

#### 中序遍历
中序遍历：左子树+根节点+右子树。
在遍历左右子树时，仍然先遍历左子树，再遍历根节点，后遍历右子树。

结果: DEAGHCS

#### 后序遍历
后序遍历：左子树+右子树+根节点。
在遍历左右子树时，仍然先遍历左子树，在遍历右子树，后访问根节点。

结果: DAEHSCG

### 代码实现
#### 定义树的类
```java
public class TreeNode {
     int val;
     TreeNode left;
     TreeNode right;
     TreeNode() {}
     TreeNode(int val) { this.val = val; }
     TreeNode(int val, TreeNode left, TreeNode right) {
         this.val = val;
         this.left = left;
         this.right = right;
     }
}
```

#### 前序
```java
/**
* result: 返回顺序的List<Integer>
* @param root
* @param result
*/
public void preOrderTraverse (TreeNode root,List<Integer> result){
    if(root == null){
      return;
    }
    result.add(root.getVal());
    preOrderTraverse(root.left,result);
    preOrderTraverse(root.right,result);
}
```

#### 中序
```java
/**
* result: 返回顺序的List<Integer>
* @param root
* @param result
*/
public void inOrderTraverse (TreeNode root,List<Integer> result){
    if(root == null){
      return;
    }
    preOrderTraverse(root.left,result);
    result.add(root.getVal());
    preOrderTraverse(root.right,result);
}
```

#### 后序
```java
/**
* result: 返回顺序的List<Integer>
* @param root
* @param result
*/
public void postOrderTraverse (TreeNode root,List<Integer> result){
    if(root == null){
      return;
    }
    preOrderTraverse(root.left,result);
    preOrderTraverse(root.right,result);
    result.add(root.getVal());
}
```

### 迭代的方式
```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> res = new ArrayList<Integer>();
    Deque<TreeNode> stk = new LinkedList<TreeNode>();
    while (root != null || !stk.isEmpty()) {
      while (root != null) {
        stk.push(root);
        root = root.left;
      }
      root = stk.pop();
      res.add(root.val);
      root = root.right;
    }
    return res;
  }
```

### Morris
```java
// TODO 
```

### References
<https://leetcode-cn.com/problems/binary-tree-inorder-traversal/>
