<h2>Flutter Widgets</h2>

<hr>

<h2>ListView</h2>

<hr>

- 스크롤이 가능한 리스트안에 나타났으면 하는 아이템 항목들을 넣어서 사용할 때 ListView 위젯을 사용한다. 
- 간편하게 사용이 가능하다. 

> ListView 속성

1. children
   - 리스트 뷰에 보여질 자식 위젯 리스트들
2. scrollDirection
   - 리스트 보여지는 방향 설정 Axis.horizontal, Axis.vertical (가로, 세로)
3. reverse -> bool
   - 아이템의 순서를 거꾸로 보여주게 한다. 
4. physics
5. addAutomaticKeepAlives
6. cacheExtent



 

> ListView.builder

- ListView의 리스트들을 동적으로 만들고 싶을 때 사용한다. 
- 동적으로 추가하거나, 어딘가에서 데이터를 받아온 것을 토대로 리스트형식으로 뿌려주고 싶을 때 사용한다. 
- 그래서 멤버 변수로 List 타입의 변수로 핸들링한다. 

| ----        | ----                                | ----                                                         |
| ----------- | ----------------------------------- | ------------------------------------------------------------ |
| itemCount   | int                                 | ListView에 보여질 리스트 아이템의 개수                       |
| itemBuilder | IndexedWidgetBuiler(context, index) | 동적으로 리스트를 build해주는 속성으로 함수에 index를 받는데, 이는 리스트뷰의 현재 아이템의 인덱스를 뜻한다. 그리고 ListView에 보여질 각각 아이템(위젯)을 리턴하면 된다. |

```dart
class _ListViewPageState extends State<ListViewPage> {
  List<int> a = [0];
  @override
  Widget build(BuildContext context) {
    return SafeArea(
      child: Scaffold(
        appBar: AppBar(
          title: Text('listview'),
          actions: [
            IconButton(
              icon: Icon(Icons.add),
              onPressed: () {
                setState(() {
                  a.add(a[a.length - 1] + 1);
                });
              },
            )
          ],
        ),
        body: ListView.builder(
          itemCount: a.length,
          itemBuilder: (context, index) {
            return Text(a[index].toString());
          }, 
        ),
      ),
    );
  }
}
```



> ListView.separated

- ListView.Builder에서 아이템들끼리 만들어질 때마다 붙어있는 것이 아니라 구분자로 나눠주고 싶을 때 사용한다. 

- separatorBuilder 속성을  이용한다. 

```dart
class _ListViewPageState extends State<ListViewPage> {
  List<int> a = [0];
  @override
  Widget build(BuildContext context) {
    return SafeArea(
      child: Scaffold(
        appBar: AppBar(
          title: Text('listview'),
          actions: [
            IconButton(
              icon: Icon(Icons.add),
              onPressed: () {
                setState(() {
                  a.add(a[a.length - 1] + 1);
                });
              },
            )
          ],
        ),
        body: ListView.separated(
          itemCount: a.length,
          itemBuilder: (context, index) {
            return Text(a[index].toString());
          },
          separatorBuilder: (context, index) => const Divider(
            color: Colors.red,
          ),
        ),
      ),
    );
  }
}

```

