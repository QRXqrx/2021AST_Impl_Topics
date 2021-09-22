## 实现：基于Soot的Java符号执行工具  

**参考文献**：

1. **Symbolic Execution for Software Testing: Three Decades Later** 
2. CUTE and jCUTE: Concolic Unit Testing and Explicit Path Model-Checking Tools   

**复现要点**：

- 参考文献中对符号执行的定义，利用Soot程序分析库完成对目标程序的符号状态分析，并利用graphviz将得到的路径约束图保存成PDF图的形式。Soot可以从maven中心库中下载编译好的jar包；
- 使用约束求解器（如Z3、CVC4等），求解上一步得到的符号状态，保存求解得到的测试数据。

**实验对象链接**：https://github.com/QRXqrx/2021-AutomatedSoftwareTesting

**工具/库链接**：

- 程序分析库Soot：https://github.com/soot-oss/soot
- 约束求解器Z3：https://github.com/Z3Prover/z3
- 图生成工具Graphviz：https://graphviz.org/
- 利用Soot进行插桩的混合执行工具Acteve，支持Z3：https://code.google.com/archive/p/acteve/
- 利用Soot进行插桩的混合执行工具LimeTB，支持Yices和Boolector：http://www.tcs.hut.fi/Software/lime/
- 利用Soot进行插桩的混合执行工具JCute，支持ip_solve：http://osl.cs.illinois.edu/software/jcute/

