- [算法解题规则及相关定义](#算法解题规则及相关定义)
    - [树相关](#树相关)
        - [二叉查找树/二叉搜索树](#二叉查找树/二叉搜索树)
        - [平衡二叉树/平衡二叉查找树AVL](#平衡二叉树/平衡二叉查找树AVL)
        - [平衡多路查找树B-Tree](#平衡多路查找树B-Tree)
        - [红黑树](#红黑树)
    - [数组相关](#数组相关)
        - [求两个区间的交集](#求两个区间的交集)
    - [位运算](#位运算)
        - [取余](#取余)
    - [排序](#排序)
    - [穷举](#穷举)
    - [动态规划](#动态规划)

# 算法解题规则及相关定义
## 树相关

### 二叉查找树/二叉搜索树
具有以下特点：
1. 节点的左子树只包含小于当前节点的数
2. 节点的右子树只包含大于当前节点的数
3. 节点的左子树和右子树自身也是二叉搜索树

### 平衡二叉树/平衡二叉查找树AVL
1. 对于任意节点，其左子树的键值小于该节点，右子树的键值大于该节点
2. 对于任意节点，其左子树和右子树的高度差最大为1

### 平衡多路查找树B-Tree


### 红黑树
红黑树满足一下特点：
1. 每个节点不是黑色就是红色
2. 根节点是黑色
3. 叶子节点都是黑色
4. 所有红色节点的子节点一定是黑色
5. 从根节点到所有叶子节点路径中，黑色节点个数相等

## 数组相关
### 求两个区间的交集
假设有两个区间[x1, y1]和[x2, y2]：
如果两区间无交集则满足：`y1 < x2 || y2 < x1`；
反之，如果有交集，则满足：`y1 >= x2 && y2 >= x1`

## 位运算
### 取余
当b是2的n次幂时，满足以下等式：
`a % b` 等价于 `a & (b - 1)`，且后者效率高

## 排序
- 如果是数据排序方面的题，那基本上是和二分查找有关系的
- 如果是在一个无序数组上的搜索或者统计，基本上来说需要动用 O(1) 时间复杂度的 hash 数据结构
- 在一堆无序的数据中找 top n 的算法，基本上来说，就是使用最大堆或是最小堆的数据结构

## 穷举
如果是穷举答案相关的题（如八皇后、二叉树等），基本上来说，需要使用深度优先、广度优先或是回溯等递归的思路

## 动态规划
