### BuildContext相关知识点

flutter里经常要用到这个额buildContext，而且一部小心就用翻车了。比如明明有context但是路由跳转就是报错，这里记录下问题吧。



在flutter里使用`Navigator.of(context)` 这种api的时候，这个of操作其实是根据context向上寻找NavigatorState，所以我们要拿到Navigator下面的context才能找到NavigatorState，进而调用上面的各种方法。`MediaQuery`, `Scaffold` 都是这个样子的。所以在MyApp里存一个全局context不一定能去跳转路由。 只有在Navigator组件下面的buildContext才行，MaterialApp在构建的时候，内部会使用Navigator， 所以在onGenerateRoute 返回的Route里的context可以拿来跳转。



### Builder组件

```dart
class Builder extends StatelessWidget {
  /// Creates a widget that delegates its build to a callback.
  ///
  /// The [builder] argument must not be null.
  const Builder({
    Key? key,
    required this.builder,
  }) : assert(builder != null),
       super(key: key);

  /// Called to obtain the child widget.
  ///
  /// This function is called whenever this widget is included in its parent's
  /// build and the old widget (if any) that it synchronizes with has a distinct
  /// object identity. Typically the parent's build method will construct
  /// a new tree of widgets and so a new Builder child will not be [identical]
  /// to the corresponding old one.
  final WidgetBuilder builder;

  @override
  Widget build(BuildContext context) => builder(context);
}
```

我们可以通过Builder组件来获取Builder组件的context, 我们日常开发中经常使用一个页面的顶层context对象。例如

```dart
class Page extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(),
    );
  }
}
```

这个时候你使用这个context是不能正常调用Scaffold.of(context).openDrawer()的，因为context不再scaffold组件的下面，of方法也就找不到对应的对象，所以可以在Scaffold下面的任意组件上使用Builder获得对应context，然后通过这个context来调用Scaffold.of(context)，就能正常找到ScaffoldState了。



> **BuildContext**objects are actually **Element** objects. The **BuildContext**interface is used to discourage direct manipulation of **Element** objects.

官方说builfContext 实际上是Element对象。