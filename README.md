big.LITTLE
=======





#1	CONFIG_OF 宏的问题
-------

##1.1	启用
-------

我们启用了 CONFIG_OF


##1.2	引入的问题
-------

但是发现驱动中很多 OF 相关的函数, 未定义

```cpp
drivers/clocksource/sunxi_timer.c

error: implicit declaration of function ‘of_clk_get’ [-Werror=implicit-function-declaration]
```

```cpp
drivers/pinctrl/pinctrl-sunxi.c
error: implicit declaration of function ‘of_property_for_each_string’ [-Werror=implicit-function-declaration]
```

解决办法

1	实现未定义的函数

2	在驱动中取消 CONFIG_OF 宏

3	用其他函数替代

4	#if 0 屏蔽有问题的代码


#2	arch/arm/kernel/topology.c
-------


其中未定义

```cpp
static DEFINE_PER_CPU(unsigned long, cpu_scale);
```

及其函数接口, 于是我们定义了宏

```cpp
/*  add by gatieme(ChengJean) @ 2016-12-08 00:19 */
#ifdef CONFIG_CPU_POWER_MANAGEMENT
#endif
```

#3	kernel/stop_machine.c
-------

未定义 `cpu_stopper_task`, 及其函数


#4	CONFIG_NO_HZ_COMMON
-------

未定义`CONFIG_NO_HZ_COMMON` 宏, 因此很多函数未定义


#5	arm-linux-gnueabi-nm
-------

```cpp
/home/gatieme/Work/Arm/Kernel/3.4/GitHub/3.4/scripts/mksysmap: line 44: ~/Work/Arm/Board/cubieboard/cc-a80/cc-a80-linux-sdk/tools/crosscompiler/bin/arm-linux-gnueabi-nm: No such file or directory
```
