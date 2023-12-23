<h2>Flutter Widget</h2>

<hr>

<h2>Stack</h2>

<hr>

- Multi-child layout widget이다. 
- Row와 Column 혹은 Container기반의 레이아웃의 갑갑함을 해결하고
- 그리고 중복이 가능한 위젯을 지원한다.
- 즉, 이름처럼 위젯위에 위젯을 쌓고, 쌓는 중첩된 레이아웃(레이아웃끼리 겹칠 수 있는)을 지원해주는 위젯이다. 
- 이는 간단하게 위젯들을 마음대로 결합하고 위치를 변경시킬 수 있는 편리한 위젯이다.
- Stack 위젯의 children 속성을 통해 위젯들을 배치할 수 있다.
- 그럴 때, children 속성의 리스트에서 위에 있는(앞에 있는) 위젯은 아래로 깔리게 되고 그 다음 순차적으로 그 위에 위젯들이 깔리는 형식이다. 
- 위치 정렬을 설정해주지 않으면 각 하위 위젯요소들은 **topStart**로 default로 설정된다. 
- 이를 위해 **Positioned** 위젯과 같이 사용된다.

=> 단순히 Stack만 사용하면 보기 안좋다. 그렇기에 Stack의 하위 위젯으로 존재하는 위젯들을 어떻게 배치하는가에 대한 위젯이다. 

> 속성 값

1. children -> widget[]

   - Stack 위젯의 하위 요소 위젯들

2. fit -> StackFit

   - Stack 위젯에서 위치가 지정되지 않은 자식 위젯의 크기를 조정하는 속성

     **StackFit.loose** :  부모로부터 Stack에 전달된 제약 조건이 느슨하게 된다. 

     <i>Stack 위젯에 350x600으로 강제하는 제약 조건이 있는 경우, 이는 Stack의 위치가 정해지지 않은 하위 위젯들에게 0에서 350까지의 폭과 0에서 600까지의 높이를 허용한다.</i>

     **StackFit.expand** : 부모로부터 Stack에 전달된 제약 조건은 허용되는 최대 크기로 조여진다.

     <i>Stack 위젯의 너비가 10에서 100 사이이고 높이가 0에서 600 사이인 느슨한 제약 조건이 있는 경우, Stack의 위치가 정해지지 않은 하위 위젯들은 모두 너비가 100픽셀이고 높이가 600픽셀이다.</i>

3. alignment-> AlignmentDirectional

   - Stack 위젯의 하위 요소들을 정렬하는 기준

     - 3x3 격자가 있다고 가정했을 때 가장위쪽 왼쪽칸이 (1,1)이라고 했을 때 좌표값으로 그 위치로 정렬하게 된다는 의미다.

     ```dart
     alignment: AlignmentDirectional.center, //Example
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

4. overflow -> Overflow  (Deprecated 됨 clipBehavior 속성으로 대체)

   - Stack 위젯의 경계선을 벗어나게 할지 말지 제어하는 속성

     **Overflow.clip** : 벗어나지 못하게 함 (벗어나면 짤림)

     **Overflow.visible** : 벗어나도록 함 

5. clipBehavior-> Clip

   - overflow 속성의 대체(?) 즉, 기능은 비슷함

     **Clip.none** : Overflow.visible과 같음

     **Clip.hardEdge** : Overflow.clip과 같음 

<hr>

> Positioned 위젯

- 사용방법은 Stack의 하위 위젯을 Positioned 위젯으로 감싸면 된다.
- Stack 내에서 특정 하위 요소에 특정 위치를 지정할 수 있다.
- width와 height 속성 빼고 나머지 속성들은 padding 효과를 지닌다 속성에 대한 방향으로부터 얼마나 떨어질지에 대한 값을 의미한다.
- left, right, top, bottom 속성 값은 음수 값도 가능해서 반대 방향으로 떨어지게 만들 수도 있다. 

1. left
2. right
3. top
4. bottom
5. height
   - Positioned 위젯으로 감싼 하위 위젯의 세로(높이)길이 설정; **하위위젯의 크기가 설정되있다 하더라도 이 설정값으로 뭉갬**
6. width
   - Positioned 위젯으로 감싼 하위 위젯의 가로(너비)길이 설정; **하위위젯의 크기가 설정되있다 하더라도 이 설정값으로 뭉갬**
7. child
   - Container와 같이 위치, 크기 설정을 도와줄 하위 위젯

+ Postioned.fill()를 이용해 Stack의 공간만큼을 채울 수 있다.
+ 또한 이 Positioned.fill()의 속성값에는 left, right, top, bottom 속성이 존재하여 떨어지게 하는 효과도 줄 수 있다.  
+ 이는 먼저 공간을 확 채운 뒤, 그 다음에 방향 속성에 대한 값을 적용한다. 