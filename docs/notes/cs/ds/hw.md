# 作业订正
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

