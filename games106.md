## 和OpenGL的RHI（rendering hardware interface）的区别

OpenGL的RHI：

* 几乎所有的绘制命令都依赖隐式或者显式的Rendering Context调用
* 驱动干了太多的事，对GPU的掌握比较少啊
* （显卡公司）驱动层任务会比较重

现代的RHI（比如Vulkan）：

* 需要开发者关心同步以及内存分配
* 多线程友好
* 开发者对GPU的掌控权很高

