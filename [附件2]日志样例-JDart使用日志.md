# JDart
JDart是jpf的concolic拓展。JDart与SPF维护符号执行路径的方式不同，JDart将符号执行路径保存在树状结构中；而SPF则保存成一条单一路径


## JDart Config

- ``AnalysisConfig``：符号域配置和搜索配置
- ``ConcolicConfig``：符号执行的终止；混合执行方法读取与定义。和``ConcolicMethodConfig``联动确定，``concolic.method``指代的具体方法

#### 混合执行配置
- 为需要每个需要混合执行的方法指定一个唯一名称。配置前缀：``concolic.method.<cm> = <fully-qualified-name>``
- 指定被主要分析的方法：``concolic.method = <cm>``
- 符号化参数：在指定混合执行的参数列表时写做``methodname(<sym_name>:<type>)``

#### 分析配置
- 配置前缀：``jdart.configs.<cm>``，``<cm>``是前面指定的符号执行方法的唯一名称

#### 搜索配置
- 搜索（Exploration）配置的基本形式：``<prefix>.exploration``，例如``jdart.configs.sample.exploration``
- ``suspend``和``resume``可以改变搜索的行为
    - 匹配suspend模式的方法，搜索会被悬挂（suspended）
    - 匹配resume模式的方法，搜索会被复原（resumed）


- 例子
```text
# don't explore initially
jdart.configs.sample.exploration.initial = false 

# Resume exploration in the method sample.Foo.foo(int,boolean)
jdart.configs.sample.exploration.resume = sample.Foo.foo(int,boolean)

# .. but suspend it again in any method with name bar (regardless of arguments)# and the method baz().
jdart.configs.sample.exploration.resume =\
        sample.Foo.bar(*);\
        sample.Foo.baz()
```
我觉得搜索相关的配置还挺重要的，看起来和引导整个搜索过程有关。比如什么时候暂停搜索？什么时候flip并恢复搜索？可能和这些配置有关。

#### 符号域配置（Field）
- 基本形式：``<prefix>.symbolic``，例如``jdart.configs.sample.symbolic``
- 将object视作符号，意味着该对象的所有成员变量都会被视作符号

关于Symbolic Field的相关配置在下列几个类中：
- ``SymbolicObjectsContext``：判定符号值的类型（引用、初级数据类型等等），传递给相应的方法进行处理（``parseObject``、``parsePrimitiveField``等）。目前观察到的标记方式有两种：（1）通过在application级别的jpf文件中，通过<prefix>.symbolic.include添加。目前观察到的，解析这种配置的方法为``AnalysisConfig.predicateFromString``，该方法会返回一些函数式接口（``Predicate``），``SymbolicObjectsContext``内部会通过该方法和解析得到的predicate实例进行判定，体现为``SymbolicObjectsContext.makeSymbolic``；（2）通过注解进行标记，体现为``SymbolicObjectsContext.forceSymbolic``。根据log的情况，目前可以知道引用类型的Field值默认使用**具体值**（非符号值）。
- ``AnalysisConfig``：分析配置类。针对jpf Application级配置中和符号域、搜索相关的一些配置进行解析
- ``ConcolicMethodExplorer``：根据一个``AnalysisConfig``的实例``anaConfig``来创建``SymbolicObjectsContext``的实例。


