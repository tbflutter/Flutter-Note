<h2>Flutter How flutter Reders Widgets</h2>

<hr>

- 위젯은 불변하지만, 위젯 트리는 변경 가능하다. 
- 위젯 트리에서 일부를 빼내고, 다른 위젯 구성으로 교체할 수 있다. 
- 하지만 위젯의 일부를 변경하기 위젯 트리 전체를 rebuild하는 것은 원하지도 않고 성능면에서 좋지 않다. 

**그렇다면 플러터는 어떻게 위젯 트리를 관리할까?**

> Three Trees of Flutter 

- 플러터에서는 하나의 트리(위젯트리)를 통해 위젯을 관리하는 것으로 보이지만 
- 실상 3가지의 트리로 구성되어 있다. 

```
Widget - Element - RenderObject
(Widget tree) - (Element tree) - (Render tree)
```

- 위젯 트리는 우리가 코드상에서 얼마든지 control할 수 있다. 
- 그러나 element tree와 render tree는 플러터가 내부적으로 control하는 것이다. 이 두개의 트리는 우리가 **만든 위젯 트리에 근거하여** 생성된다.
- 위젯 트리는 우리가 작성한 코드에 근거하여 플러터가 build 메소드를 호출해서 생성하는 것이다. 
- 이 위젯 트리는 하나의 구성일 뿐 직접적으로 화면에 그려지지는 않는다. 
- 그러므로 위젯 트리는 하나의 설계도역할로 플러터에게 화면에 이러한 순서와 모양으로 UI를 그려달라고 하는 역할이다. 



- 중요한 것은 Element tree로 이 Element tree는 중간에서 위젯트리와 Render트리를 연결하고 있다. 
- Element 트리는 플러터가 자동으로 우리가 만든 위젯트리에 근거하여 생성을 해주는 요소이며
- 위젯트리에 있는 모든 위젯 하나하나가 1대1로 Element tree와 링크되어있다. 



- Render tree는 직접적으로 화면에 그려주는 High Level System이다. 

- Element tree는 중간에서 Render tree가 실제적으로 렌더링하기 위해서 가지고 있는 render 객체와 1대1로 링크되어있다. 

- 눈으로 보이는 화면의 모든 요소들은 Render tree의 작업 결과이다. 

- 위젯트리가 생성될때마다 Element tree도 함께 생성된다. 

  

- element라는 것도 위젯들의 정보를 담고 있는 하나의 객체이다. 

- 다른 위젯들처럼 element도 메모리에 등록되어서 플러터에 의해 control되는 요소이다.

```dart
Widget tree <---------- Element tree 
    Cotainer <--------  Container element
    
```

- 위젯트리에서 Container가 생성되면 즉시, 이에 대응하는 Container element를 Element tree상에 추가한다. 

  그리고 이 Container element는 위젯트리상의 Container 위젯을 가리킨다.(pointer) 

- 또한 element의 특징은 위젯을 가리키고있는 것만 아니라 그 위젯의 정보를 함께 가지고 있다. (속성값등)



<hr>

- 위젯 트리는 build 메소드가 호출될 때마다 새롭게 re-build된다. 
- 위젯트리가 매번 rebuild된다는 것은 위젯트리 자체도 한번 생성되면 바뀌지 않기에 바꾸려면 아예 새로 만들어야한다. 
- 이는 위젯트리내에 모든 위젯들도 rebuild된다는 의미이다. 
- 그러나 사소한 부분만 변경해도 Hot reload 때문에 매번 build메소드가 호출되면서 위젯 트리가 rebuild된다. 
- 그러면 이에 연결된 Element tree도 rebuild도 되고 이에 맞물린 Render tree까지 rebuild되어서 최악의 Performance가 된다. 



- 이러한 문제를 Element tree가 해결해준다. 
- Element tree는 위젯 트리 구조를 그대로 복사하기때문에 링크되었던 위젯의 위치, 타입, 속성들의 모든 정보를 가지고 있다. 
- 그래서 어떤 위젯의 새롭게 추가된 부분때문에 build메소드가 호출되고 위젯트리가 rebuild된다면 
- Element tree도 함께 rebuild되는 것이다 아니라 새롭게 생성된 위젯들의 타입과 위치가 일치하면 그대로 링크만 업데이트 한다. 
- 그리고 타입과 위치는 같지만 새로운 코드에 의해 다시 렌더링이 필요한 위젯에 한하여 Render tree에게 정보를 전달해주고 Render tree는 이를 근거로 기존 위젯의 Render 객체에서 바뀐 부분만을 다시 그려주게 된다. 

<h3>Ex)</h3>

1. Container 배경색을 white -> blue로 바꿈
2. 반영하기 위해 Hot reload 실행
3. build 메소드 호출
4. 위젯 트리가 rebuild되면서 위젯 트리상의 모든 위젯들 rebuild됨
5. Element tree는 rebuild된 위젯들을 기존 위젯들의 정보와 비교한 후 일치하면 새로운 위젯들로 링크만 업데이트함
6. 렌더링이 필요한 위젯에 대해서 Render tree에게 정보를 넘겨주고 Render tree는 Container의 색상만 변경한 후 다시 화면에 그리게 함