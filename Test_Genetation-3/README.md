## 实现：基于ASM的Java符号执行工具  

**参考文献**：

1. TesMa and CATG: Automated Test Generation Tools for Models of Enterprise Applications    
2. Symbolic Execution for Software Testing: Three Decades Later  

**复现要点**：

- 参考文献中对符号执行的定义，利用ASM程序分析库完成对目标程序的符号状态分析，并利用graphviz将得到的路径约束图保存成PDF图的形式。ASM可以从maven中心库中下载编译好的jar包；
- 使用约束求解器（如Z3、CVC4等），求解上一步得到的符号状态，保存求解得到的测试数据。

**工具/库链接**：

- 字节码插桩库ASM：https://asm.ow2.io/
- CATG开源库：https://github.com/ksen007/janala2/
- 图生成工具Graphviz：https://graphviz.org/