#### 其他配置
- 前缀统一：``jdart.configs.sample``
- max_depth：指定约束树的最大深度
- max_alt_depth：negation的最大深度。jdart通过求解执行某一条件，然后再翻转走到另一子树。那么这条配置就可以限制另一子树的深度
- ``constraints(;``指定一系列用于限定搜索过程的约束条件。已有的测试数据也许可以通过形成constraint的方式加入到搜索过程当中
> constraints (;-separated list of strings): further constraints to be imposed on the exploration. This is an expression over the overall set of symbolic variables, which will be enforced for every constraint solving step during concolic execution.
> 约束写在“=”之后。单个约束用()括起来、多个约束之间用“;”隔开。


##  JDART: A Dynamic Symbolic Analysis Framework
JDart的两个关键组件：**Dynamic Exploration** & **Constraints Constructor** (interface with constraint solver)
Two main components: Executor and Explorer
- *Executor*：执行程序，记录与数据值有关的符号约束。**分析程序生成符号约束**；
- *Explorer*：实现搜索策略，完成符号编码（这个编码和遗传算法的编码能不能对应上？）。**将符号约束组织成约束树，决定下一步探索哪些路径**

额外功能：Junit测试用例生成、方法总结（Summarization）。JDart是扩拓展可配置的，所有的模块也都是可以等位替换的

应用到JDart的工具：PSYCO & JPF-Doop

**疑问**：Dynamic Symbolic Analysis的结果是约束树还是求解完毕的一众解？

#### JDart特征
- 先生成约束树。尚未完成求解的路径标记为未知，求解完毕的未知标记执行结果，全部求解完成之后才会终止；
- 默认只有传入的参数是符号值。可拓展成员变量、静态变量和返回值为符号值。
- 支持特定区域的搜索终止或重启（suspending/resuming exploration based on  the method level description）
- 提供可拓展的TerminationStrategy，可以按照使用者的意愿决定什么时候终止


#### JDart配置
![9853a38cbda33d72c40894b37c4af61d.png](en-resource://database/756:1)
constraints和指定的concolic方法中指定的符号化参数有关系

#### 与其他工具对比和结合
- Randoop：随机从一个数值池中选择输入，胜在测试的宽度，短在测试的深度；JDart可以生成很长的代码路径，两者集合产生了JPF-Doop
- 约束求解器：CVC4, Yices, Z3, Coral
- 符号执行工具:：SAGE, JCute, CATG, LCTG, SPF, JFuzz
- 插桩工具：Soot、ASM
- 估计可以通过分析字节码在后面插上路径约束，通过这种方式收集到路径约束后，修改成求解器可以求解的形式，最后求解得到结果（有时间实现一下？）
![4b4bb5c567b34b7ffee60f577e8adae9.png](en-resource://database/758:1)


## JDart Code
#### gov.nasa.jpf.jdart.ConcolicExplorer
**The facade to jdart, the heart of the concolic execution framework**



## 运行例子
在针对待测方法完成JDart配置后可以利用`RunJPF.jar`进行concolic执行。在执行的最后会输出统计信息，包括
- 运行时长（Profiling）：JDart的运行时间、JPF启动时间等
符号执行树：以树的形式输出程序执行到的路径；
- 路径统计：总路径（total）、可行路径（OK）、不可行路径（ERROR）、不确定路径（DONT_KNOW）。符号执行树会记录一个路径条件PC，一般是源代码中谓词的取反（因为字节码中条件语句的实现是Label + goto，else会直接跟在判定后面，如果判定为真就直接跳走到标志then部分的Label处）。OK是没有错误；ERROR是发现报错；DONT_KNOW可能与方法调用有关（feature/simple/test_zoo.jpf）
 - Valuation统计：将符号路径转化成具体值输出，罗列出一组测试用例（Test Input）
   
#### feature/simple/test_zoo.jpf
这个例子关注JDart在方法调用上的处理方式

```text
[INFO] Profiling:
JDART-run: 415 ms [0 s]
JPF-boot: 169 ms [0 s]

[INFO] Completed Analyses: 1
Completed Analyses: 1
[INFO]
[INFO] Analyses for method features.simple.Input.zoo(i:int,j:short,f:float)
[INFO] ==================================
[INFO] -('i' <= 73)
  |-[+]-('i' == 12)
  |      |-[+]_/OK: [ return:='j',  ]
  |      +-[-]-('i' == 42)
  |            |-[+]_/OK: [ return:='j',  ]
  |            +-[-]_/OK: [ return:='j',  ]
  +-[-]-(('f' + (float)(sint32)'j') <= 256.0)
        |-[+]_/DONT_KNOW
        +-[-]_/OK: [ return:='j',  ]

[INFO] ----Constraints Tree Statistics---
[INFO] # paths (total): 5
[INFO] # OK paths: 4
[INFO] # ERROR paths: 0
[INFO] # DONT_KNOW paths: 1
[INFO]
[INFO] -------Valuation Statistics-------
[INFO] # of valuations (OK+ERR): 4
[INFO]
[INFO] java.lang.Float:f=257.0, java.lang.Integer:i=12, java.lang.Short:j=32767,
[INFO] java.lang.Float:f=257.0, java.lang.Integer:i=42, java.lang.Short:j=32767,
[INFO] java.lang.Float:f=1.414, java.lang.Integer:i=1, java.lang.Short:j=2,
[INFO] java.lang.Float:f=257.0, java.lang.Integer:i=76, java.lang.Short:j=32767,
[INFO] --------------------------------
```
在符号执行树中，``+-[-]-(('f' + (float)(sint32)'j') <= 256.0)``对应程序中的一条方法调用zoo_sub。在zoo_sub方法中，第一条判断语句为：``if (f + j > 256)``。方法调用导致了后续的``DONT_KNOW``

#### features/arrays/test_m1.jpf
从文献中得知，JDart在对数组类型的支持方面做的并不好。运行这个例子的时候，为了确定main方法在JDart运行中的作用，我将该例子对应的测试类的main方法注释掉。在SPF和JPF中，开发者必须为待测程序定制一个包含main方法作为入口的TestDriver，不知道在JDart中如何？
- 没有main方法，JDart混合执行同样不会开始，因此在使用JDart时也需要提供一个TestDriver作为测试入口；
- 注释掉m1方法（该配置下的目标方法，由concolic.method指定），JDart也没有运行起来。可知JDart和jpf、spf一样，只能显示执行到的部分的内容（现在要想的是如何拿到已有数据对应的测试执行路径）

#### features/functions/test_foo_replay.jpf
- 关注配置项：<prefix>.values，看起来貌似是是为某个concolic方法指定可选参数的。JDart会从给定参数里面选择一些作为符号执行的起点（貌似），下面两个代码块分别是配置和输出：
```text
@include=./example.jpf

concolic.method=foo

concolic.method.foo.values=1.0,0.0;\  
    1.0,1.0;\ 
    0.0,1.0;\ 
    0.0,0.0;\  
    0.5,0.5
```
**输出**
```text
[FINER] Adding constraint (d1 > 0.0 && d1 < 3.0 && d2 > 0.0 && d2 < 3.0)
[FINEST] Producer.perturb(): features.functions.Input.foo(DD)V
[FINEST] ConcolicExplorer.newPath()
[FINER] Finding new valuation
[FINER] Found: SAT : d1:=0.5,d2:=0.5
[FINEST] Producer.perturb(): features.functions.Input.foo(DD)V
[FINEST] ConcolicExplorer.newPath()
[FINEST] Reexecuting with valuation d1:=0.5,d2:=0.5
[FINER] Finding new valuation
[FINER] Found: SAT : d1:=0.5,d2:=0.25
[INFO] Predicted inconclusive divergence
[FINER] NOT attempting execution
[FINER] Finding new valuation
[FINER] Found: SAT : d1:=0.5,d2:=0.25
[INFO] Predicted inconclusive divergence
[FINER] NOT attempting execution
[FINEST] Producer.perturb(): features.functions.Input.foo(DD)V
[FINEST] ConcolicExplorer.newPath()
[FINEST] Reexecuting with valuation d1:=1.0,d2:=0.0
[FINEST] Producer.perturb(): features.functions.Input.foo(DD)V
[FINEST] ConcolicExplorer.newPath()
[FINEST] Reexecuting with valuation d1:=1.0,d2:=1.0
[FINEST] Producer.perturb(): features.functions.Input.foo(DD)V
[FINEST] ConcolicExplorer.newPath()
[FINEST] Reexecuting with valuation d1:=0.0,d2:=1.0
[FINEST] Producer.perturb(): features.functions.Input.foo(DD)V
[FINEST] ConcolicExplorer.newPath()
[FINEST] Reexecuting with valuation d1:=0.0,d2:=0.0
[FINEST] Producer.perturb(): features.functions.Input.foo(DD)V
[FINEST] ConcolicExplorer.newPath()
[FINEST] Reexecuting with valuation d1:=0.5,d2:=0.5
[FINER] Completed analysis: gov.nasa.jpf.jdart.ConcolicMethodExplorer@5e82df6a
[FINE] Constraints Tree had depth 3
[FINEST] ConcolicListener.searchFinished(): -1
```
不能确定的是，这些值的意义是赋予搜索一个开始的点还是有什么说形成了一个数值水池，可以随时取用？

#### features/globals/test_foo.jpf
Z3报错，前面也见过类似的错误
```text
Finding new valuation
[FINER] Found: SAT : i:=200001
[FINEST] Producer.perturb(): features.globals.Input.foo(I)V
[FINEST] ConcolicExplorer.newPath()
[FINEST] Reexecuting with valuation this.k[2]:=0,this.k[1]:=0,this.k[0]:=0,i:=200001,this.b:=false,this.state:=0
[FINER] Finding new valuation
ASSERTION VIOLATION
File: ..\..\src\api\api_ast.cpp
Line: 906
UNREACHABLE CODE WAS REACHED.
Z3 4.8.9.0
Please file an issue with this message and more detail about how you encountered it at https://github.com/Z3Prover/z3/issues/new**
```

#### features/simple/using.jpf
里面给出了很多jpf相关的定义，其中指定listener貌似能够获取路径

## 关于Constraints
- 给定constraints意味着限定了搜索的区域。JDart貌似会从给定的test input开始进行混合执行，之后向已有的约束方向行进。 例子：myexample2/simple/Main1


## 自定义JDart拓展
### 实现可配置的单路径约束收集
根据观察，JDart会以TestDriver中传入的数据作为起点，然后按照一定的策略进行搜索（search for valuation）。当传入的测试数据可行时，最终找到的所有valuation往往有一组是Test Driver中传入的数据。因此，只需要在第一次求解完毕之后收集并返回这条路径就可以实现我们的需求。
#### 新增数据类型
定义数据类，用于存放收集到的路径约束。最初的实现中将这个数据类型命名为``edu.nju.pa.SinglePath``
#### 增加配置
- 单路径收集开关：``jdart.single.path.mode``
- 单路径收集输出开关：``jdart.single.path.mode.print``
- 输出文件名，默认为xml格式、输出到jpf所在的文件夹下：``jdart.single.path.print.name``
#### 修改JDart原生类
根据观察结果，在运行时的调用顺序一般为：
- ConcolicPertubator ：初始化ChoiceGenerator
- InternalConstranitTree ：创建内部约束树
- ConcolicMethodExplorer ：为目标方法创建混合执行
- SymbolicObjectsContext ：将于当前混合执行相关的值符号化
- ConcolicPertubator ：启动混合执行（调用ConcolicExplorer） 
- ConcolicExplorer ：混合执行的过程核心，但实际上只给出了混合执行核心逻辑框架，具体的执行功能委托出去给ConcolicMethodExplorer、ConcolicListener等
- ConcolicMethodExplorer ：调度某个方法的混合执行过程，其中hasMoreChoices方法是干涉符号执行过程的核心。在initValuation和nextValuation之间加入判定：当处于SingleMode的状态下时，在初始化搜索之后直接停止
- ConcolicListener ：是对JPF Listener拓展的实现，主要功能是帮助用户介入VM执行过程，使我们能够追踪控制一系列VM相关操作比如：方法进入、指令执行等。
- InternalConstranitTree ：构建内部约束树。其中的核心逻辑是在一次搜索中为每一个符号值确定一个Valuation，确定这个valuation是否可解、求解的结果如何（OK/ERROR）
- Note that ：在SingleMode关闭的情况下，ConcolicMethodExplorer会在initValualtion之后继续寻找nextValuation，这一过程汇总会调用内部约束树的findNext()。前面三步（ConcolicListener一般和涉及到的方法调用有关）可能会反复多次直到搜索结束；如果某一次ConcolicMethodExplorer执行结束了，他会紧接着调用InternalConstraintTree.finish方法
- ConcolicListener ：监控/控制VM调度
- ConstraintTree ：将内部约束树转换成便于输出的最终格式

修改主要涉及的类：JDart、ConcolicExplorer、ConcolicMethodExplorer、InternalConstraintTree，其中：
- JDart：（被配制成shell时）JDart定义了混合执行的整体流程。对应新增的配置，添加配置解析和相应的操作；
- ConcolicExplorer：修改成员变量，添加get方法，使SinglePath（存放约束条件的数据结构）能够传入；
- ConcolicMethodExplorer：增加SinglePath成员变量、相应修改构造器；修改hasMoreChoice方法，使其在SingleMode打开时，在initValuation结束后直接返回FALSE；
- InternalConstraintTree：类似地修改成员变量和构造器；修改decision方法，追踪并保存求解得出节点的约束和深度

### 实现JDart多次执行
修改目标：
- 收集test driver中的每个被调用方法执行到的符号路径
- 收集test driver某一次方法调用执行到的符号路径