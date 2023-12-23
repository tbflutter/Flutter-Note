<h2>Flutter Basic</h2>

<hr>

> 위젯 - Widget

- 일반적인 위젯은 **독립적으로 실행되는 작은 프로그램**이다. 

  => 스마트폰이나 컴퓨터에 설치하여 날씨나, 뉴스, 생활정보를 보여준다. 

  => 그래픽이나 데이터를 처리하는 기능을 가지고 있다.

- 플러터에서 위젯이란?

  - **UI를 만들고 구성하는 "모든" 요소들**을 말한다. 

  ``` 
  텍스트, 아이콘, 이미지, 버튼등
  ```

  - 위에 적어놓은 것들은 눈에 보이는 것들이다.

  - 이뿐만 아니라 UI 디자인과 관련해서 레이아웃을 돕는 요소들까지도 위젯이라고 한다. 

  ```
  정중앙 배치 - Center
  세로로 나열 - Column
  가로로 나열 - Row
  세세한 위치 설정 - Padding 등등
  ```

  - 즉, 플러터에서는 모든 것이 위젯이다. 

    화면, 구성요소 하나하나 다 위젯이고, 위젯들이 쌓여서 만들어진 앱 그 자체도 위젯이다. 

  

- 플러터는 이 모든 것을 오직 코드 하나로 위젯들을 나열한다.  

  이로 인해 직관적으로 작성할 수 있을 것이다. 

- A widget is an immutable description of part of a user interface 

  => 플러터의 모든 위젯은 불변성을 가진다. 

- 하지만 UI는 가변적이다. 사용자와 수많은 다양한 인터렉션을 통해 데이터가 변경될 것이고, UI는 반드시 업데이트 되어야한다. 

- 플러터는 어떻게 UI를 업데이트할까?

> 위젯의 타입 

- 플러터에서는 "state"라는 개념을 사용한다. 

- state는 앱에서 사용되는 data들을 의미한다. 

- State란 UI가 변경되도록 영향을 미치는 데이터이다. 

  이런 state는 2개로 나누어질 수가 있다. 

  1. App state

     앱 전반에 걸쳐 사용되는 data

     앱 이곳저곳 전반적으로 다 필요하고, 한쪽에서 app state를 변경하면 다른 쪽에서도 data 변경을 반영해야함

  2. Widget state

     widget 내부에서만 사용되는 data

     위젯 내부에서만 사용된 따로 공유하거나 할 필요가 없음

- Stateless Widget - 상태가 없는 정적인 위젯, 이전 상호작용의 어떠한 값도 저장하지 않음 

   => 움직임이나 변화가 전혀 없는 상태

  1. 스크린상에서 존재만 할 뿐 아무것도 하지 않음

  2. 어떠한 실시간 데이터도 저장하지 않음

  3. 어떤 변화(모양, 상태)를 유발시키는 value 값을 가지지 않음

     => 텍스트나 이미지등은 화면속에 존재만 할 뿐 어떤 모양의 변화나 움직임이 없다.

- Stateful Widget - 변화가 있는 동적인 위젯, Value 값을 지속적으로 추적 보존함

  => 항상 움직이거나 무언가 변화가 생기는 상태 

  1. 사용자의 상호작용에 따라서 모양이 바뀜

  2. 데이터를 받게 되었을 때 모양이 바뀜

     => 사용자가 TextField(사용자가 입력하는 곳)에서 무언가를 입력할 때마다 문자 정보를 표시하는 과정에서 지속적으로 필드의 내용이 바뀌게 됨
  
