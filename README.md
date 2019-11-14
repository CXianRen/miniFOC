# miniFOC
FOC,STM32F4,step-by-step,noob

# 无刷电机怎么动？

[知乎传送门](https://www.zhihu.com/search?q=%E6%97%A0%E5%88%B7%E7%94%B5%E6%9C%BA%E5%8E%9F%E7%90%86&utm_content=search_suggestion&type=content
)
通俗讲就是产生个磁场去拉转子。
![动态原理图](https://github.com/CXianRen/miniFOC/blob/master/img/BLDC原理.gif)
![动态原理图](https://github.com/CXianRen/miniFOC/blob/master/img/q轴磁场.png)
上面演示都为内转子电机，而常用的都是外转子(便宜，┑(￣Д ￣)┍)。原理上都是一样的，反过来就好。

## 外转子电机
### 极对数 = 极对数=极数/2。 
    是数有多少个磁铁，或者可以利用反电动势测出：STM32 官方给的方法（在Motor Profiler 5.4.1中）
    给任意亮相通横流，然后转一圈，看示波器/电流源的电压跳变次数（我没试成，小电机，反电动势太小了，不好测，也不好转）
### 电角度 
    TODO
    
# SVPWM是什么？

