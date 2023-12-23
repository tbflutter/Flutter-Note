<h2>Flutter Widgets</h2>

<hr>

- <h2>Scaffold</h2>

<hr>

- <i>Implements the basic material design visual layout structure.</i>

  => 기본적인 material design의 시각적인 레이아웃 구조를 실행함. 

- 기존 안드로이드 개발 환경에서는 Java나 Kotlin으로 로직을 구성하고

  UI를 담당하는 XML파일과 Mapping을 해주어야 했다.

  

- Scaffold란 발판, 비계(건설에서의 임시 발판)을 뜻한다. 

- 이는 기본적인 앱에서 디자인적인 뼈대를 구성해주는 위젯이다. 

  - 아래는 Scaffold의 생성자 즉, 속성값들을 보여준다. 

```dart
class Scaffold extends StatefulWidget {
  /// Creates a visual scaffold for material design widgets.
  const Scaffold({
    Key key,
    this.appBar,
    this.body,
    this.floatingActionButton,
    this.floatingActionButtonLocation,
    this.floatingActionButtonAnimator,
    this.persistentFooterButtons,
    this.drawer,
    this.endDrawer,
    this.bottomNavigationBar,
    this.bottomSheet,
    this.backgroundColor,
    this.resizeToAvoidBottomPadding,
    this.resizeToAvoidBottomInset,
    this.primary = true,
    this.drawerDragStartBehavior = DragStartBehavior.start,
    this.extendBody = false,
    this.extendBodyBehindAppBar = false,
    this.drawerScrimColor,
    this.drawerEdgeDragWidth,
    this.drawerEnableOpenDragGesture = true,
    this.endDrawerEnableOpenDragGesture = true,
  }) : assert(primary != null),
       assert(extendBody != null),
       assert(extendBodyBehindAppBar != null),
       assert(drawerDragStartBehavior != null),
       super(key: key);
...
```



<hr>

> 속성 값 - 화살표는 맵핑 타입

1. appBar -> PreferredSizeWidget

   - 주로 창 이름을 담당하고 있다.

   - 앱의 최상단에 위치해있는 바(Bar)를 의미한다.  

   - AppBar 클래스와 맵핑되어서 사용된다. 

     > AppBar 클래스 속성

     1. title -> Widget
        - 앱바에서 보여지는 기본 위젯 (보통 주제나 이름등 문자열- Text 위젯)
     2. backgroundColor -> Color
        - 앱바의 배경색
     3. centerTitle -> bool
        - 타이틀의 위치를 중심으로 옮길지 말지 결정 
     4. elevation -> double
        - 부모를 기준으로 앱바를 배치할 z 좌표 값 (얼마나 띄워지는 효과를 줄지)
     5. leading -> Widget
        - 앱바에서 아이콘 버튼이나 간단한 위젯을 왼쪽에 배치할 때(보통 이미지버튼, 이미지?)
     6. actions -> List&lt;Widget&gt;
        - 앱바에서 여러개의 아이콘 버튼등을 오른쪽에 배치할 때 사용(보통 이미지버튼)

2. body -> Widget

   - 가운데 영역을 의미한다. 본문내용, 주요내용을 담는 역할을 하고있다.
   - 일반적인 가운데의 내용을 넣으면 됨 (너무 광범위)

3. bottomNavigationBar -> Widget

   - 앱의 하단에 표시되는 하단 메뉴(탐색 모음)이다.
   - BottomNavigationBar 클래스와 맵핑되어서 사용된다. 

   > BottomNavigationBar 클래스 속성값

   - 속성값으로 items : [ ]를 가지는데 이안에 리스트처럼 여러개의 Item들을 넣을 수 있다.

   - 각 아이템은 BottomNavigationBarItem 클래스로 가지게 된다.

     - BottomNavigationBarItem 속성값
       1. icon -> Icon : 해당하는 칸에 아이콘을 부여
       2. label -> String : 해당하는 칸에 간단한 설명을 위한 텍스트 

     1. currentIndex
     2. selecteditemColor
     3. onTap
     4. type
     5. unselectedItemColor
     6. 

4. floatingActionButton -> Widget

   - 오른 하단의 body위에 떠있는 효과를 주는 버튼
   - FloatingActionButton 클래스와 맵핑되어서 사용된다. 

   > FloatingActionButton 클래스 속성

   1. onPressed -> VoidCallback
      - 버튼을 클릭했을 때 동작하는 콜백함수 
   2. backgroundColor -> Color
      - 버튼의 배경색
   3. tooltip -> String
      - 버튼을 꾹 눌렀을 때 간단한 설명
   4. child -> Widget
      - 버튼의 하위 자식 (보통 아이콘)
   5. shape -> ShapeBorder
      - 버튼의 모양

5. floatingActionButtonLocation -> FloatingActionButtonLocation

   - floatingActionButton의 위치를 설정한다. 
   - FloatingActionButtonLocation 클래스와 맵핑되어서 사용한다.

   > FloatingActionButtonLocation 멤버 변수

   - FloatingActionButton.start 이런식으로 사용함
   - Docked가 붙은 속성값은 하단 탭메뉴등이 있을 경우 겹치게 위치를 둔다
   - Top이 붙은 속성값은 버튼이 상단에 위치하게 된다. 
   - 앱 기준 가로방향으로 start - center - end 순으로 배치된다. 

   1. centerDocked 
   2. centerFloat
   3. centerTop
   4. endDocked
   5. endFloat
   6. endTop
   7. startDocked
   8. startFloat
   9. startTop

6. drawer -> Widget

   - 왼쪽에서 오른쪽으로 슬라이드해서 측면에 표시되는 패널 메뉴 

7. backgroundColor -> Color

   - 전체 Scaffold의 기초가 되는 material 위젯의 배경 색상이다. 



