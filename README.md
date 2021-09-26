## 说明

NJU自动化测试2021源码分析方向题目仓库。作业的评分细则见**附件1**

一些题目给出了实验对象，仓库链接：https://github.com/QRXqrx/2021-AutomatedSoftwareTesting

#### 仓库结构

- 每个题目一个文件夹，结合方向和编号进行查找
- 每个文件夹给出部分参考文献，和一些说明。注意:warning:：所给说明和参考文献可能不完整，一些实现过程中所需的资料（论文、工具、使用说明和数据集）需要同学们自行检索
- 用**黑体**标出的参考文献为该题目主要参考

#### 可交付的成果

- 工具实现源代码，上传到github，交作业时提供仓库链接

  - 注：建议在开发过程中设置仓库为私有；提交作业后再公开仓库

- 工具的开发日志。记录内容可以是：
  1. 使用某些工具的过程，特别是官方文档缺失或者不够详细的部分；
  2. 使用工具或复现中遇见的一些问题和对应的解决方案；
  3. 一些可行的配置，如：环境准备、项目构建。
  
  使用Markdown编写，命名成LOG.md，放在作业代码仓库根目录下。

  日志样例见**附件2**，内容和形式仅供参考，大家可以写出自己的风格，比如写成小故事什么的。。。
  
- 工具的设计文档，包括

  1. 工具的介绍；
  2. 工具的模块构成；
  3. 工具的使用说明。

  使用Markdown编写，并名称README，放在作业代码仓库根目录下。

- 实验结果，包括但不限于：
  - 符号执行构建得到的PDF图
  - 测试生成产生的测试数据
  - 实验数据的统计（各种统计图表）
  
- 演示视频：录屏工具的操作和运行情况并提交

- 工具汇报PPT：上述产品的要点汇总，包括

  1. 工具背后方法（主要参考文献）的理解
  2. 实现过程中的难点、要点
  3. 实验结果的展示、分析



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
7. 复现：ConTesa: Directed Test Suite Augmentation for Concurrent Software  [TSE'18]
8. 复现：Testing with Fewer Resources: An Adaptive Approach to Performance-Aware Test Case Generation  [TSE'19]
9. 复现：Efficient Test Generation Guided by Field Coverage Criteria [ASE'19]



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



## 方向七：AI for SE

1. 复现CLCDSA。CLCDSA: Cross Language Code Clone Detection using Syntactical Features and API Documentation  [ASE'19] （Code Representation）
2. 复现WEBQA。Web Question Answering with Neurosymbolic Program Synthesis  (Code Representation, Program Synthesis) [PLDI'21]
3. 复现NFRE 。A Lightweight Framework for Function Name Reassignment Based on Large-Scale Stripped Binaries  (Code Representation) [ISSTA'21]



## 方向八：Program Repair

1. 复现CPR。Concolic Program Repair [PLDI'21]



## 方向九：Regression Testing

1. 复现：Dependent-Test-Aware Regression Testing Techniques [ISSTA'20]
2. 复现：Differential Regression Testing for REST APIs [ISSTA'20]
3. 复现：Continuous Test Suite Failure Prediction  [ISSTA'21]



## 方向十：Test Reduction

1. 复现：An Empirical Study of JUnit Test-Suite Reduction [ISSRE'11]



## 难度参考

见**附件3**



## 联系方式

- qrx@smail.nju.edu.cn

