### 占位容器Widget
每次展示网络请求的模块Widget，都会有一套逻辑：loading占位、请求失败占位、请求成功结果、暂时为空、网络问题占位等基本占位或者结果
为了开发的便捷性，可以在占位范围封一层widget，专门处理各种占位，只需要传入状态和成功的Widget就能无缝替换

但是组件高度不确定，就需要适配高度，例如列表的占位一般屏幕高度；独立卡片的占位高度一般固定多少

点击重试的回调暴露出去；

```dart
  import 'package:flutter/material.dart';
import 'package:loading_indicator_view/loading_indicator_view.dart';

enum BodyLoadingState {
  loading, //加载中
  success, //正常
  failure, //失败
  empty, //空视图
  netunconnect, //断网
}

class BodyContainer extends StatefulWidget {
  final child;
  final loadingWidget;
  final emptyWidget;
  final loadFailedWidget;
  final resetH;
  final reloadCallBack;
  final loadingState;
  final emptyTitle;
  final failedTitle;
  final failedBtnWidget;
  final loadingTitle;
  /// Widget占位 适用对象：带网络请求的组件，用此组件
  /// 
  /// 通过 loadingState控制占位类型
  /// 
  /// resetH 加载占位的高度，此高度居中，预估值 默认300
  /// 
  /// reloadCallBack 点击重试的回调
  /// 
  /// loadingWidget、emptyWidget、loadFailedWidget 见名知义
  /// 
  /// 创建者：zk  2021/7/15 
  BodyContainer({Key key,
  @required this.child,
  this.resetH,
  this.reloadCallBack,
  this.loadingWidget,
  this.emptyWidget,
  this.loadFailedWidget,
  this.loadingTitle = '加载中...',
  this.emptyTitle = '暂无结果',
  this.failedTitle = '加载失败，点击重试',
  this.failedBtnWidget,
  this.loadingState = BodyLoadingState.loading}) : super(key: key);

  @override
  _BodyContainerState createState() => _BodyContainerState();
}

class _BodyContainerState extends State<BodyContainer> {
  //加载中
  _loading(){
    if (widget.loadingWidget != null) {
      return widget.loadingWidget;
    }
    return Container(
       height:widget.resetH,
       child:Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children:[
          Container(
            width:40,height:40,
            margin: EdgeInsets.only(bottom:20),
            child: LineSpinFadeLoaderIndicator(
              ballColor: Colors.grey,
            ),
          ),
          Text(widget.loadingTitle),
        ]));
  }
  //加载失败
  _loadingFailed(){
    if (widget.loadFailedWidget != null) {
      return widget.loadFailedWidget;
    }
    return GestureDetector(
      onTap:(){//点击重新加载
        if(widget.reloadCallBack != null){
          widget.reloadCallBack();
        }
      },
      child:Container(
       height:widget.resetH,
       child:Column(
          mainAxisAlignment: MainAxisAlignment.center,
        children:[
          Image.asset('images/nft/load_failed.png',width: 90,),
          SizedBox(height: 20,),
          GestureDetector(
            child:widget.failedBtnWidget ?? Container(
              alignment: Alignment.center,
              // padding: EdgeInsets.only(left:20,right:20),
              width: 140,
              height:30,
              decoration: BoxDecoration(
                borderRadius:BorderRadius.all(Radius.circular(5)),
                color:Colors.red,
              ),
              child:Text(widget.failedTitle)
            )
          ),
        ]))
    );
  }
  //加载结果空
  _loadEmpty(){
    if (widget.emptyWidget != null) {
      return widget.emptyWidget;
    }
     return Container(
       height:widget.resetH,
       child:Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children:[
          Image.asset('images/nft/load_failed.png',width: 90,),
          SizedBox(height: 20,),
          Text(widget.emptyTitle)
        ]
      ));
  }

    //网络断开 一般通用 所以不用在外部传入
  _netUnConnect(){
     return Container(
       height:widget.resetH,
       child:Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children:[
          Container(
            width:70,height:70,color:Colors.cyan,
            margin: EdgeInsets.only(bottom:20),
          ),
           Text('无网络，请检查网络后重试'),
        ])
     );
  }


  _result(){
    switch (widget.loadingState) {
      case BodyLoadingState.loading:
        return _loading();
        break;
      case BodyLoadingState.failure:
        return _loadingFailed();
        break;
      case BodyLoadingState.empty:
         return _loadEmpty();
        break;
      case BodyLoadingState.success:
        return widget.child;
        break;
      case BodyLoadingState.netunconnect:
        return _netUnConnect();
        break;
      default:
    }
  }
  @override
  Widget build(BuildContext context) {
    return Container(
      alignment: Alignment.center,
      // height: widget.resetH,
       child: _result(),
    );
  }
}

```