- build() 메소드

   - build 메소드는 StatelessWidget과 StatefulWidget(정확히는 연결된 State 클래스)에서 구현이 되며 화면을 구성할 UI들을 구현하는 메소드이다. 

   - 화면이 출력될 때 build 메소드가 호출되면서 build 메소드 내부에 구현한 UI 위젯들이 화면에 출력된다. 

     1. StatelessWidget은 변화가 필요없는 화면을 구성할 때 사용하는 위젯 클래스이며

        그렇기 때문에 build 메소드는 딱 한번만 호출된다. 

     2. StatefulWidget은 화면의 구성이 상태 변화에 따라 재구성되어야 할 때 사용함

        - 상태 변경은 setState() 메소드를 이용해서 변경해야한다.
        - setState() 메소드가 호출될 때마다 build 메소드를 재호출하여 화면을 다시 그린다. 

   - 플러터 기본 프로젝트인 카운팅 앱에서 StatelessWidget으로 만들었을 때 

     버튼을 눌렀을 때 실제 count 값은 바뀌지만 화면의 Text에는 반영되지 않는다. 

     (StatefulWidget는 setState() 메소드가 없다면 StatelessWidget과 똑같이 변화를 반영하지 않는다.)

1. Stateless Widget 

   - State가 변하지 않는 위젯이다.

   - 빌드 타이밍에 부모로부터 받은 정보로에만 의존하는 요소이다. 

   - 이는 한번 빌드(객체화)되면 신경쓰지 않아도 된다는 의미이다. 

   - 그렇기에 한번 적용되면 re-build되지 않는 한 변하지 않는다.(빌드될 때 딱 한번 그려짐)

   => 이벤트나 사용자 인터렉션에 의해 그려지지 않음

   **"변경될 data가 없다."** 라는 의미지 data가 없다는 것은 아니다. 

   (내부의 data가 immutable함)

   몇가지 parameter를 생성자에 전달한다. 그러나 이 parameter는 이후에 변경되지 않는다. 

   - build 메소드만 오버라이딩하면 된다. 

     ```dart
     class Name extends StatelessWidget{
         @override
         Widget build(BuildContext context){
             return ~~;
         }
     }
     ```

     - Stateless 위젯은 rebuild만을 통해서 새로운 State를 적용할 수 있다.

       - 주의! reload와 rebuild는 엄연히 다른 것이다.

         reload : 자동차 타이어, 핸들 다른 상품으로 구입(이전 프레임에서 변경된 사항 적용 하여 실행)

         rebuild : 새로운 자동차를 구입 (처음부터 실행)

   

