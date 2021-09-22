## 复现：Python混合执行工具 PyExZ3

**参考文献**：

1. Symbolic Execution for Software Testing: Three Decades Later 

**复现要点**：

- 参考文献中对符号执行的定义，参照PyEXZ3仓库中的代码和资料完成对目标程序的符号状态分析。利用graphviz将分析的结果打印输出为PDF图。
- 使用约束求解器（如Z3、CVC4等），求解上一步得到的符号状态，保存求解得到的测试数据。

**实验对象链接**：https://github.com/QRXqrx/2021-AutomatedSoftwareTesting

**工具/库链接**：

- PyExZ3开源库：https://github.com/thomasjball/PyExZ3
- 约束求解器Z3：https://github.com/Z3Prover/z3
- 图生成工具Graphviz：https://graphviz.org/

