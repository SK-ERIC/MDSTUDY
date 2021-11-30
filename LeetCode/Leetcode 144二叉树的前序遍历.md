# LeetCode 144：二叉树的前序遍历

给你二叉树的根节点 root ，返回它节点值的 前序 遍历。

**示例 1：**

![144_1.jpg](https://i.loli.net/2021/05/28/6LmW3Crq4hpGnzb.jpg)

```javascript
输入：root = [1,null,2,3]
输出：[1,2,3]
```

**示例 2：**

```javascript
输入：root = []
输出：[]
```

**示例 3：**

```javascript
输入：root = [1]
输出：[1]
```

**示例 4：**

![144_2.jpg](https://i.loli.net/2021/05/28/P6EgbYudxjOXQtN.jpg)

```javascript
输入：root = [1,2]
输出：[1,2]
```

**示例 5：**

![144_3.jpg](https://i.loli.net/2021/05/28/6qdG3mPaAXEfgo8.jpg)

```javascript
输入：root = [1,null,2]
输出：[1,2]
```

**提示：**

- 树中节点数目在范围 [0, 100] 内
- -100 <= Node.val <= 100

**进阶：**递归算法很简单，你可以通过迭代算法完成吗？

## 方法一：递归

### 思路与算法

首先需要了解什么是二叉树的前序遍历：
按照访问 [根节点 —— 左子树 —— 右子树] 的方式遍历这棵树。
而在访问左子树和右子树的时候，我们按照同样的方式遍历，直到遍历完整棵树。

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */

/**
 * @param {TreeNode} root
 * @return {number[]}
 */
function preorderTraversal(root) {
  const res = [];
  const preorder = (root) => {
    if (!root) return;
    res.push(root.val);
    preorder(root.left);
    preorder(root.right);
  };
  preorder(root);
  return res;
}
```

#### 复杂度分析

- 时间复杂度：_O(n)_，其中 _n_ 是二叉树的节点数。每一个节点恰好被遍历一次。

- 空间复杂度：_O(n)_，为递归过程中栈的开销，平均情况下为 _O(logn)_，最坏情况下树呈现链状，为 _O(n)_。
