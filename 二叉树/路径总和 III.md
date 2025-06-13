------

# 路径总和 III

## 📌 题目描述

给定一个二叉树的根节点 `root` 和一个整数 `targetSum`，求该树中**路径和等于 targetSum 的路径数量**。

- 路径不需要从根节点开始，也不需要在叶子节点结束；
- 但是路径方向必须是**从父节点到子节点**（向下）。

------

## 🔍 解法一：前缀和 + DFS + 回溯（推荐 ✅）

### ✅ 思路解析

我们使用**前缀和 HashMap**记录路径上所有节点值的和，借助 DFS 实现遍历。

### 🔑 核心公式：

```text
当前前缀和 currSum
若 currSum - targetSum = prevSum 存在于前缀和哈希表中，
说明有一条从某节点到当前节点的路径和为 targetSum。
```

### 🧩 Java 实现代码

```java
public class Solution {
    private int count = 0;
    private Map<Long, Integer> prefixSumCount = new HashMap<>();

    public int pathSum(TreeNode root, int targetSum) {
        prefixSumCount.put(0L, 1);  // 初始化：前缀和 0 出现一次
        dfs(root, 0L, targetSum);
        return count;
    }

    private void dfs(TreeNode node, long currSum, int targetSum) {
        if (node == null) return;

        currSum += node.val;

        count += prefixSumCount.getOrDefault(currSum - targetSum, 0);

        prefixSumCount.put(currSum, prefixSumCount.getOrDefault(currSum, 0) + 1);

        dfs(node.left, currSum, targetSum);
        dfs(node.right, currSum, targetSum);

        // 回溯，撤销当前节点的前缀和
        prefixSumCount.put(currSum, prefixSumCount.get(currSum) - 1);
    }
}

class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int x) { val = x; }
}
```

------

## 🧠 知识点归纳

### 1. `currSum` 的作用与范围

- 它是当前路径上的节点值累加和；
- 是一个递归函数的参数，**每一层递归都有自己的副本**；
- 在 Java 中是值传递，不会影响上层调用，不需要手动回滚。

### 2. `prefixSumCount` 的作用

- 是全局共享的，用于记录「前缀和出现次数」；
- 用于查找是否存在某一段路径使得和为 target；
- **回溯阶段必须手动撤销当前节点的贡献**，否则会影响其它路径计算。

### 3. DFS 遍历顺序

- 是**先序遍历（Pre-order）**：当前 → 左 → 右；
- 因为我们需要在遍历子树前就处理当前节点的前缀和逻辑。

------

## ✅ 时间与空间复杂度

| 项目       | 分析                                     |
| ---------- | ---------------------------------------- |
| 时间复杂度 | O(N)，每个节点只访问一次                 |
| 空间复杂度 | O(N)，最坏情况下哈希表存储每个节点前缀和 |

------

## 📚 总结

- 当前缀和用于树路径问题时，必须搭配回溯，确保不同路径间数据不交叉污染；
- 利用 DFS 的“函数调用栈天然记录路径”这一特性，可以高效完成路径和统计；
- `currSum` 不需要回退是因为它是每层递归的局部变量，而 `prefixSumCount` 是共享状态。

