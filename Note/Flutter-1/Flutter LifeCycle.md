<h2>Flutter LifeCycle</h2>

<hr>

- 플러터에도 안드로이드에서처럼 생명주기를 가지고 있다.
- 프로그램이 어떤 순서로 어떤 과정으로 실행이 되는지에 대한 순서도이다. 
- 그 중에서 플러터에서는 모든 것이 위젯으로 이루어져 있기 때문에, 각 위젯의 생명주기, 위젯이 어떻게 시작하고 만들어지고, 사라지는지에 대한 과정이다. 

- 그렇기에 위젯의 종류는 두가지로 나뉜다고 했다. 
  - Stateless Widget
  - Stateful Widget

<hr>

<h2>Stateless Widget</h2>

- 상태가 없는 위젯으로 생성자 실행 후에 <u>바로 build 메소드가 호출</u> 된다. 

```dart
Stateless
    |
Constructor
    |
    |
    |
  build()
```

- build() 메소드외에서는 BuildContext에 접근할 수 없다. 



<hr>

<h2>Stateful Widget</h2>

- state, 즉, 상태가 있는 위젯으로 createState() 메소드를 통해 Stater가 생성된다.
- 기본적으로 Stateful Widget은 immutable한 위젯이다. 
- State 클래스로 이제 상태를 변경해주는 것이다. 

```dart
                    Stateful
                       |
Constructor
    |                                              |
createState()               setState()             |
     |                          |                  |
 initState()                    |          didUpdateWidget()
    |                           |                  |
didChangeDependencies()         |                  |
    |                           |                  |
                 build()
     ...          ...          ...
                dispose()
```



1. createState()
   - StatefulWidget 객체를 생성하면 생성자가 호출되는데, 그 후에 곧바로 createState()가 호출된다. 
   - State 객체를 생성해주는 역할을 한다.
2. initState()
   - State 객체가 생성되면서, 위젯이 최초 생성되는 상황에 호출된다. 즉, 처음 이 위젯이 생성될 때, 단 한번만 호출이 된다. 
   - 초기 setting을 위한 메소드이며, rebuild를 하여도 호출되지 않는다. 
   - 이 initState() 메소드에서는 async await가 불가능하며, BuildContext에 접근할 수 없다.
3. didChangeDependencies()
   - initState()가 호출된 후에 호출이 되는데, 본질적으로 해당 위젯이 의존하는 위젯이 변경되면 호출이 된다.
   - inheritedWidget을 사용하는 경우가 대표적이다.
   - 위젯 A가 위젯 B를 상속받았는데, 위젯 B가 업데이트될 때 이 메소드가 호출이 된다. 
   - 초창기에는 initState()처럼 똑같이 1번 호출이 된다. 
   - 그러나 BuildContext에 접근이 가능하다.
4. build()
   - 이 build() 메소드를 통해 실질적으로 위젯을 그려진다.(render) 
   - 위젯을 그려주는 역할을 하기 때문에, 초기에 물론이고, 변경될 때마다 호출이 된다. 
5. setState()
   - State 객체의 상태가 변경되었다는 것을 알려주는 메소드이다. 
   - State 객체의 상태가 변경될 때마다 setState() 메소드를 통해서 알려준다. 
   - 이 메소드를 호출하면 내부 argument에 들어간 함수를 실행하고 build() 메소드를 호출해준다. 
   - rebuild를 요청하여 state의 값이 변경된 것을 알려준다.
6. didUpdateWidget()
   - 부모 위젯이 rebuild되어 위젯이 갱신될 때 호출된다.
   - didUpdateWidget()이 호출된 후에는 항상 build() 메소드를 호출한다. 
   - 과거의 위젯과 현재 위젯을 비교하는 메소드이다. 
   - State 값을 비교하여 rebuild를 막거나 실행함
7. dispose()
   - 이 State 객체가 영구적으로 제거될 때, 사라질 때 호출이 된다. 영구적으로 제거되기 때문에
   - build 메소드가 호출되지 않는다. 즉, State 객체가 폐기되어진 상황이다. 
   - initState()와 쌍으로 쓰이며, Memory Leak 현상을 방지하기 위해 메모리를 해제해주는 역할을 해준다. 
   - 화면이 종료될 때 호출이 된다는 의미이다. 