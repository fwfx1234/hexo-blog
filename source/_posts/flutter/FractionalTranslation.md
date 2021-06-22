#### FractionalTranslation使用百分比来做动画

最近遇到一个从底部弹窗出来的需求，但是弹窗高度是根据child组件的高度确定的，不是很好动态计算，翻CupertinoPageRoute的源码，看他是咋做的，就发现这个SlideTransition组件，里面使用FractionalTranslation来做动画的，支持百分比，刚好适合我的需求。

```dart
FractionalTranslation(
        translation: Offset( 0.0, _animation?.value),
        child: Container(),
      );
```

使用起来很简单，没啥可说的，实现还没研究，日后研究了再补上，先记录下有这么个好用的组件。