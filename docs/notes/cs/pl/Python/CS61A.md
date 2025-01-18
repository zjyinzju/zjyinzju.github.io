

## 基本语法与函数

+ `/`正常除法，得到浮点数; `//`向下取整

+ 赋值语句都是先算右值再依序与左侧名称绑定
```py
>>> x,y=3,4.5
    x,y=y,x
```

+ 函数定义：
```py
def <name>(<formal parameters>):
    return <return expression>
```

+ `print`等非纯函数返回值始终为`None`
```py
>>> print (print(1),print(2))
1
2
None None
```

+ `def`和赋值语句将名称与当前值绑定，且此前所有的绑定都失效

+ 函数及形参名称选取：
> 1. 单词之间用下划线
> 2. 函数名反映对参数的操作或结果
> 3. 参数名反映其作用

+ 函数是一种抽象:domain(接受的参数合集) → range(返回的参数合集) intent(计算输入和输出的关系以及可能产生的副作用)


## 条件控制 高阶函数 环境

+ 好的函数是抽象的：通用、简单、不重复
+ """docstring"""用于函数说明，`help(<function>)`可查看
+ Boolean Operators:`and` `or` `not` & short-circuiting:
```py
>>> -1 and 2 and 0 and 1
0
>>> False or 3124 or 1/0
3124 
```
换言之，从左向右，某位置的表达式已经可以保证整个语句的真假可判断时，计算结果为此表达式的结果，否则为最右侧值
+ 执行比较并返回布尔值的函数通常`is`开头且无下划线，如`isinstance` `isfinite`

??? abstract "condition statement"

    + if-elif-else
    ```py
    if <predicate>:
        <suite>
    elif <predicate>:
        <suite>
    else:
        <suite>
    ```
    + for loop
    ```py
    for <name> in <iterable>:
        <suite>
    ```
    + while loop
    ```py
    while <predicate>:
        <suite>
    ```
    + break/continue
    ```py
    while True:
        if <predicate>:
            break
        <suite>
    ```

+ 在函数定义中，其他函数也可以作为形参传入，这使得函数更通用也更抽象

??? note "eg"
    ```py
    >>> def summation (n, func):
    ...     total, k = 0 , 1
    ...     while k <= n:
    ...             total, k = total+func(k), k + 1
    ...     return total
    ...
    >>> def identity(x):
    ...     return x
    ...
    >>> def square(x):
    ...     return x*x
    ...
    >>> def y(x):
    ...     return 2*x
    ...
    >>> summation(3, y)
    12
    >>> summation(3, square)
    14
    ```

+ 嵌套地定义函数：被嵌套定义的函数的父栈帧不是全局帧，而是定义它的函数的局部帧，从而形成了从内到外的环境链。在计算函数内部的表达式时，Python会先在当前帧中查找，然后逐级向外查找，直到找到首个绑定的值。

+ 函数作为返回值：可以复合多个函数
```py
>>> def compose1(f, g):
        def h(x):
            return f(g(x))
        return h
```

+ Curring(柯里化):使用高阶函数将多元函数转化为只接受一个参数的函数链
```py
>>> def curried_pow(x):
            def h(y):
                return pow(x,y)
            return h
```
+ Lambda expression 是函数的简化，但可读性差
!!! note
    lambda    x : f(g(x))

    A function takes x and returns f(g(x))

??? info "others"
    === "ifelse"
        例如：
        >`abs_number = lambda x : x if x >= 0 else -x`

        是取x绝对值（有点像C里的`?`）
    === "利用短路"
        源自于hw02的mod_maker,利用`or`和 `and`的短路问题避免用`ifelse`而控制条件输出
        ```py
        >>> def mod_maker():
                return lambda x,y : x%y or True
            mod = mod_maker()
            '''
            >>>mod(4,8)
            4
            >>>mod(8,4)
            True
            '''
        ```


+ 函数装饰器（decorator）
??? abstract "example"
    ```py
    >>> def trace(fn):
            def wrapped(x):
                print('-> ', fn, '(', x, ')')
                return fn(x)
            return wrapped

    >>> @trace
        def triple(x):
            return 3 * x

    >>> triple(12)
    ->  <function triple at 0x102a39848> ( 12 )
    36
    ```

    