#### 自定义转场动画

```dart
import 'package:flutter/material.dart';
enum Router_Type { Fade,Scale,SpinScale,FromLeft }
/* CustomRouter
** 自定义转场动画
* widget 主题页面
* routerType 转场类型 淡入、缩放、旋转缩放、左入(默认)
* 使用示例： Navigator.push(context,CustomRouter(Widget(参数)));
* createBy:zk
*/
class CustomRouter extends PageRouteBuilder{

   final Widget widget;
   final Router_Type routerType;
    // ignore: missing_required_param
    CustomRouter(this.widget,{this.routerType = Router_Type.FromLeft}):super(
      transitionDuration: Duration(milliseconds: 500),
      pageBuilder: (BuildContext context,Animation<double> animation1,Animation<double> animation2){
        return widget;
      }
    );
    @override
  Widget buildTransitions(BuildContext context, Animation<double> animation, Animation<double> secondaryAnimation, Widget child) {
    switch (this.routerType) {
      case Router_Type.Fade:
         //渐变效果
        return FadeTransition(
          opacity: Tween(begin: 0.0,end: 1).animate(CurvedAnimation(
            parent: animation,
            curve: Curves.fastLinearToSlowEaseIn,
          )),
          child: child,
        );
        break;
      case Router_Type.Scale:
         // 缩放效果
        return ScaleTransition(
          scale: Tween(begin: 0.0,end: 1.0).animate(CurvedAnimation(
            parent: animation,
            curve: Curves.fastOutSlowIn,
          )),
          child: child,
        );
        break;
      case Router_Type.SpinScale:
        // 旋转+缩放
        return RotationTransition(
          turns: Tween(begin: 0.0,end: 1.0).animate(CurvedAnimation(
            parent: animation,
            curve:Curves.fastOutSlowIn
          )),
          child: ScaleTransition(
          scale: Tween(begin: 0.0,end: 1.0).animate(CurvedAnimation(
            parent: animation,
            curve: Curves.fastOutSlowIn,
          )),
          child: child,
        ),
        );
        break;
      default:
      // 左右滑动动画
        return SlideTransition(
        position: Tween<Offset>(
          begin: Offset(1.0,0),
          end: Offset(0.0,0.0),
        ).animate(CurvedAnimation(
          parent: animation,
          curve: Curves.fastOutSlowIn,
        )),
          child: child,
        );
    }
  }
}
```
