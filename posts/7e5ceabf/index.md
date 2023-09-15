# 让windows  10完美支持Intel处理器的大小核调度

实在是不喜欢windows 11的各种设计，但是新买的13700K据说只能在windows 11下才能完美发挥性能。

有没有让windows 10支持intel 13代cpu大小核调度的方法呢？

一番搜索，找到了完美的解决方案。

原文在[这里](https://www.bilibili.com/video/BV1LK411B7ZS/?spm_id_from=333.1007.top_right_bar_window_default_collection.content.click&vd_source=f58c6359690424b3ca690f7d63a18318)

## 打开windows 10电源管理的隐藏功能

使用管理员打开`powershell`,输入以下命令：
```
# 处理器性能核心放置最小核心数量 （小核或全部睡眠数量，取决于异类策略）
powercfg -attributes SUB_PROCESSOR 0cc5b647-c1df-4637-891a-dec35c318583 -ATTRIB_HIDE

#针对第1类处理器电源效率的处理器性能核心放置最小核心数量（大核+超线程睡眠数量百分比）
powercfg -attributes SUB_PROCESSOR 0cc5b647-c1df-4637-891a-dec35c318584 -ATTRIB_HIDE

#在电源选项中查看：生效的异类策略
powercfg -attributes SUB_PROCESSOR 7f2f5cfa-f10c-4823-b5e1-e93ae85f46b5 -ATTRIB_HIDE

#在电源选项中查看：异类线程调度策略
powercfg -attributes SUB_PROCESSOR 93b8b6dc-0698-4d1c-9ee4-0644e900c85d -ATTRIB_HIDE

#在电源选项中查看：异类短运行线程调度策略
powercfg -attributes SUB_PROCESSOR bae08b81-2d5e-4688-ad6a-13243356654b -ATTRIB_HIDE

```

## 调整windows 10电源计划设置
做如下设置：
```
# 台式机推荐的策略（游戏推荐）
处理器性能放置最小核心数量：100%
针对第1类处理器电源效率的处理器性能核心放置最小核心数量：50%（休眠超线程）
生效的异类策略：使用异类策略1
异类线程调度策略：首选高性能处理器
异类短线程调度策略：首选高性能处理器
```

以上设置是针对台式机的，笔记本电脑另有设置，可查看原文


