## 复现：静态测试选择工具STARTS  

**参考文献**：

1. **STARTS: STAtic Regression Test Selection**    
2. An Extensive Study of Static Regression Test Selection in Modern Software Evolution    

**复现要点**：

- 实现功能、保证可用性即可，执行效率其次；
- 成品形式不限（不一定非得实现成插件）；
- 实现同类功能的库可以替换，如：类似的静态分析库、图操作库
- 注意不同粒度测试选择在实现方面的差异

**工具/库链接**：

- WALA程序分析库：https://github.com/wala/WALA

  可以从Maven中心库下载编译好的jar包

- Soot程序分析库：https://github.com/soot-oss/soot

