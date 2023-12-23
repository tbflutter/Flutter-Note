<h2>Flutter Widgets</h2>

<hr>

<h2>IndexedStack</h2>

<hr>

- 기본적인 Stack 위젯은 위젯들이 한 화면에서 Stack 자료구조처럼 중첩되면서 쌓인다. 
- IndexedStack은 이런 중첩된 위젯들이 인덱스를 가지면서 특정 인덱스를 화면 가장 위에 서로 전환되면서 사용되는 위젯이다. 
- 채널을 변환하는 TV와 같은 역할을 한다. 
- 한번에 하위 위젯 하나만을 보여주지만, 모든 하위 위젯의 상태를 유지해준다. 
- 그래서 이 IndexedStack은 중첩된 위젯들 중에서 하나를 화면의 최상단에 올려놓고, 사이에 화면에 보여줄 인덱스가 변경되면, 그 인덱스에 맞는 위젯이 화면 상단에 보여지게 된다.
- 여기서 인덱스라 함은 이 IndexedStack의 children 속성, 즉 위젯들의 리스트의 각 인덱스이다. 
- 그래서 앱 안에서 위젯들간의 간편한 이동, 변경에 유용하다.
- 그래서 Tap, BottomNavigationBar등 화면간의 이동등에 간편하게 사용될 수 있다. 
- 유용한 이유는 이 위젯의 인덱스를 바꿔도 나머지 invisible한 위젯의 상태는 그대로 보존이 되기 때문이다. 



> 속성

- children -> List&lt;Widget&gt;
  - IndexedStack의 자식 위젯들이 들어가는 곳이다. 
- ★ index -> int?
  - children에 속한 자식 위젯들 중 화면에 보여줄 위젯의 인덱스이다. 
- alignment, clipBehavior, fit 
  - Stack과 동일



```dart
IndexedStack(
	children : [
        Widget1(),
        Widget2(),
        Widget3()
    ],
    index : _currentIndex 
)
/*
_currentIndex가 0일 경우 Widget1()이 보여짐
_currentIndex가 1일 경우 Widget2()가 보여짐
_currentIndex가 2일 경우 Widget3()가 보여짐
*/
```





