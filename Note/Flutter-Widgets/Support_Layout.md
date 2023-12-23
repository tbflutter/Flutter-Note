<h2>Flutter Widgets</h2>

<hr>

<h2>Support Layout Widgets</h2>

<hr>

- 레이아웃 위젯중 배치, 화면경계를 용이하도록 도와주는 레이아웃 위젯들

<hr>

<h2>1. SizedBox</h2>

- 위젯간의 간격을 조절하고 싶을 때 사용한다. (혹은 칸을 추가하거나)
- (사실 간격 조절이지 그 안에 이 SizedBox로 공간이 차있는 것이다.)

> 속성 값

- child -> Widget
  - Container와 같은 역할을 하는 child 속성이다.
- width -> double
  - 이 SizedBox의 너비(가로)의 크기를 설정한다.
- height -> double
  - 이 SizedBox의 높이(세로)의 크기를 설정한다. 

<hr>

<h2>2. SafeArea</h2>





<hr>

<h2>3. Padding</h2>

- 위젯간의 간격을 조절하고 싶을 때 사용한다.
- SizedBox와 달리 정말 칸을 띄우는 것이다. 
- 주로 Multi-Child Widget에서 자식 위젯간의 간격을 조절할 때 사용함

> 속성 값

- child -> Widget

- padding -> EdgeInsetsGeometry

  - 이 속성은 이 Padding 위젯에만 존재하는 것이 아니라 다양한 위젯들에 **padding** 속성이 존재하는데 다 공통되게 사용이 된다. 

  - **EdgeInsets** 클래스와 맵핑되어서 사용이 된다. 
  - 각 메소드마다에 들어가는 값은 double 값으로 얼마나 떨어뜨려놓을지에 대한 값이다. 
    1. **EdgeInsets.all(value)** : 상하좌우 모두 띄워주겠다는 의미
    2. **EdgeInsets.only(left, right, top, bottom)** : 각 argument들은 Named Argument로써,  원하는 방향에 대해서 각각 선택적으로 사용할 수있다. (말 그대로 only) <i> default 값으로 모두 0을 가짐</i>
    3. **EdgeInsets.symmetric(vertical, horizontal)** : 가로와 높이 대해서 떨어뜨리는 효과이다. (이것도 마찬가지로 Named argument로써 사용하고 싶은 축에 대해 선택적으로 사용하면 된다.) <i> default 값으로 모두 0을 가짐</i>



