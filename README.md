# miniFOC
FOC,STM32F4,step-by-step,noob

# 无刷电机怎么动？

[知乎传送门](https://www.zhihu.com/search?q=%E6%97%A0%E5%88%B7%E7%94%B5%E6%9C%BA%E5%8E%9F%E7%90%86&utm_content=search_suggestion&type=content
)
通俗讲就是产生个磁场去拉转子。
<div align=center><img src="https://github.com/CXianRen/miniFOC/blob/master/img/BLDC原理.gif"/></div>
<div align=center><img src="https://github.com/CXianRen/miniFOC/blob/master/img/q轴磁场.png"/></div>

上面演示都为内转子电机，而常用的都是外转子(便宜，┑(￣Д ￣)┍)。原理上都是一样的，反过来就好。

## 外转子电机
### 极对数 = 极对数=极数/2。 
    是数有多少个磁铁，或者可以利用反电动势测出：STM32 官方给的方法（在Motor Profiler 5.4.1中）
    给任意亮相通恒流，然后转一圈，看示波器/电流源的电压跳变次数（我没试成，小电机，反电动势太小了，不好测，也不好转）
### 电角度 
    TODO

# SVPWM
## 怎么动最好？
    上面讲了，需要产生个拉动转子的磁场，那么当然是这个磁场和转子磁场正交的时候力矩最大啦,那么旋转起来后，
    这磁场的轨迹就是个圆了（不知道这样讲对不对，有可能大概不确定是错的）。然后我们要控制磁场就要控制电流，
    电流不好搞，我们就搞电压呗。并且，这个空间电压还要稳定，就有如图：

<div align=center><img src="https://github.com/CXianRen/miniFOC/blob/master/img/Space_vectors.gif"/></div>  
    紫色为合成的电压，其他三个色是相位差120°的相电压
    那么只要在abc三个相中输入 3个相位差120°的正弦波就能搞出这个玩意了。
    但是emmmm，有这么简单就好了

## 驱动电路（逆变器）
    我们的是数字控制嘛，只能怼PWM出来。驱动电路如图：
<div align=center><img src="https://github.com/CXianRen/miniFOC/blob/master/img/驱动器.png"/></div>
    用驱动芯片会比较好，自己搭的话要怼6路PWM，还要控制死区，比较麻烦。原理一样的。
    驱动有3个半桥组成，显然，每个半桥在任意时刻只有上半桥开或下半桥开。这样子3个半桥一共有2^3=8个状态。
    上半桥全开和下半桥全闭合是没有输出的状态。那么就能够实现8个向量（2个0向量）的电压。
    
[推导传送门](https://blog.csdn.net/qlexcel/article/details/74787619#comments)

    有了8个基础向量我们就能够控制基础向量的长度（工作时间）去合成目标向量了。
    对于扇区条件和时间的推导看这里：
[扇区条件推导](https://blog.csdn.net/michaelf/article/details/94013805)

    总结一下上面两个链接的步骤：
    Step1： 计算U1,U2,U3
    Step2:  根据U1,U2,U3 确认所在的扇区
    Step3： 根据所在的扇区计算两个基本向量的工作时间。
    Step4： 然后根据工作时间计算 PWM比较器的值。

    对于图下图的解读：
<div align=center><img src="https://github.com/CXianRen/miniFOC/blob/master/img/发波图.JPG"/></div>
    这个倒三角是啥：是PWM的计数器的值，中间蓝色虚线为0，详见下面的 STM32 PWM 中心对齐 模式
    数字1，2，3处就是算出来，PWM该跳变为高的时间点。X轴为T。
    
## 验证SVPWM的正确性
    TODO 示波器拉看那个高电平持续最段的相的高电平时间应该等于高电平持续时间最大的相的两个高电平的间隔时间