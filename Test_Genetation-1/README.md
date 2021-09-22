## 实现：测试数据再生成  

**参考文献**：

1. **Test Data Regeneration : Generating New Test Data from Existing Test Data**  
2. Search-based software test data generation: a survey

**实验对象链接**：https://github.com/QRXqrx/2021-AutomatedSoftwareTesting

**复现要点**：

- 理解文献1提出的测试数据再生成技术的核心思想。
- 在实验对象已给出测试代码的基础上生成更多的测试数据
- 不必拘泥于文中提到的登山法，也可以尝试其他的启发式算法，比如：遗传算法、退火算法等。

**工具/库链接**：

- 字节码插桩库ASM：https://asm.ow2.io/

- 测试生成工具Evosuite：https://github.com/EvoSuite/evosuite

  可以借助里面的一些API引导测试数据再生成

