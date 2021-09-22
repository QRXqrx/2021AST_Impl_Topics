## 实现：基于Soot的静态测试切片

**参考文献**：

1. **Test Case Purification for Improving Fault Localization**
2. Dynamic Program Slicing
3. **A Survivor’s Guide to Java Program Analysis with Soot**  

**Tips**：

- 测试切片的流程参考测试提纯的前两个部分：原子化和测试用例切片
- 原子化（Atomization）部分可以选用任意一种程序变换工具，如Javaparser、Spoon；
- Soot切片产生的结果是近似字节码的中间表示（例如Jimple），可以转换成字节码之后通过反编译得到源代码；
- 可以将Soot的jar包或源代码导入项目中使用；
- 切片的出发点为每个单元测试用例的Junit assert语句，围绕这些语句构建切片准则（Slicing Criterion）
- 检查切片结果的可运行性，最后得到的切片结果最好是可运行的java源代码

**工具/库链接**：

- Spoon：https://github.com/INRIA/spoon

- Javaparser: https://github.com/javaparser/javaparser

  可以从Maven中心库下载编译好的jar包

- Soot程序分析库：https://github.com/soot-oss/soot

- Java反编译工具：https://github.com/java-decompiler/jd-core

