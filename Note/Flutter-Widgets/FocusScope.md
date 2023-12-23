<h2>Flutter Widget</h2>

<hr>

<h2>FocusScope</h2>

<hr>

- 입력창에서 키보드를 부르면, 이 키보드를 끄려면은 반드시 뒤로가기 버튼을 누르거나 완료같은 버튼을 눌러야한다. 
- 귀찮게 그렇게 정형화된 규칙에 따라 키보드를 끄고 싶지않을 때, 즉, 어느 화면을 톡 누르면 키보드가 꺼지게 할 때 사용하는 방법이다.



- 이 방법을 사용하기 위해서는 GestureDetector 위젯 클래스를 사용해야한다. 
- 이 GestureDetector 위젯 클래스의 위치는 화면을 눌렀을 때 꺼지게끔 하고 싶은 그 범위의 최상단 위젯의 부모로 두면 된다. 



- 사용방법

```dart
return Scaffold(
	body : GestureDetector(
    	onTap : () => FocusScope.of(context).unfocus(), // 이 부분이 핵심
        child : Column(...)
    )
)
```





