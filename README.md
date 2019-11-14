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

# SVPWM是什么？
## 怎么动最好？
    上面讲了，需要产生个拉动转子的磁场，那么当然是这个磁场和转子磁场正交的时候力矩最大啦,那么旋转起来后，这磁场的轨迹就是个圆了（不知道这样讲对不对，有可能大概不确定是错的）。然后我们要控制磁场就要控制电流，电流不好搞，我们就搞电压呗。空间电压跟产生的磁场正交（不是很确定，按道理是的）。并且，这个空间电压还要稳定，就有如图：

<div align=center><img src="https://github.com/CXianRen/miniFOC/blob/master/img/Space_vectors.gif"/></div>  
    紫色为合成的电压，其他三个色是相位差120°的相电压
    那么只要在abc三个相中输入 3个相位差120°的正弦波就能搞出这个玩意了。
    但是emmmm，有这么简单就好了
## 驱动电路（逆变器）