2. Stateful Widget

   - State가 변하는 위젯이다.

   - 내부에 data가 변경될 경우, 변경된 사항을 화면을 다시 그려서 반영시킨다. 

   - StatefulWidget은 2개의 클래스로 구성되어 사용된다. 

     => 바뀌는 부분과 바뀌지 않는 부분으로 구성된다. 

     - 이는 위젯의 state가 바뀌고 우리가 변경된 사항을 화면에 그리라고 명령하면

       플러터는 기존 위젯을 날려버리고 업데이트된 부분을 반영해서 위젯을 다시 그리게 된다. 

     -  다시 그릴 때, 해당 위젯을 완전히 날려버리고 다시 그린다고 하면 data가 함께 날라가기 때문에 안된다. 

     - Data는 보존하면서 해당 data를 위젯에 입혀서 다시 만들어내게 된다.

     - 그러면 계속해서 사라지지 않고 data를 들고 있을 class가 필요한 것이다. 

     - 그 class는 StatefulWidget은 가지고 있어야하기 때문에 2개의 클래스로 구성된다. 

     ```dart
     class Test extends StatefulWidget{
         @override
         _TestState createState() => _TestState();
     }
     class _TestState extends State<Test>{
         @override
         Widget build(BuildContext context){
             return ~~;
         }
     }
     ```

     -  두 위젯 Stateful, Stateless 위젯의 공통점은 생성자를 통해 외부에서 데이터가 입력이 되면 그 결과를 반영하기 위하여 build 메소드가 호출이 되면서 위젯이 rebuild 되고 필요한 부분의 UI를 다시 Rendering한다.

     - Stateless와의 결정적인 차이점은 내부에 State라는 또 다른 클래스가 있다. 

       => build 메소드는 State 클래스가 가지고 있다. 

       그래서 엄밀히 말하면 Stateful 위젯에서 rebuild를 초래하는 것은 결과적으로 State 클래스이며, 이를 통해서 화면을 다시 Rendering해준다. 

       - rebuild되는 경우
         1. Stateless처럼 Child 위젯등의 생성자를 통해 데이터가 전달될 때
         2. Internal state가 바뀔 때

     -  그래서 대부분의 경우 StatefulWidget클래스는 State 클래스를 생성시키는 기능만 하는 것이 다인 경우가 많다. (createState()메소드를 통해)

     - 우리가 보게 되는 것은 Test클래스 객체인데 이 객체는 실은 변할 수 없는 객체이다. 

       => StatefulWidget은 Widget클래스를 상속하고 있다. 

       그런데 Widget클래스는 기본적으로 immutable하다. 즉, 한번 생성하면 State가 변하지 않는다는 것이다. 그렇기에 StatefulWidget을 상속받은 커스텀위젯클래스는 

       <i>**Stateful위젯임과 동시에 Stateless위젯처럼 immutable한 위젯이다. **</i>

       하지만 Stateful위젯은 State의 변화를 반영해야한다. 그렇기 때문에 두개의 클래스로 나누어서 Stateful위젯을 상속받은 클래스는 **immutable**한 특징을 유지하고

       그 아래에  State클래스를 상속받은 클래스는 **mutable**한 특징을 대신하게 한 것이다. 

     - data에 변경사항이 생겨서 다시 위젯을 그리게 되면 우리에게 보여진 Test클래스의 객체는 사라지고 다시 _TestState클래스에서 변화가 반영된 data를 기반으로 위젯을 만들어서 Test에게 올려보내고 Test객체는 새로 받은 위젯을 우리에게 보여준다.

     - Test클래스와 _TestState클래스는 연결되어 있어야한다.

     - 연결시키기 위해서는 각 클래스에 코드를 넣어줘야한다.

     - Test클래스에서는 createState() 메소드를 통해 _TestState 클래스와 연결하며

       _TestState는 상속시 State&lt;Test&gt; 제네릭 클래스를 통해 연결한다.

       

     - createState() 
     
       ```dart
       @override
       State<StatefulWidget> createState(){
           return null;
    }
       ```

       State 타입으로 지정되어 있고 제네릭타입으로 StatefulWidget이 지정되어있다.

       createState()는 반드시 State타입의 객체를 리턴해야하는데

       결국 StatefulWidget타입만이 올수있는 객체이여야 한다. 

       이는 State클래스와 연결했던 것과 유사한 느낌이다. 

       - 그리고 createState() 메소드는 Stateful위젯이 생성될 때 마다 호출되는 메소드이다. 그렇기에 State 클래스를 상속하면서 연결된 제네릭 타입의 객체를 리턴해야한다. 

     - State를 변화시키려면 build메소드를 호출해서 위젯을 rebuild하는 방법밖에 없다.

     - 그러나 단순히 버튼만 눌렀다고 build 메소드를 호출할 수 없다.

     - 그래서 플러터는 build메소드를 호출해줄수있는 setState 메소드를 가지고 있다.

       - setState()

         - 역할
     
      1. 매개변수로 전달된 함수를 호출하는 것
           2. build 메소드를 호출하는 것

         - state가 변했고 이 변화를 반영해서 다시 rebuild를 해야하기 때문 

         - setState가 표시해준 위젯들을 "dirty"라고 표현함

           ```
       setState() 안에 정의된 함수에 의해 값이 변경되는 변수 A, setState가 실행되면 
           해당 build 메소드내에서 A가 사용되는 모든 곳을 dirty로 표시하고
           setState()에 의해 build 메소드가 재실행되면 dirty로 표시된 곳만을 찾아서 UI를 업데이트함
           ```
         
           
         
         - 두개의 로봇이 있는데, 변신 로봇이 있는데 변신 과정에서 계속 state가 바뀌어야 하고 이를 반영하려면 보다 많은 부품과 비용이 들어간다.
         
         - 플러터 입장에서 state 객체는 변신로봇에 해당된다.
         
         - 그래서 state가 변한 state 객체를 비용이 싼 stateful 위젯으로 만들어서 계속 rebuild함 

