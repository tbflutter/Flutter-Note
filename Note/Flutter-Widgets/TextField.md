<h2>Flutter Widgets</h2>

<hr>

<h2>TextField</h2>

<hr>

- 사용자가 입력할 수 있도록 해주는 입력창 위젯이다.
- 모바일 키보드를 이용해 무언가를 입력하게 하는 위젯
- 이 TextField 위젯의 가장 중요한 속성은 controller이다. 
- 이 controller가 이 입력창을 컨트롤하고 접근하고 관리하는 형태이다. 
- controller가 입력창에 무엇을 입력했는지에 대해 추적하고, 값이 있는지 없는지에 대해서도 추적한다. 

<hr>

> TextField 속성 

1. controller -> TextEditingController

   - 컨트롤러를 지정해준다. 그래서 TextEditingController 타입의 컨트롤러 하나를 생성해서 

   - 이 TextField 위젯의 controller 속성에 할당해준다. 그러면 그 컨트롤러는 이 위젯의 관할이 된다. 

   - 이 컨트롤러를 이용해서 입력한 텍스트의 값을 가져오거나 텍스트의 값을 설정할 수가 있다. 

     ```dart
     // 값을 가져오는 방법
     TextEditingController _controller = TextEditingController();// 컨트롤러 생성
     //....
     
     String inputString = _controller.text.toString();
     ```

     ```dart
     // 값을 설정하는 방법
     TextEditingController _controller = TextEditingController();
     //...
     //첫번째 방법
     _controller.text = "This!!";
     
     //두번째 방법
     _controller.value = TextEditingValue(text : "This!!");
     ```

     

2. style -> TextStyle

   - 입력창에 적히는 텍스트의 style을 지정해준다. 일반 Text 위젯에 사용하는 TextStyle과 같음

3. onTap -> void Function()

   - 입력창을 탭했을 때(클릭했을 때) 실행되는 콜백 함수 

4. cursorColor -> Color

   - 입력창을 누르면 깜빡깜빡거리는 cursor, 이 cursor의 색상을 지정해준다.

5. cursorWidth -> double

   - 입력창을 누르면 깜빡깜빡거리는 cursor, 이 cursor의 너비의 길이를 지정해준다.

6. cursorHeight -> double

   - 입력창을 누르면 깜빡깜빡거리는 cursor, 이 cursor의 높이의 길이를 지정해준다.

7. cursorRadius -> Radius

   - 입력창을 누르면 깜빡깜빡거리는 cursor, 이 cursor를 둥글게 만들어준다. 
   - Radius.circular(double) : 원형
   - Radius.elliptical(double) : 타원형

8. decoration -> InputDecoration

   - 이 입력창을 꾸며주는 역할을 한다. 매우 다양함

   - InputDecoration 위젯으로 사용한다.

     > InputDecoration 속성

     1. labelText : 입력창에 대한 설명으로 입력창 상단에 붙는 라벨
     2. hintText : 무엇을 입력해야되는지에 대한 약축설명 텍스트 (입력하면 사라짐)
     3. hintStyle : 위의 hint 텍스트의 style을 지정
     4. icon : 입력창 앞에 붙는 아이콘
     5. prefixIcon : 입력창안에서 접두사로 붙는 아이콘
     6. suffixIcon : 입력창안에서 접미사로 붙는 아이콘

9. onChanged -> void Function(String)

   - 입력창에 입력되는 값이 변경될 때마다 실행되는 콜백 함수
   - 이 콜백 함수의 매개변수에 들어가는 값은 변경된 텍스트를 의미한다. 

10. obscureText -> bool

    - 입력되는 텍스트를 가릴 것인지 말 것인지, true면 가리는 것 -> default 값은 * 모양

11. readOnly -> bool

    - 입력창을 오직 보기만 하도록 할 것인지, 입력도 할 수 있게 할 것인지, true면 오직 읽는 것만 가능

12. maxLength -> int

    - 입력되는 텍스트의 길이의 최대 길이를 지정

13. minLines -> int

    - 입력되는 최소 문장 개수를 의미

14. maxLines -> int

    - 입력되는 최대 문장 개수를 의미

15. enabled -> bool

    - 입력창을 허용할 것인지 말 것인지에 대한 bool 값 지정 



<hr>

```dart
// 예시
TextEditingController _controller = TextEditingController();
TextField(
    controller : _controller,
    onTap : (){
        print("Tapped!");
    },
    onChanged : (text){
        print("current Text : "+text);
    }
    cursorColor : Colors.red,
    cursorWidth : 8.5,
    cursorHeight : 50.0,
   	cursorRadius : Radius.circular(50.0),
    decoration : InputDecoration(
    	labelText : 'Label',
        hintText : 'hint!',
        hintStyle : TextStyle(color : Colors.grey),
        icon : Icon(Icons.add_circle),
        prefixIcon : Icon(Icons.refresh),
        suffixIcon : Icon(Icons.clear)
    )
```