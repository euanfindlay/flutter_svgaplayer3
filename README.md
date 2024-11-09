# flutter_svgaplayer3

## Introduce

flutter_svgaplayer3 is a expand SVGAPlayer-Flutter light-weight animation renderer. You use [tools](https://svga.io/designer.html) to export `svga` file from `Adobe Animate CC` or `Adobe After Effects`, and then use SVGAPlayer to render animation on mobile application.

`SVGAPlayer-Flutter` render animation natively via Flutter CustomPainter, brings you a high-performance, low-cost animation experience.

If wonder more information, go to this [website](https://svga.io/).
* SVGAPlayer-Flutter only supports version 2.0 format (the original author has stopped maintaining it).
* flutter_svgaplayer3 supports 3.0 format only.

## Usage

Here introduce `flutter_svgaplayer3` usage. Wonder exporting usage? Click [here](https://svga.io/designer.html).

### Add dependency

```
dependencies:
  flutter_svgaplayer3: ^0.0.1  #latest version
```

### Locate files

SVGAPlayer could load svga file from Flutter local `assets` directory or remote server. Add file to pubspec.yaml by yourself.

### Super simple to use

The simplest way to render an animation is to use `SVGASimpleImage` component.

```dart
class MyWidget extends Widget {

  @override
  Widget build(BuildContext context) {
    return Container(
      child: SVGASimpleImage(
          resUrl: "https://github.com/yyued/SVGA-Samples/blob/master/angel.svga?raw=true"),
    );
  }

}
```

Animation will run repeatedly. If you wondering a stronger animation controls, use code.

### Code to use

To control an animation rendering, you need to create a `SVGAAnimationController` instance just like Flutter regular animation. Assign to `SVGAImage`, load and decode resource using `SVGAParser`, and then do things as you want with `SVGAAnimationController`.

```dart
import 'package:flutter/material.dart';
import 'package:flutter_svgaplayer3/parser.dart';
import 'package:flutter_svgaplayer3/player.dart';

void main() => runApp(MyApp());

class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> with SingleTickerProviderStateMixin {
  SVGAAnimationController animationController;

  @override
  void initState() {
    this.animationController = SVGAAnimationController(vsync: this);
    this.loadAnimation();
    super.initState();
  }

  @override
  void dispose() {
    this.animationController.dispose();
    super.dispose();
  }

  void loadAnimation() async {
    final videoItem = await SVGAParser.shared.decodeFromURL(
        "https://github.com/yyued/SVGA-Samples/blob/master/angel.svga?raw=true");
    this.animationController.videoItem = videoItem;
    this
        .animationController
        .repeat() // Try to use .forward() .reverse()
        .whenComplete(() => this.animationController.videoItem = null);
  }

  @override
  Widget build(BuildContext context) {
    return Container(
      child: SVGAImage(this.animationController),
    );
  }
}
```

### Reuse MovieEntity

The `MovieEntity` will `dispose` after `AnimationController` dispose or `AnimationController::videoItem` reset.

After dispose, the `MovieEntity` can not reused.

If you eager to reuse the `MovieEntity`, assign `MovieEntity::autorelease` to false.

```dart
final videoItem = await SVGAParser.shared.decodeFromURL(
        "https://github.com/yyued/SVGA-Samples/blob/master/angel.svga?raw=true");
videoItem.autorelease = false;
```

You need to call `MovieEntity::dispose()` method when no need to use.

### Cache

We will not manage any caches, you need to use `dio` or other libraries to handle resource caches.

Use `SVGAParser.decodeFromBytes` method to decode caching data.

## Features

Here are many feature samples.

* [Replace an element with Bitmap.](https://github.com/yyued/SVGAPlayer-Flutter/wiki/Dynamic-Image)
* [Add text above an element.](https://github.com/yyued/SVGAPlayer-Flutter/wiki/Dynamic-Text)
* [Hides an element dynamicaly.](https://github.com/yyued/SVGAPlayer-Flutter/wiki/Dynamic-Hidden)
* [Use a custom drawer for element.](https://github.com/yyued/SVGAPlayer-Flutter/wiki/Dynamic-Drawer)

## License

[![996.ICU](https://img.shields.io/badge/link-996.icu-red.svg)](https://996.icu)

[Anti 996](./LICENSE)
