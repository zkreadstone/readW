### 1.单击按钮的封装

flutter 开发中经常遇到单击按钮的需求，例如点击按钮进行网络请求，点击按钮进行界面刷新弹窗等，
为了避免出现网络状况不好情况下，用户连击按钮，就会连续请求；或者界面连续点击后不断push的情况；就可以封装一个可以控制点击后延时多长时间才能再次点击的按钮，
封装代码如下：

```dart
class SingleTapBtn extends StatefulWidget {
  final onTap;
  final minTapDuration;
  final child;
  SingleTapBtn({Key key,
  this.onTap,
  this.minTapDuration = 500,
  @required this.child}) : super(key: key);

  @override
  _SingleTapBtnState createState() => _SingleTapBtnState();
}

class _SingleTapBtnState extends State<SingleTapBtn> {
  bool _isCan = true;
  @override
  Widget build(BuildContext context) {
    return GestureDetector(
      onTap: (){
        if (widget.onTap != null && _isCan) {
        widget.onTap();
        _isCan = false;
        // 500 毫秒内 不能多次点击
        Future.delayed(Duration(milliseconds: widget.minTapDuration), () {
          _isCan = true;
        });
      }
      },
      child:widget.child
    );
  }
}

```
