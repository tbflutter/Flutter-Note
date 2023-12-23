<h2>Analysis of Flutter Structure</h2>

<hr>

- 플러터 프로젝트를 처음 생성할 때 나오는 플러터 기본 프로그램인 카운팅 프로그램에 대해 전체적인 코드 분석

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;
  @override
  _MyHomePageState createState() => _MyHomePageState();
}
class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text(widget.title),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            Text(
              'You have pushed the button this many times:',
            ),
            Text(
              '$_counter',
              style: Theme.of(context).textTheme.headline4,
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: Icon(Icons.add),
      ),
    );
  }
}
```

1. 플러터 앱을 만들때 제일 먼저 해야할일이 "flutter material" 라이브러리를 반드시 import해야한다. 

   - 라이브러리를 반드시 가져와야만 플러터 프레임워크 sdk에 포함된 모든 기본 위젯과 머티리얼 디자인 테마 요소들을 사용할 수 있다.

   - Material Design이란 모바일, 데스크탑 그외 다양한 디바이스를 아우르는 일관된 디자인을 위해서 구글이 제공한 가이드라인

```dart
import 'package:flutter/material.dart';
```



2. 메인함수 - 앱의 시작점 

   - 컴파일러가 작성한 코드를 변환하는 작업할 때 가장 먼저 이 메인함수를 참조한다. 

   ```dart
   void main(){
       
   }
   ```

3.  runApp(app) 함수 

   - 최상위 함수로 argument로 들어가는 위젯을 최초로 불러온 위젯인 만큼

     위젯 트리에서 최상위에 위치하는 위젯이고, 스크린 레이아웃을 최초로 빌드하여 화면에 뿌려주는 역할을 함

   - 반드시 위젯을 argument로 가져야한다. 

   - argument로 가지는 위젯은 Root 위젯이 되어 위젯트리의 가장 최상위에 위치하게 된다.

   - 안드로이드에서의 setContentView()와 같은 역할을 한다. 

   ```dart
   void main() => runApp(MyApp());
   // =>은 함수가 한줄일 때 축약해서 쓸 수 있는 기법 
   //또한 MyApp()을 argument로 넣는데 이는 MyApp() 객체를 넣는 것이다. 
   // 이는 new를 생략한 것이다.(모든 위젯 클래스는 이렇게 생략하여 객체를 넣음)
   ```

   - MyApp은 플러터에서 제공하는 기본 위젯이 아니라 사용자가 직접 만들어야 하는

     Custom Widget이다. (이름은 사용자의 마음대로)

4. MyApp 위젯 클래스

   - 위에서 메인함수에서 runApp()함수를 통해 Root 위젯을 실행하는데 그 Root 위젯에 해당하는 위젯클래스이다. 

   ```dart
   class MyApp extends StatelessWidget {
     @override
     Widget build(BuildContext context) {
       return MaterialApp(
         title: 'Flutter Demo',
         theme: ThemeData(
           primarySwatch: Colors.blue,
           visualDensity: VisualDensity.adaptivePlatformDensity,
         ),
         home: MyHomePage(title: 'Flutter Demo Home Page'),
       );
     }
   }
   ```

   - StatelessWidget을 상속받아 리턴타입이 Widget인 build 메소드를 오버라이딩한다. 

   - 이 build메소드는 MaterialApp 위젯을 리턴한다. 

   - MaterialApp 객체 즉, 괄호안에 있는 값들은 속성(Attribute)으로 현재 필요한 값을 넣어준다. => Named Argument 기법에 따라서 속성 이름을 명시하고 값을 부여한다. 

     -> 각 속성마다 콤마(,)로 구분한다. 

     => 이 속성들은각 위젯 클래스의 생성자의 파라미터이다. 

     1. title이라는 속성에 'Flutter Demo'라는 값을 넣음

     2. theme이라는 속성에 ThemeData() 객체를 넣음

        1. primarySwatch속성에 Colors.blue 값 넣음
        2. viualDensity 속성에 VisualDensity.adaptivePlatformDensity 값 넣음

     3. home이라는 속성에 MyHomePage() 위젯 객체를 넣음

        - home 속성은 MaterialApp의 내용에 해당한다. (화면을 구성하는)

        1. title속성에 'Flutter Demo Home Page' 값을 넣음 

5. MyHomePage 위젯 클래스

   - MyApp 클래스에서 MaterialApp을 사용할 때 home 속성에 MyHomePage() 객체를 argument로 넣었다. 

   ```dart
   class MyHomePage extends StatefulWidget {
     MyHomePage({Key key, this.title}) : super(key: key);
     final String title;
     @override
     _MyHomePageState createState() => _MyHomePageState();
   }
   ```

   - MyHomePage 클래스의 생성자로 값을 받아온다. 
   - 생성자에서 Named Argument를 사용한다. key값과 title 값을 Named Argument로 지정하고 부모클래스의 생성자도 호출하여 key값을 argument로 넘겨준다.
   - 생성자로부터 받은 title값을 멤버 변수(final String title)인 title에게 값을 할당해준다. 
   - createState() 메소드를 오버라이딩하여 연결된 State 클래스의 객체를 생성한다. 

6. MyHomePageState 클래스

   - createState() 메소드를 통해 MyHomePageState 클래스의 객체를 생성한다.
   - 이 클래스는 private 이다. (클래스 이름 앞에 _(underscore)가 붙음)

   ```dart
   class _MyHomePageState extends State<MyHomePage> {
     int _counter = 0;
     void _incrementCounter() {
       setState(() {
         _counter++;
       });
     }
     @override
     Widget build(BuildContext context) {
       return Scaffold(
         appBar: AppBar(
           title: Text(widget.title),
         ),
         body: Center(
           child: Column(
             mainAxisAlignment: MainAxisAlignment.center,
             children: <Widget>[
               Text(
                 'You have pushed the button this many times:',
               ),
               Text(
                 '$_counter',
                 style: Theme.of(context).textTheme.headline4,
               ),
             ],
           ),
         ),
         floatingActionButton: FloatingActionButton(
           onPressed: _incrementCounter,
           tooltip: 'Increment',
           child: Icon(Icons.add),
         ),
       );
     }
   }
   ```

   - 연결할 StatefulWidget 클래스의 제네릭 타입으로 State 클래스를 상속받는다. 

   - private로 int타입으로 counter 변수를 0을 초기값으로 만든다. 

   - StatefulWidget의 build 메소드는 State클래스가 가지고 있다. 

     그렇기에 build 메소드를 오버라이딩한다.

   - 그리고 Scaffold를 리턴한다. Scaffold는 앱에서 디자인적 부분에서의 뼈대역할을 한다.

   - Scaffold의 속성에 각각 값을 넣어준다. 

     1. appBar : 화면의 상단 바(Bar)를 보여줌

        - AppBar() 위젯을 title속성에 Text()위젯에 보여줄 문자열을 작성한 후 넣음

          => State 클래스내에서 statefulwidget 클래스에 존재하는 멤버에 접근하고 싶으면 **"widget"**으로 접근한다. Ex) widget.title 

     2. body : appBar 구역 제외한 나머지 전체 내용 부분

        - Center() 위젯으로 argument로 들어가는 위젯들을 가운데로 위치시킴

          - child : 말 그대로 자식 위젯 "하나"를 의미한다. 

        - Center위젯의 자식을 Column 위젯으로 넣음

        - Column()위젯은 자식으로 들어가는 모든 위젯들을 세로로 위치 시킴(안드로이드에서 LinearLayout의 vertical 느낌)

          - mainAxisAlignment : 현재 위젯의 중심 축을 기준으로 각 위젯들을 어떻게 정렬 시킬 것인지에 대한 속성

          - children : 말 그대로 자식"들" 위젯이다. 하나 이상의 자식 위젯이 올수있다. 

            => children : &lt;Widget&gt;[]으로 표현되는데 여러개의 위젯들이 배열(리스트)로 올 수 있다는 의미이고,  각 자식 위젯들은 콤마(,)로 구분한다. 

          - Column의 첫번째 자식 위젯으로  Text() 위젯으로 화면에 보여줄 내용을 함께 채움

          - 또 Column의 두번째 자식 위젯으로 Text() 위젯으로 화면에 보여줄 내용을 채우는데 State 클래스의 멤버 변수 값을 출력한다. Text() 위젯에 들어가는 내용은 String 형식이여야 하므로 형식 지정을 위해 $을 사용하여 

            '$_counter'로 문자열에 변수를 사용할 수 있도록 한다. 

          - 그리고 Text는 스타일을 지정할 수 있는데 

            이는 Text()위젯의 속성 중 "style"이 있는데, 이 style 속성에 TextStyle() 위젯 객체를 넣어줘야한다. (근데 위의 예시는 Theme을 주었다.)

     3. floatingActionButton : 하단에 존재하는 떠있는 효과를 가지고 있는 버튼

        - FloatingActionButton() 위젯으로 넣어준다. 

          - onPressed : 버튼을 눌렀을 때 어떤 액션을 취할지에 대해 정의하는 곳

            _incrementCounter 메소드를 호출하는 형식이다. 

            ```dart
            void _incrementCounter() {
                setState(() {
                  _counter++;
                });
              }
            ```

            - _incrementCounter 메소드는 현재 카운터 값을 하나 증가시키고 

            - setState() 메소드를 통해 상태변화를 반영시키는 기능을 가진다. 

            - onPressed 속성은 이렇게 미리 정의한 메소드를 줄 수도 있지만(재사용을 위한) 익명 함수로도 부여할 수 있다.

              ``` dart
              onPressed : (){
              	//동작
              }
              ```

          - tooltip : 버튼에 hovering(갖다 대었을 때)했을 때 툴팁으로 등장하는 문구

          - child : 하위(자식) 위젯을 "하나" 가지는 속성

            - Icon 위젯에 플러터에서 기본적으로 제공하는 아이콘 이미지들을

              Icons에서 불러와서 사용함 

              (Icons.add를 사용함 => 더하기 모양의 아이콘을 사용하겠다라는 의미)

<hr>

> 실행 순서

runApp() -> MyApp()의 build() -> MyHomePage의 생성자 및 createState() 메소드 -> MyHomePageState의 build() 메소드 -> 버튼을 눌렀다면 setState() 호출 및 build() 메소드 호출