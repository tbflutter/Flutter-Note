<h2>Flutter Widgets</h2>

<hr>

<h2>Container</h2>

<hr>

- 레이아웃 배치를 하는데 도움을 주는 위젯이다. (눈에 보이지 않는 위젯)
- 위젯에 배경스타일, 배경색, 모양, 크기 제한을 두고싶을 때 사용한다. 
- 자식 위젯을 설정하여 구성, 장식, 위치를 정할 수 있다.
- 한 위젯을 담는 공간, 박스라고 생각하면 될 것이다. 
- Container는 "Single-child layout widget"이다. (오직 하나의 자식만을 갖는)

- <i>Containers with no children try to be as big as possible</i>

 => 자식위젯이 없는 경우 Container위젯은 할 수 있는 최대한의 공간을 차지한다. 

- 자식위젯을 가지게 되면 그 자식위젯의 크기로 자동으로 줄어들게 된다.

  => width, height 속성으로 크기를 설정하는 것이 아니라면 

  (Container의 자식으로 Text()위젯으로 넣는다면 이는 텍스트의 폰트 사이즈만큼 Container의 크기가 결정됨)

> 속성 값 - 화살표는 맵핑타입

1. child -> Widget

   - 자식 위젯으로 설정할 위젯 (오직 "하나의 위젯") 

2. color -> Color

   - Container에 담긴 자식 위젯혹은 자기자신의 배경색을 설정한다.

3. width -> double

   - Container의 가로 길이를 설정한다.

4. height -> double 

   - Container의 세로 길이를 설정한다. 

5. margin -> EdgeInsetsGeometry

   - 위젯의 바깥쪽 간격(화면 가장자리로부터 간격)을 조절한다.

6. padding -> EdgeInsetsGeometry

   - 위젯의 안쪽 간격(자식위젯내용과 Container와의 간격)을 조절한다.

7. alignment -> AlignmentGeometry

   - Container 내부에 자식위젯을 정렬한다.(어디에 위치시킬 것인지)

   - 정렬을 하고 나면 Container는 확장되어서 Container의 부모위젯의 너비와 높이를 채운다. (그렇기에 이를 막으려면 width, height 속성 값을 채워줘야한다.)

   - Alignment 클래스를 사용한다. 

     - 3x3 격자가 있다고 가정했을 때 가장위쪽 왼쪽칸이 (1,1)이라고 했을 때 좌표값으로 그 위치로 정렬하게 된다는 의미다.

     ```dart
     alignment: Alignment.center, //Example
     ```

     1. bottomRight - (3, 3)
     2. center - (2, 2)
     3. bottomCenter - (2, 3)
     4. bottomLeft - (1, 3)
     5. centerLeft - (1, 2)
     6. centerRight - (3, 2)
     7. topCenter - (2, 1)
     8. topLeft - (1, 1)
     9. topRight - (3, 1)

8. constraint -> BoxConstrainst

   - 자식 위젯에게 적용할 제약조건

9. transform -> Matrix4

   - Container를 그리기전에 적용할 변환 매트릭스(Container를 변형시키는 역할)

10. decoration -> Decoration

    - Container를 꾸미는 속성(Container의 모양등등)
    
    - 주로 BoxDecoration() 위젯과 맵핑해서 사용함 
    
      > BoxDecoration 속성
    
      1. border
         - Container의 테두리 속성을 다룸(테두리 두께, 색, 스타일)
         - Border() 위젯과 맵핑해서 사용 
      2. borderRadius
         - Container의 테두리를 곡선지게 만드는 속성
         - BorderRadius() 위젯과 맵핑해서 사용
      3. backgroundBlendMode
      4. boxShadow
      5. gradient
      6. image
      7. shape

