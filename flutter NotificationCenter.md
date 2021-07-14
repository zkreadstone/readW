### 通知中心的封装

```dart
class NotificationCenter {
  // 工厂模式
  factory NotificationCenter() => _getInstance();

  static NotificationCenter get instance => _getInstance();
  static NotificationCenter _instance;

  NotificationCenter._internal() {
    // 初始化
  }

  static NotificationCenter _getInstance() {
    if (_instance == null) {
      _instance = new NotificationCenter._internal();
    }
    return _instance;
  }

  //创建Map来记录名称
  Map<String, dynamic> postNameMap = Map<String, dynamic>();

  // GetObject getObject;
  Map<String, GetObject> postFunc = Map<String, GetObject>();

  //添加监听者方法
  addObserver(String postName, object(dynamic object)) {
    postNameMap[postName] = null;
    postFunc[postName] = object;
    // getObject = object;
  }

  //发送通知传值
  postNotification(String postName, dynamic object) {
    //检索Map是否含有postName
    if (postNameMap.containsKey(postName)) {

      postNameMap[postName] = object;
      GetObject getObject = postFunc[postName];
      getObject(postNameMap[postName]);
    }

  }
  //移除通知
  removeNotification(String postName) {
      
     if (postNameMap.containsKey(postName)) {

        postNameMap.remove(postName);
        postFunc.remove(postName);
     }
  }
}
```
