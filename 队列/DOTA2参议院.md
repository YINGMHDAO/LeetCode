------

# Dota2 参议院模拟投票问题

## 🧩 题目简介

- 给定一个字符串 `senate`，表示 Dota2 世界中的参议员序列：
  - `'R'` 代表 Radiant（天辉）阵营；
  - `'D'` 代表 Dire（夜魇）阵营。
- 每一轮中，参议员可以：
  - 禁止另一位对方参议员的权利（即 ban 掉对方）；
  - 如果剩余的参议员全是同一阵营，则宣布胜利。

## 🎯 目标

模拟投票过程，预测哪一方最终获胜。输出 `"Radiant"` 或 `"Dire"`。

------

## 🧠 解法思路

### ✅ 核心思想：队列 + 下标模拟轮次

- 用两个队列分别存储 `'R'` 和 `'D'` 的参议员 **下标**；
- 每一轮中，弹出两个队列的最前面元素 `R` 和 `D`；
- 谁的下标小（即谁先出现），谁就 ban 掉对方，并把自己的下标加 `n` 后重新加入队列，表示下一轮再投票。

### 🔁 为什么要加 `n`？

- 防止当前轮投过票的议员插队参与下一轮；
- 通过 `index + n` 实现“轮”的概念，让投票顺序保持公平。

------

## ✅ Java 实现代码

```java
class Solution {
    public String predictPartyVictory(String senate) {
        int n = senate.length();
        Deque<Integer> dDeque = new ArrayDeque<>();
        Deque<Integer> rDeque = new ArrayDeque<>();
        for (int i = 0; i < n; i++) {
            if (senate.charAt(i) == 'R') {
                rDeque.offerLast(i);
            } else {
                dDeque.offerLast(i);
            }
        }
        while (!rDeque.isEmpty() && !dDeque.isEmpty()) {
            int R = rDeque.pollFirst();
            int D = dDeque.pollFirst();
            if (R < D) {
                rDeque.offerLast(R + n);
            } else {
                dDeque.offerLast(D + n);
            }
        }
        return rDeque.isEmpty() ? "Dire" : "Radiant";
    }
}
```

------

## ⏱️ 复杂度分析

| 分类       | 说明                             |
| ---------- | -------------------------------- |
| 时间复杂度 | O(n)，每个议员最多出场一次       |
| 空间复杂度 | O(n)，用于存储两个阵营的下标队列 |

------

## ❌ 常见错误分析

### 错误做法：不加 `n` 直接把胜者放回队尾

- 可能导致议员“插队”，在同一轮多次出场；
- 会出现死循环或错误结果；
- 无法正确模拟轮流投票机制。

------

## ✅ 总结

- 本题的精髓是用下标模拟轮次 + `index + n` 控制下一轮投票；
- 队列是处理这类“循环淘汰 + 顺序投票”的经典数据结构；
- 建议掌握类似问题的处理框架，如约瑟夫问题、击鼓传花等变形题。

------