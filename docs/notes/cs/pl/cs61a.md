# CS61A Fall 2023

## week1
> 基本语法与函数

+ 赋值语句都是先算右值再依序与左侧名称绑定
```py
>>> x,y=3,4.5
    x,y=y,x
```

+ `print`返回值始终为`None`
```py
>>> print (print(1),print(2))
1
2
None None
```

+ 函数及形参名称选取：
> 1. 单词之间用下划线
> 2. 函数名反映对参数的操作或结果
> 3. 参数名反映其作用

+ 函数是一种抽象:domain(接受的参数合集) → range(返回的参数合集) intent(计算输入和输出的关系以及可能产生的副作用)

+ `/`就正常除法，得到浮点数; `//`向下取整(类似C中int型的`/`)

## week2
> 条件控制 高阶函数 环境

+ 好的函数是抽象的：通用、简单、不重复
+ Python是以缩进划分语块的 (《繁星·春水》)
+ Boolean Operators:`and` `or` `not` & short-circuiting:
```py
>>> -1 and 2 and 0 and 1
0
>>> False or 3124 or 1/0
3124 
```
换言之，从左向右，某位置的表达式已经可以保证整个语句的真假可判断时，计算结果为此表达式的结果，否则为最右侧值
+ 执行比较并返回布尔值的函数通常`is`开头且无下划线，如`isinstance` `isfinite`
+ Nested function definition: Python中函数**可以嵌套定义**
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
## week3 
> 函数应用
+ 利用构建函数最终完成一段音频的制作
??? eg code 
    ```py
        # Example: Sound

    from wave import open
    #struct--encoding integers in the format that WAV files require
    from struct import Struct
    from math import floor

    frame_rate = 11025

    def encode(x):
        """Encode float x between -1 and 1 as two bytes.
        (See https://docs.python.org/3/library/struct.html)
        """
        i = int(16384 * x)
        return Struct('h').pack(i)

    def play(sampler, name='song.wav', seconds=2):
        """Write the output of a sampler function as a wav file.
        (See https://docs.python.org/3/library/wave.html)
        """
        out = open(name, 'wb')
        out.setnchannels(1)
        out.setsampwidth(2)
        out.setframerate(frame_rate)
        t = 0
        while t < seconds * frame_rate:
            sample = sampler(t)
            out.writeframes(encode(sample))
            t = t + 1
        out.close()

    def tri(frequency, amplitude=0.3):
        """A continuous triangle wave."""
        period = frame_rate // frequency
        def sampler(t):
            saw_wave = t / period - floor(t / period + 0.5)
            tri_wave = 2 * abs(2 * saw_wave) - 1
            return amplitude * tri_wave
        return sampler

    c_freq, e_freq, g_freq = 261.63, 329.63, 392.00

    play(tri(e_freq))

    def note(f, start, end, fade=.01):
        """Play f for a fixed duration."""
        def sampler(t):
            seconds = t / frame_rate
            if seconds < start:
                return 0
            elif seconds > end:
                return 0
            elif seconds < start + fade:
                return (seconds - start) / fade * f(t)
            elif seconds > end - fade:
                return (end - seconds) / fade * f(t)
            else:
                return f(t)
        return sampler

    play(note(tri(e_freq), 1, 1.5))

    def both(f, g):
        return lambda t: f(t) + g(t)

    c = tri(c_freq)
    e = tri(e_freq)
    g = tri(g_freq)
    low_g = tri(g_freq / 2)

    play(both(note(e, 0, 1/8), note(low_g, 1/8, 3/8)))

    play(both(note(c, 0, 1), both(note(e, 0, 1), note(g, 0, 1))))

    def mario(c, e, g, low_g):
        z = 0
        song = note(e, z, z + 1/8)
        z += 1/8
        song = both(song, note(e, z, z + 1/8))
        z += 1/4
        song = both(song, note(e, z, z + 1/8))
        z += 1/4
        song = both(song, note(c, z, z + 1/8))
        z += 1/8
        song = both(song, note(e, z, z + 1/8))
        z += 1/4
        song = both(song, note(g, z, z + 1/4))
        z += 1/2
        song = both(song, note(low_g, z, z + 1/4))
        return song

    def mario_at(octave):
        c = tri(octave * c_freq)
        e = tri(octave * e_freq)
        g = tri(octave * g_freq)
        low_g = tri(octave * g_freq / 2)
        return mario(c, e, g, low_g)
    play(mario_at(1/2))
    ```


