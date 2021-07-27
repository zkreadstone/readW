ListView 或者 GridView的不可滚动+自适应高度 
 1. shrinkWrap:true,
 2. physics: NeverScrollableScrollPhysics()

```dart
  Container(
    width: MediaQuery.of(context).size.width - 24,
    color: Colors.cyan,
    child:GridView.builder(
      shrinkWrap:true,
      physics: NeverScrollableScrollPhysics(),
      gridDelegate: SliverGridDelegateWithFixedCrossAxisCount(
        crossAxisCount: 2,
        crossAxisSpacing: 11.0,
        childAspectRatio: 170.0/274.0
      ),  
      padding: EdgeInsets.only(top:0,bottom:0),
      itemBuilder:(context,index) => _cardItem(),
      itemCount: dataList[index][1].length,
      ) 
    ),
```
