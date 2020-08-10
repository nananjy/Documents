# Tengine

## 回顾
说起nginx大家都不陌生，以前我们一直用lamp平台（L指Linux，A指Apache，M一般指MySQL，也可以指MariaDB，P一般指PHP），直到后来我们用lnmp平台（L指Linux，N指Nginx，M一般指MySQL，也可以指MariaDB，P一般指PHP），区别就在于apache和nginx区别是nginx并发能力更强，虽然Apache功能强大。

## Tengine定义
Tengine是由淘宝网发起的Web服务器项目。它在Nginx的基础上，针对大访问量网站的需求，添加了很多高级功能和特性。Tengine的性能和稳定性已经在大型的网站如淘宝网，天猫商城等得到了很好的检验。它的最终目标是打造一个高效、稳定、安全、易用的Web平台。

## nginx和tengine的区别
1. tengine是在nginx上面开发的，包含了nginx的性能。
2. tengine更适合大访问量网站的需求，相比nginx更加的稳定，性能更加的强劲。
```
据网络测试：
Tengine相比Nginx默认配置，提升200%的处理能力。
Tengine相比Nginx优化配置，提升60%的处理能力。
参考：https://blog.csdn.net/lianjiangdai/article/details/94907289
```


