---
layout: post
title: AnimatedBuilder basics
categories:
- flutter
excerpt: Basic example with AnimatedBuilder
---

AnimatedBuilder need to have 2 main arguments specified:

* animation - instance of AnimationController
* builder - build function used for updating widget tree with new animated value.

Example of statefull widget with `AnimatedBuilder`:

```dart
class AnimBuilderPage extends StatefulWidget {
  AnimBuilderPage({Key key}) : super(key: key);

  _AnimBuilderPageState createState() => _AnimBuilderPageState();
}

class _AnimBuilderPageState extends State<AnimBuilderPage>
    with SingleTickerProviderStateMixin {
  AnimationController _controller;
  Animation<double> _size;
  Animation<Color> _color;

  @override
  void initState() {
    super.initState();
    _controller =
        AnimationController(vsync: this, duration: Duration(seconds: 2));

    _size = Tween<double>(begin: 0, end: 200).animate(_controller);
    _color = ColorTween(begin: Colors.lightGreen, end: Colors.lightBlue)
        .animate(_controller);

    _controller.forward();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: AnimatedBuilder(
        animation: _controller,
        child: Container(),
        builder: (BuildContext context, Widget child) {
          return Opacity(
            opacity: _controller.value,
            child: Container(
              width: _size.value / 2,
              height: _size.value,
              color: _color.value,
            ),
          );
        },
      ),
    );
  }
}
```

Basic concepts of `AnimationController` and `Tween` objects:
* `AnimationController` can be initialized only in state class of statefull widget with `SingleTickerProviderStateMixin` mixin, because this mixin will provide ticker state to `AnimationController`
* `AnimationController` is just a iterator (function) that returns value from range 0.0 to 1.0 by default when ticker chaning state
* We can map actual `AnimationController` value from default range to any different range with `Tween`.
* We can connect different `Tween`s object to 1 `AnimationController`. This will allow us create complex animations.

Result (I've updated widget tree a bit):

![image tooltip here](/assets/images/animated_builder.gif)
