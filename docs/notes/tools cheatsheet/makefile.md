
!!! info "简单区分"
    + makefile 是一种文件格式，包含了构建项目的规则和指令，由Make工具解释执行。
    + GNU Make 是一个自动化构建工具，根据 makefile 文件中的规则执行编译、链接等操作。
    + CMake 是一个跨平台的构建工具，它不直接构建项目，而是根据 CMakeLists.txt 生成 Makefile 文件。 

- [ ] GNU Make
- [ ] CMake

## 概述
+ makefile 定义了一系列的编译规则，指定文件编译顺序、是否需要重新编译等信息。
+ 一旦写好，只需执行 make 命令即可自动编译整个工程。
    + make 是一个解释makefile的工具。

??? extra "编译和链接相关"
    对于C 和 C++ 程序，首先将源文件编译为中间代码文件，
    在Windows下为.obj文件，在Linux下为.o文件；
    然后将很多个中间代码文件链接为可执行文件，
    在Windows下为.exe文件，在Linux下为.out文件。

    !!! tip "编译"
        编译需要的是语法正确以及函数、变量的声明正确。
        而对于后者，需要向编译器提供头文件路径。

    !!! tip "链接"
        主要是链接函数和全局变量，链接器不在乎源文件，只在乎中间目标文件。
        当源文件太多，编译生成的目标文件也会很多，这时需要给中间目标文件打包，
        在 Windows 下为 .lib 文件，在 Linux 下为 .a 文件。

## makefile 规则概述
如下为一个简单的 makefile 文件：
```makefile
target ... : prerequisites ...
    recipe
    ...
    ...
```

+ target: 目标文件（可执行文件、中间目标文件等）
+ prerequisites: 生成该 target 所需要的文件和（或） target
+ recipe: 该 target 要执行的命令

简言之，`target` 这里的一或多个目标文件依赖于 `prerequisites` 中的文件，其生成规则定义在 `recipe` 中。

!!! warning "核心"

    `prerequisites` 中如果有一个以上的文件比 `target` 文件新，则 `recipe` 定义的命令就会被执行。

!!! example "eg"
    ```makefile
    edit : main.o insert.o search.o \ 
            delete.o update.o
        cc -o edit main.o insert.o search.o \
            delete.o update.o
    main.o : main.c defs.h
        cc -c main.c
    insert.o : insert.c defs.h
        cc -c insert.c
    search.o : search.c defs.h
        cc -c search.c
    delete.o : delete.c defs.h
        cc -c delete.c
    update.o : update.c defs.h
        cc -c update.c
    clean :
        rm edit main.o insert.o search.o \
            delete.o update.o
    ```

    !!! tip "解释"
        
        + `\` 只是换行符，为了方便阅读。
        + `edit` 为可执行文件（Windows 下即为 edit.exe），
        它依赖于 `main.o insert.o search.o delete.o update.o`。
        + 一系列的 `*.o`文件又依赖于 `*.c` 和 `defs.h`。
        + `clean` 并非文件名而是动作名，执行 `make clean` 时会删除所有生成的文件。
        + `cc-c` `cc-o` `rm` 都是shell中的命令，注意需要一个Tab键缩进。
    
    当我们执行 `make` 命令时，make 会自动找到当前目录下的 makefile/Makefile 文件，
    找到我们定义的首个目标文件（这里是 `edit`），然后根据依赖关系逐个生成文件（有点像堆栈）：
        
    1. 如果有 `edit` 文件且比其依赖的文件新，则将其作为最终的目标文件
    2. 如果没有 `edit` 文件或者 `edit` 文件比其依赖的文件旧，则根据定义好的规则生成 `edit` 文件
    3. 如果 `edit` 依赖的`*.o`文件不存在，则会继续向下查找、生成对应的 `*.o` 文件

+ 考虑到某些文件可能会重复出现，为了方便地复用和维护，我们可以使用变量（类似宏定义）来简化 makefile 文件；
+ 同时，GNU Make 可以自动推导一些文件依赖关系，例如，当`make`看到一个`*.o`文件，它会自动找到对应的`*.c`文件，这部分我们无需手写：
+ Makefile 中可以通过 `include` 指令包含其他文件，这样可以将一些公共的规则放在一个文件中，然后在其他文件中引用。
```makefile
include common.make *.mk # 包含common.make文件和所有以.mk结尾的文件
obj = main.o insert.o search.o delete.o update.o
edit : $(obj)
    cc -o edit $(obj)
main.o : defs.h
insert.o : defs.h
search.o : defs.h
delete.o : defs.h
update.o : defs.h
.PHONY : clean  # 声明clean为伪目标
clean :
    -rm edit $(obj)
```
??? tip "tips"
    
    + 如果 `include` 出错但不希望终止 make 的执行，可以在前面加`-`
    + 建议命名为Makefile，这样在排序上较为靠近其他重要文件例如README
    + 建议不要定义和使用`MAKEFILES`环境变量。

??? extra "GNU make 工作方式"
    1. 读入所有的makefile
    2. 读入被include的其他文件
    3. 初始化文件中的变量
    4. 推导隐式规则，并分析所有规则
    5. 为所有的目标文件创建依赖关系链
    6. 根据依赖关系，决定哪些目标要重新生成
    7. 执行生成命令

## 规则书写
> 规则包括两部分：目标和依赖关系

> Makefile 中应该只有一个最终目标，第一条规则中的第一个目标将被作为默认的最终目标。

!!! note "格式"
    
    === "1"
        ```makefile
        target: prerequisites
            command # 一个Tab的缩进
        ```

    === "2"
        ```makefile
        target: prerequisites ; command # 分号分隔
        ```

### 通配符
make 支持三种通配符：
+ `*` 匹配任意长度的字符串
+ `?` 匹配单个字符
+ `[...]` 匹配括号中的任意一个字符

```makefile
# 匹配所有以.c结尾的文件
%.o : %.c
    cc -c $< -o $@
```
