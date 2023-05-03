# ABAQUS_intro

## 0.帮助文档

[达索官方网站帮助文件](https://help.3ds.com/2021/English/DSSIMULIA_Established/SIMULIA_Established_FrontmatterMap/sim-r-DSDocAbaqus.htm?contextscope=all&id=b15ba98a76ce4305b417ecdd54e5394d)

[6.7-2016 版本](http://130.149.89.49:2080/)

[6.14](http://130.149.89.49:2080/v6.14/index.html)

## 1.概述

### 1.1文件系统

**有限元要素：单元，网格，节点，材料，连接关系，求解设置，边界条件**

**1.数据库文件**

`.cae`: 格式文件，包含具体的建模信息。

`.odb`: 包含输出结果，变形后的结果。

**2.数据文件**

`.dat`: 记录数据，参数检查，单元质量检查等信息

**3.输入文件**

`.inp`: 包含有限元模型，即有限元的 7 个要素，不包含几何模型

`.pes`: 参数更改后重新的 `inp` 文件

### 1.2模块介绍

![](E:\FiniteElementAnalysis\img\04.jpg)

**零部件模块：**Part

**装配体模块：**Assembly，类似建模软件，同一个零部件可多次调用，装配约束

**草图模块：**二维形式

![](E:\FiniteElementAnalysis\img\05.jpg)

### 1.3分析流程

![](img\06.jpg)

## 2.建模

`ctrl+alt+左键`：动态观察

鼠标左键长按打开下拉列表：

![](img\07.jpg)

壳体抽取中面设置：

![](img\08.jpg)

## 3.网格划分

[hypermesh 指南](https://mp.weixin.qq.com/s?__biz=MzI2NzMyNjI2Nw==&mid=2247530124&idx=1&sn=71711d37bacab69d571c50fa348cd8b4&chksm=ea82b0d8ddf539ceb83943b21d0e58e56bbd4589d3e41ef15faff1f6b42bdd326cc94be45dbc&scene=27)

[HyperMesh2021 安装教程及软件下载](https://www.bilibili.com/read/cv16122305?from=search&spm_id_from=333.337.0.0)

<img src="img/09.jpg" style="zoom:80%;" />

六面体网格：

- 扫略网格
- 结构化网格
- 自由网格划分
- 自底向上的网格划分

### 3.1圆形平面网格划分

圆形区域网格划分：铜钱形。

<img src="img/10.jpg" style="zoom:80%;" />

橘黄色：当前网格划分不适用，解决方式：

- 模型切割
- 修改网格划分

### 3.2二合一的网格划分

**避免产生三角形网格**

由二变为一

![](img/11.jpg)

**三合一的网格划分**

![](img/12.jpg)

**混合划分**

$10 = 2*2 + 2*3$

### 3.3自底向上的网格划分

![](img/13.jpg)

## 4.Assembly 模块

Assembly 模块：

优势：同一个零部件可调用多次

独立实体：网格划分需针对每个零件进行

非独立实体：只需针对母体进行即可

![](img/14.jpg)

- 旋转平移

- 阵列

- 装配约束

- 共节点

  模型的布尔求和：材料冲突

  网格模型共节点：**注意容差的大小 < 最小网格尺寸 ;适用于材料不一致的情况**




## 5.线性静力学分析

### 5.1简介

**前提**

材料线性；结构小变形，小位移，小转动，刚度不变

**弹性力学的基本假设**

1.连续性假设

2.线弹性假设

3.均匀性假设

4.各项同性假设

5.小变形假设

**线性分析内容**

1.线性静力学分析

- 系统运动速度为 0
- 动态稳定分析，平动加速度+旋转加速度。该类情况下加速度的变化频率远小于结构系统自身的自振频率

2.线性动力学分析

### 5.2实体静力学分析流程

#### 1.工作路径的指定和文件保存

#### 2.前处理

**2.1几何模型的构建 (part)**

统一单位制：如 mm| Mpa | t

**2.3材料参数的定义 (property)**

- 创建材料：杨氏模量 E =2.1e5 Mpa 和泊松比 v = 0.3
- 创建截面
- 截面指派，完成指派后，颜色由灰白色变为绿色。

**2.4网格系统的构建(mesh)**

- 网格划分：绿色表示结构化网格划分

  - 建议第一次网格划分(系统默认的)，确定数量和质量

  - 网格的控制：数量和质量角度，加载的位置，进行模型切分等操作
  - 第二网格划分

- 单元类型

  - 六面体，默认 C3D8R

- 装配体操作(Assembly)，创建

#### 3.求解

**3.1求解器的设定(step)**

- 静力学分析，静力通用分析步
- 场输出，云图。contact: 接触
- 历史输出，变量-时间的历史曲线

**3.2连接关系的构建**

- 相互作用

**3.3边界条件的设定(load)**

- 载荷边界条件：载荷施加
- 位移边界条件：约束问题，如固端约束：Encastre
- 切换到 Job 设置

#### 4.后处理

**4.1位移**

**4.2应力**

**其他**

考虑重力：密度 $7.85×10^{-9} \textup{t/mm}^3$

## 6.壳体结构静力学分析

### 6.1流程

#### 1.工作路径设置和保存

#### 2.前处理

1.几何建模

2.材料参数的定义

- 创建材料
- 创建截面，部件名称-截面类型-材料类型-截面属性
- 截面指派，Middle surface 从中面向两侧配置

3.网格系统的构建

- 网格划分
- 单元类型：S4R，4 节点缩减积分的单元
- 装配

#### 3.求解

1.求解器设定

- 通用分析步
- 场输出，云图，应力和位移
- 历史输出

2.连接关系的构建，针对多个部件分析设置

3.边界条件的设定，位移边界条件以及荷载边界条件

#### 4.后处理

1.设定位移和应力的显示比例

![](img/16.jpg)

2.sneg(SENG=-1.0)：shell 的下侧表面

spos(SPOS=1.0)：上侧表面

或修改为包络形式：Envelope

#### 5.其他

**采用抽取中面的方式创建**

创建 solid 实体并抽取中面

## 7.非线性分析

### 7.1简介

**非线性行为：**

载荷引起结构刚度发生变化

典型的情况：金属材料的弹塑性 + 大变形问题(细长杆+长薄板) + 状态变化(接触+单元失效)

#### 非线性分类

**几何非线性**

**ABAQUS standard 默认假设小变形；ABAQUS explicit 默认假设大变形；**

- 结构的大变形/大旋转 (Large deflections or rotations)
- 双稳态分析，越跃失稳 (Snap through)
- 初始应力或者载荷刚度 (Initial stresses or load stiffening)

**材料非线性**

- 金属弹塑性：小应变-线弹性；大应变-塑性变形（不可逆）

- 超弹性材料：应力应变为非线性曲线（可恢复变形）

- 其他形式：材料属性可以为温度或其他领域的函数

**边界非线性**

边界条件的不连续，如接触

#### 自动增量步控制

存在于隐式分析中：牛顿-拉夫逊法

- 初始增量步：开始的第一个分量步大小，(默认为当前分析步大小，推荐为分析步时间的 5%-10%)
- 最小增量步/最大增量步：允许的最小/大增量步大小，默认 (1e-5) / 当前分析步大小
- 最大增量步数：默认 100
- 当增量步不收敛时，增量步大小缩小为当前增量步的 25%（默认只允许缩小 5 次）
- 迭代步数在 5 次以下，增量步将增大当前增量步的 50%。



### 7.2几何非线性分析

大变形分类：

- 刚体位移，运动产生，多体动力学分析
- 弹性变形，受力产生，弹塑性分析 | 超弹性分析

#### 1.前处理

- 几何模型的构建
- 材料参数的定义
  - 创建材料
  - 创建截面
  - 截面指派
  - 法向设定

- 网格构建
  - 装配体构建，**独立实体 (Independent)**
  - 网格划分，全局尺寸设置为 1
  - 单元类型，保持默认，B31

#### 2.求解

- 求解器的设定

  - 静力通用分析步

  ![](img/17.jpg)

- 输出云图的设定

  - 场输出，S,U
  - 历史输出

- 连接关系的构建

- 边界条件的设定

  - 位移边界条件
  - 载荷边界条件

#### 3.后处理

位移 ； 应力

### 7.3材料非线性

真实应变：
$$
\epsilon = \ln (\frac{l}{l_0})=\ln(1+\epsilon_{norm})
$$
材料非线性引起的几何非线性的变形较大，需打开非线性分析，同时避免不收敛。








