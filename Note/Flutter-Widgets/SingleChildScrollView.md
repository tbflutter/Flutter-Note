<h2>Flutter Widget</h2>

<hr>

<h2>SingleChildScrollView</h2>

<hr>

- 주로 화면은 고정되있는 크기에 이르러있다. 
- 그래서 플러터의 경우에는 지정된 화면을 오버플로우 시키면 에러 아닌 경고같은 것을 절취선 형태로 띄운다. 
- 또한 무언가 입력할 때 키보드창을 열 때도 위와 같은 현상이 일어난다. 
- 이러한 오버플로우를 유연하게 다룰 수 있는 위젯이 **SingleChildScrollView** 위젯이다.

- SingleChildScrollView 위젯은 세로축, 가로축에 대해서 화면을 스크롤할 수 있게 해주는 역할을 한다. 



- 스크롤이 필요한 화면(오버플로우 될 것 같은)구성 중에서 가장 최상단에 존재하는 레이아웃 위젯의 Parent로 들어와야한다. 

```dart
@override
Widget build(BuildContext context){
    return Scaffold(
    	body : SingleChildScrollView(
        	child : Center(
            	child : Text('Hello World')
            )
        )
    );
}
```



> 속성 

- 가장 중요한 속성은 **scrollDirection**으로 스크롤을 어느 방향(축)으로 설정할 것인지에 대해 정하는 것이다.
- default값으로 세로방향(vertical 방향)으로 스크롤을 하게 해준다. 
- Axis.horizontal - 가로 방향(축)
- Axis.vetical - 세로 방향(축)
- child는 스크롤을 가능케 하고 싶은 가장 최상단 레이아웃 위젯을 의미한다. 



- 만약 가로축, 세로축 모두 스크롤하고 싶다면 **SingleChildScrollView**를 중첩시키는 것이다.

```dart
SingleChildScrollView(
    scrollDirection : Axis.vertical // 세로축 스크롤
	child : SingleChildScrollView(
        scrollDirection : Axis.horizontal // 가로축 스크롤
    	child : Column(
        	children : [...]
        )
    )
)
```