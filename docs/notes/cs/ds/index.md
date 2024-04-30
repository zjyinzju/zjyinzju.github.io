# 数据结构基础
!!! tip "浙大课程笔记和自学笔记"
    教师：陈超超

    参考（其实感觉就是照抄，毕竟教学的ppt都是统一的）：xg & Q 的笔记

!!! warning "尚未完工"
    课好多，我相似
??? abstract "课程内容大纲"

    1. 第一节课介绍分数构成、作业形式等重要内容！
    2. 复杂度分析
        - 大 O、大 Ω、大 θ、小 o
    2. 栈和队列
        - 中缀表达式转后缀表达式
        - 中缀表达式转前缀表达式
        - 表达式树
    3. 树
        - 前、中、后序遍历
        - threaded binary tree 线索二叉树
        - 完全二叉树、满二叉树
    4. 二叉搜索树
        - 查找、插入
        - 删除根节点
        - 支持删除指定节点(带 lazy tag 后的查找和删除)
    5. 堆
        - 线性建堆及其复杂度证明
        - push & pop
        - d-heap（满足堆性质的 d 叉树）: 
            - push & pop 复杂度
            - 父亲编号、最大的儿子编号、最小的儿子编号
    7. 并查集
        - union-by-size 及其复杂度证明
        - union-by-depth 及其复杂度证明
        - 路径压缩
    8. 图
    9. 最短路算法
        1. Floyd
        2. Dijkstra
            - 堆优化
            - 可以处理负权边吗？
        3. Bellman-Ford & SPFA
        4. 拓扑排序
    10. 其他图论算法
        1. 最小生成树
            - Kruskal
            - Prim
        2. 最大流
            - 最大流最小割定理，平面图最大流的对偶图方法，及其局限性（课程不涉及，但可以加快手算最大流的速度）
            - 增广路算法：
                1. 什么是反向边？
                2. Dinic & 当前弧优化
    11. DFS 的应用：
        1. 欧拉路径（回路）和哈密尔顿路径
        2. **无向图**的双连通分量
            - 定义（一个关节点可以出现在多个双连通分量中吗？）
            - tarjan 算法
        3. **有向图**的强联通分量
            - tarjan 算法
    12. 排序：
        - 插入排序
        - 希尔排序
        - 堆排序
        - 快速排序
        - 归并排序
        - table sort
        - bucket sort（桶排序） & radix sort（基数排序）
        - 其他
            - 稳定的排序
            - 基于交换的排序的复杂度下界证明
    13. Hash
        - 哈希函数：自变量是整数的情况，自变量是字符串的情况
        - 开放寻址法
            1. linear probing 循环找下一个位置直到找到空位
            2. quadratic probing: 往后找 1, 2, 4, ... 个位置
            3. double hashing: `f(i)=i*hash2(x)` 探测的步长与 key 值有关
            4. rehashing
        4. seperate chaining: 对相同哈希值用链表存储
        - 删除（tag）    