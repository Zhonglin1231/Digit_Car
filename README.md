文档网址：<a>https://docs.google.com/document/d/1b4HSeOz6ZN32ci0nwz9XZIQ9KGd4uulIEMhqYx1wtA4/edit </a>  
阿里云数据库：<a>https://dms.aliyun.com/new#shared=fdd970ff-18cd-40a8-9521-31b915c497d9</a>  
时间顺序数据：<a>https://we.tl/t-MpnuGY5MDB</a>

需要做的事项：
1. 理解每项数据对应的意义（共35列）                                 ???给数据重新排序???
2. 导入pandas库，使用pandas.dataframe进行数据预处理，
3. 挑选使用什么深度学习模型，可应用的库有：pytorch，skorch，rapids（需要gpu）
4. 数据可视化 e.g. 可以利用PowerBI等工具
5. 数据库已搭好，有需要可以找我进行数据提供嘿嘿
6. 将选出数据进行建模，计算精确剩余里程

每人的任务：
* 钟林：
  * 数据提供
    * 将数据通过车辆型号和**时间**排序 ✔
  * 数据可视化
  * 其他
* 李俊喆：
  * 你想干啥
* 王启帆：
  * 你想干啥
* 历海鹰：
  * 你想干啥

soc: 剩余百分比电量（不准的，因为电量会衰减）

**剩余里程计算**
1. 使用SOC估算剩余里程
   * SOC (State of Charge) 表示电池的剩余电量百分比。结合电池的总电量（power_amount），可以估算剩余的电量，从而计算出剩余里程。
   * SoC计算：
       * 方法一：通过积分 （Coulomb Counting Method）
           * ![image](https://github.com/user-attachments/assets/bcbab659-1dfa-499c-bccc-cd8a53f5d769)

       * 方法二：通过与电压的关系预测
           * ![image](https://github.com/user-attachments/assets/4eaeca6b-8eb5-4501-885c-9b0bd185619c)

2. 假设电池电量与续驶里程之间的线性关系
   * 我们可以假设在特定工况下，电池电量与续驶里程之间存在线性关系。通过已知的总电量和工况续驶里程，可以推算出单位电量对应的续驶里程。
3. 计算公式
   * 剩余里程（公里） = (SOC / 100) * 电池总电量 (Kw.h) * 单位电量续驶里程 (公里/Kw.h)

**实施步骤**
1. 提取相关数据：从数据中提取SOC、power_amount和drive_range_gongkuang(Km)。
2. 计算单位电量续驶里程：单位电量续驶里程 = 工况续驶里程 / 电池总电量。
3. 估算剩余里程：使用上述公式进行计算。
4. 现在我们可以开始进行这些计算：

    * 提取SOC、power_amount和drive_range_gongkuang(Km)。
    * 计算单位电量续驶里程。
    * 估算剩余里程。
我们先查看现有数据中是否有SOC相关字段，并进行计算。

*计算步骤*
1. 单位电量续驶里程计算：通过工况续驶里程 / 电池总电量得到每千瓦时电量对应的续驶里程。
2. 估算剩余里程：通过(SOC / 100) * 电池总电量 * 单位电量续驶里程得到剩余里程。
结果
3. 每辆车的估算剩余里程已经显示在表格中。通过这些数据，可以了解在不同SOC水平下车辆的剩余行驶里程。

*下一步*
* 如果你有实际的SOC数据，可以将这些替换掉假设的SOC值来进行更精确的计算。此外，你还可以考虑引入更多的特征，如环境因素、车辆状态等，来进一步提升模型的准确性。

训练模型寻找下面参数之间的逻辑：\n
Qnorm(总电量)随t(时间)的变化\n
V(总电压)和SOC之间的关系