> 위젯 트리 - Widget tree

- 모든 것이 위젯으로 구성됨 
- 그래서 위젯들을 나열해서 앱을 만들어가는 과정에서 하나의 계층구조를 가지게 된다.
- 이것을 트리(tree) 구조로 표현할 수 있다. 
- 안드로이드에서는 이러한 UI들의 계층구조를 XML파일에서 가질 수 있었다. 

- 한 위젯내에 얼마든지 다른 위젯들이 포함될 수 있다. 

  => 위젯은 부모(Parent)위젯과 자식(Child)위젯으로 구성됨 

- 그래서 Parent 위젯을 위젯을 내포한다는 의미로 "Widget Container"라고도 한다. 

```
						MyApp
						  |
					  MaterialApp
					      |
					  MyHomePage
					      |
					-- Scaffold --
					|            |
				  AppBar        Center
				    |             |
				   Text 	  --Column--
				   			 |    |    |
				   		Image TextField Button
```

1. MyApp 

   - 최상위 위치에는 MyApp()이 존재한다. 바로 앱의 root 위젯이자 앱의 시작점이다. 
   - 꼭 이름이 MyApp일 필요는 없다. 
   - 일종의 커스텀 위젯으로 이곳에서 Material App이라는 것이 빌드된다. 

2. MaterialApp

   - 위에서 이어서 말하면 트리상에서 두번째 위치에는 root 위젯에서 빌드된 MaterialApp이 있다.

   - 실질적으로 MaterialApp위젯이 전체 앱을 감싸고 있는 위젯이다. 

   - 이 MaterialApp위젯을 통해서 플러터 sdk에서 제공하는 위젯들 모든 것들을 사용할 수 있게된다.

     

   - MaterialApp은 안드로이드의 Material Design을 채택한 앱이다. 

     이 MaterialApp을 만들기 위해서는 내용을 Scaffold로 감싸줘야한다. 

3. MyHomePage

   - 이것도 커스텀 위젯으로 꼭 이름이 MyHomePage일 필요는 없다.
   - 여기서 본격적으로 앱의 디자인과 기능들이 만들어진다. 

4. ★ Scaffold

   - 가장 중요한 위젯으로 앱 화면과 기능을 구성하기 위한 빈 페이지를 준비해주는 위젯이다. 
   - Scaffold 위젯 밑으로 본격적으로 UI와 관련해서 보여지는 모든 앱의 구성요소들(이미지, 버튼, 텍스트, Center, Column, Padding 등)이 사용된다. 



<hr>

<h2>How flutter Renders Widgets </h2>

- 위의 내용 다음과 연관

```dart
Widget tree <---- Element tree ----> Render tree
MyApp Stateful <-- MyApp Stateful --> Render object
    Widget             Element 
                          |
                       MyAppState
    				   object
```

- MyAppState 객체는 위젯트리상의 MyApp Stateful 위젯과 간접적으로 연결되있다. 
- MyAppState element는 위젯관련 중요 정보를 갖고있지만 이번에는 메모리 상에서 그 어디에도 종속되지 않은 독립된 객체로써 MyAppState 객체에 대한 정보도 가지게 된다. 
- setState메소드가 호출되고 build메소드로 인해서 State 객체가 rebuild되면서 최종적으로 새로운 State를 반영한 새로운 MyApp Stateful 위젯이 rebuild된다.
- MyApp Stateful element에 연결되있는 MyAppState 객체에 새로운 State가 저장이 되고 이제 MyAppState 객체는 새롭게 rebuild된 MyApp Stateful 위젯을 가리키게 된다.   

