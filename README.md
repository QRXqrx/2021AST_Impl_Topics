## 方向一：PA for AI

1. 复现：Dynamic Slicing for Deep Neural Networks [FSE’20]
2. 复现：Symbolic execution for importance analysis and adversarial generation in neural networks [ISSRE’19] （重点是实现DNN转换成其他程序的部分）



## 方向二：Test Generation

1. Regeneration的实现与拓展 [STVR’12]
2. Evosuite与Evosuite-Seeding [FSE’11-Demo + ICST’10]
3. 实现基于ASM实现的Java混合执行工具，关键：复现其中使用ASM插桩实现布尔表达式生成、收集的部分。TesMa and CATG: Automated Test Generation Tools for Models of Enterprise Applications [ICSE’15 Demo]
4. 实现基于Soot实现的Java混合执行工具，关键：复现其中使用Soot插桩实现布尔表达式生成、收集的部分。
5. 复现PyExZ3：面向python的符号执行
6. 复现：Directed Test Suite Augmentation:Techniques and Tradeoffs [FSE’10]



## 方向三：Oracle Problem

1. 复现工具：SODS。Supporting Oracle Construction via Static Analysis [ASE’16]
2. 复现工具：DODONA。Dodona: Automated oracle data set selection [ISSTA’14]
3. 复现工具：MAODS。 Automated oracle creation support, or: How I learned to stop worrying about fault propagation and love mutation testing [ICSE’12]



## 方向四：Program Slicing

#### 复现

1. Test Case Purification for Improving Fault Localization [FSE’14]
2. Log-Based Slicing for System-Level Test Cases [ISSTA'21]

#### 实现

1. 使用WALA程序分析库，参考测试提纯技术，实现基于断言的测试切片
2. 使用Soot程序分析库，参考测试提纯技术，实现基于断言的测试切片



## 方向五：Test Selection

1. 复现：STARTS。STARTS: STAtic Regression Test Selection [ASE’17-Demo]
2. 复现：Ekstazi。Ekstazi: Lightweight Test Selection [ICSE’15-Demo]
3. 复现：Hybrid Regression Test Selection [ICSE’18]



## 方向六：Specification Mining

1. 复现：DSM。Deep Specification Mining [ISSTA'18]



## 可交付的工作

- 独立完成的工具代码
- 工具的使用日志。主要记录：
  1. 使用过程中遇见的一些问题，尤其是官方文档没有提及、或者不够详细的地方；
  2. 一些可行的配置，如：环境准备、项目构建
- 实验结果，如：
  - 符号执行构建得到的PDF图
  - 测试生成产生的测试数据
  - 实验数据的统计与展示
  - 实验数据的分析