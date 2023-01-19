# Gnome调整鼠标指针大小


gnome如果调整了缩放，鼠标指针的大小不会跟着变，需要自定义大小。

运行下面的命令即可调整鼠标指针大小:

```
dconf write /org/gnome/desktop/interface/cursor-size 40
```

缩放150%的话，调整到大小`40`比较合适。

