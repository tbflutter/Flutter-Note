<h2>Flutter Widgets</h2>

<hr>

<h2>ListTile</h2>

<hr>

- 일반적으로 일부 텍스트와 선행(leading) 또는 후행(trailing) 아이콘이 포함된 단일 고정 높이를 갖는 행이다.
- 특정한 디자인을 갖는 항목 리스트를 설계할 때 유용하다. 그러한 리스트들의 각 아이템의 디자인 뼈대역할을 한다. 
- 보통 하나의 리스트 항목을 만드려면 다양한 Column, Row, Padding, Container들을 복잡하게 사용되는 반면에
- ListTile은 Material List Items의 사양을 알아서 구현하기 때문에 각 항목안에 어떤 내용을 넣을지만 고민해도 되는 편리한 위젯이다. 
- 그래서 보통 ListView와 함께 쓰인다. ListView의 리스트의 각 항목을 ListTile로 설계함



> 속성 값

| 속성 이름(parameter) | 맵핑 타입                | 설명                                                         |
| -------------------- | ------------------------ | :----------------------------------------------------------- |
| leading              | Widget                   | ListTile의 머리(앞)부분에 들어가는 위젯 아이템               |
| trailing             | Widget                   | ListTile의 꼬리(뒤)부분에 들어가는 위젯 아이템               |
| title                | Widget                   | ListTile의 첫번째 메인 공간(title)                           |
| subtitle             | Widget                   | ListTile의 두번째 메인 공간(subtitle)                        |
| isThreeLine          | bool                     | LisTile의 세번째 공간에도 보여주게 할 것인지 설정            |
| dense                | bool                     | ListTile내에 있는 아이템들을 보다 밀집시키게 할 것인지 설정  |
| onTap                | GestureTapCallback       | ListTile 위젯을 눌렀을 때에 대한 콜백 함수                   |
| onLongPress          | GestureLongPressCallback | ListTile 위젯을 길게 눌렀을 때에 대한 콜백 함수              |
| enabled              | bool                     | ListTile을 상호작용하게 할 것인지 설정                       |
| selected             | bool                     | 이 ListTile이 enabled하면, 아이템들이 동일한 색으로 렌더링됨 |
| shape                | ShapeBorder              | ListTile의 전체적인 모양 설정                                |

