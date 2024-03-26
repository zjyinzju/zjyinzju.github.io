# 作业订正
-----------------------------------
## week 1

??? question "时间复杂度计算"
    The recurrent equations for the time complexities of programs P1 and P2 are:

    + P1: T(1)=1,T(N)=T(N/3)+1
    + P2: T(1)=1,T(N)=3T(N/3)+1

    Then the correct conclusion about their time complexities is:
    
    >A.they are both O(logN)
    
    >B.O(logN) for P1, O(N) for P2
    
    >C.they are both O(N)
    
    >D.O(logN) for P1, O(NlogN) for P2
    ??? info "solution"
        B. P1类似于二分，P2显然为线性，想计算也可以递推到$O(1)$.例如p1:

        $T(N) = T(N/3) + 1 = T(N/3^2) + 2 = \cdots = T(N/3^k) + k = \cdots $

        Let $ N/3^k = 1$, then $k = \log_{3}{N} $
----------------------------------------
## week 2

??? question "时间复杂度分析"
    For a sequentially stored linear list of length N, the time complexities for deleting the first element and inserting the last element are O(1) and O(N), respectively.
    ??? info "solution"
        False. 文字游戏：“a sequentially stored linear list” 一般指数组，删除第一个元素需要移动N-1个元素，插入最后一个元素需要移动0个元素，所以时间复杂度分别为O(N)和O(1).
----------------------------------------

## week 3

??? question "队列"
    (HW3) Represent a queue by a singly linked list. Given the current status of the linked list as 1->2->3 where x->y means y is linked after x. Now if 4 is enqueued and then a dequeue is done, the resulting status must be:

    >A. 1->2->3
    
    >B. 2->3->4
    
    >C. 4->1->2
    
    >D. not unique  

    ??? info "solution"
        B.

        注意到由单向链表实现的队列，入队一定在链表尾，出队一定在链表头。

        否则如果出队在尾，则会删除尾节点，尾指针rear无法移动至新的尾节点。
----------------------------------------
## week 4
??? question "树的表示"
    (HW4) It is always possible to represent a binary tree by a one-dimensional integer array
    ??? info "solution"
        True 
        
        二叉树的可以用一维数组表示，而任何树可以通过First-Child-Next-Sibling表示法转化为二叉树。

??? question "遍历顺序"
    (HW4) If a general tree T is converted to a binary tree BT,then which of the following BT traversal gives the same sequence as that of the post-order traversal of T?
    >A. Pre-order

    >B. In-order

    >C. Post-order

    >D. Level-order

    ??? info "solution"
        C. 

        任何树通过FirstChild-NextSibling表示法转化为二叉树.

        T 的先序遍历和 BT 的先序遍历相同，T 的后序遍历和 BT 的中序遍历相同。