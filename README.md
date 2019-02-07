# [一个嵌入式操作系统的实现过程](https://gaoyichao.com/Xiaotu/?book=XiaoTuOS&title=index)

在学生时代，操作系统对于我而言是一个很神秘的东西，虽然从书本中了解了一些操作系统的基本原理，
但总缺少一些直观的认识。硕士毕业那年，机缘巧合地看到了于渊写的《Orange'S一个操作系统的实现》这本奇书，
得以管窥一二。感觉写一个操作系统应该是一件很酷的事情，而且也不是那么难，只要对计算机结构有清晰的认识，
就应该没有问题。

但是，X86这个架构对于我而言有点复杂了，我需要花费很多精力去了解各种各样的历史原因，
有点麻烦。相比之下，我更喜欢开放的ARM架构，这样我可以自由的把控各个系统外设，必要时可以自定义，是一个很不错的起点。
于是，决定在一款ARM芯片上写一个嵌入式的操作系统。

本系列文章是建立在[《STM32入门指南》](https://gaoyichao.com/Xiaotu/?book=stm32&title=index)基础之上的，
一开始选用的开发板是淘宝上买的银杏科技的iCore3，后来切换到了[正点原子的探索者](https://gaoyichao.com/Xiaotu//resource/refs/探索者原理图.pdf)。
假设读者对于STM32的单片机有一定的了解。源码托管在了[Github](https://github.com/gaoyichao/XiaoTuOS.git)上。

原先iCore3开发板的代码托管在[这里](https://github.com/gyc2015/XiaoTuOS.git)。

---

系统时钟计算方法：
f(VCO clock) = f(PLL clock input) * PLLN / PLLM
f(PLL general clock output) = f(VCO clock) / PLLP
f(USB OTG FS, SDIO, RNG clock output) = f(VCO clock) / PLLQ

PLLI2S时钟计算方法：
f(VCO clock) = f(PLLI2S clock input) * PLLI2SN / PLLM
f(PLLI2S clock output) = f(VCO clock) / PLLI2SR

