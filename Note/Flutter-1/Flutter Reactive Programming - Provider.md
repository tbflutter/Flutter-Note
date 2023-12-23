<h2>Flutter Reactive Programming</h2>

<hr>

<h2>2. Provider</h2>

<hr>

- Provider는 말 그대로 제공을 하는 것이다. 
- 바로 "데이터"를 제공하는 것인데, 
- 특정 위치 혹은 위젯에 Provider를 제공을 해주면, 그 제공된 데이터는 그 제공 받은 위젯안(아래)로 모두 접근이 가능하고 사용이 가능하다. 
- 강물이 흐르는 것처럼 위젯트리 상에서 데이터를 Provider가 제공한 위치로 부터 아래로 flow down하는 것이다. 
- 이를 통해 하나의 데이터를 공유할 수 있다는 것이다. 어느 한 부분이 변경되면 다른 부분도 Reactive하게 변경됨



- Provider가 제공한 데이터를 어떻게 받을 수 있는지 방법이 두 가지가 있다.
  1. 첫번째는 Consumer&lt;T&gt;를 사용하는 것
     - Consumer는 context 접근이 불가능할 때 사용하면 된다. (Consumer는 context를 안줘도 알아서 context를 찾아서 builder에서 context와 데이터와 전달해줄 위젯도 만들어준다.)
  2. 두번째는 Provider.of&lt;T&gt;를 사용하는 것
     - Provider.of는 context가 있으면 간단하게 직접적으로 바로 받아서 사용할 수 있다. 

<hr>

> ChangeNotifierProvider 클래스

- 데이터의 상태에 대해 Reative하게 사용하고 싶을 때 사용함

<i>A specification of ListenableProvider for ChangeNotifier. it will automatically call ChangeNotifier.dispose when needed</i>



- 특정 위치의 위젯트리에서 Provider가 제공한 데이터는 그 위젯의 아래에서만 데이터를 제공받을 수 있다. 



- 사용방법
- 먼저 데이터를 정의해야한다. 
  - 데이터를 정의할 클래스에 **ChangeNotifier** 클래스를 with키워드로 Mixin을 한다. 

```dart
class Counter with ChangeNotifier{
    int _counter = 0; // 실제로 사용할 데이터
    
    get counter => _counter; // 현재 데이터의 상태를 받아오기 위함
    
    void increment(){ // 무언가 데이터를 다루는 함수 - 변경을 위한 
        _counter++;
        notifyListeners(); // 변경되었다는 것을 Provider가 제공한 데이터에게 알림
        // 지금 데이터가 변경되었으니, 변경된 데이터에 따라 처리를 해라 라는 의미
    }
}
```



- 제공할 데이터는 .dart 파일에서 사용될 것인지 안되는 것인지 판단하고 그 파일의 위젯 클래스에 제공해도 되지만, 
- 대체로 Provider는 가장 최상단에 주는 것이 좋을 것 같다. 아래로 flow down 하기 때문에 가장 위에서 아래로 흐를 수 있도록 가장 최상단에 주는 것이 좋다. (MaterialApp의 위)



- 데이터 제공하기 - 1번 방법

```dart
class MyApp extends StatelessWidget{
    @override
    Widget build(BuildContext context){
		return ChangeNotifierProvider<Counter>.value(
            value : Counter(), // 처음에 사용할 데이터의 object를 생성해서 전달을 한다. 
        	child : MaterialApp(...)
        )
    }
}
```

- 데이터 제공하기 - 2번 방법

```dart
class MyApp extends StatelessWidget{
    @override
    Widget build(BuildContext context){
        return ChangeNotifierProvider<Counter>(
        	create : (_) => Counter(), // 처음에 사용할 데이터의 object를 생성해서 전달은 한다. 
            child : MaterialApp(...)
        )
    }
}
```

<hr>

- 제공된 데이터 사용 - Provider.of&lt;T&gt; 사용

  ```dart
  var data = Provider.of<Counter>(context); // 이런 형식으로 변수로 받아올 수도 있음
  
  // Provider.of<타입>(context) 
  ```

```dart
...
Widget build(BuildContext context){
    return Scaffold(
    	body : FlatButton(onPressed : Provider.of<Counter>(context).increment, //버튼으로 데이터 변경
                         child : Text('Button'))
        ... Text(Provider.of<Counter>(context).counter.toString()) // 데이터의 클래스의 getter를 이용해 값 가져옴 
    )
}
```



- 제공된 데이터 사용 - Consumer&lt;T&gt; 사용
  - 위젯처럼 사용하는 형식이다. 

```dart
Consumer<Counter>(
	builder :(context, value, child) => Widget(...)  //위젯을 리턴해야함
)
```

- builder 속성으로 context와 value와 child를 가져옴
- 여기서 이 **value**가 바로 이 제공된 데이터이다. (타입은 물론 그 데이터의 타입, 선언했던 타입 T와 같은 것)
- 정확하게 말하면 value는 그 클래스 타입의 **object**인 것이다. 
- 그래서 그 object로 값을 변경한다던지, 값을 가져온다던지 핸들할 수 있다. 

```dart
Consumer<Counter>(builder: (context, value, child){
            return Text('Current count returned : ${value.getCounter()}');
          })
```



- 사용 예제 
  - 로그인 화면인지 회원가입 화면인지에 대한 flag bool값으로 bool값에 따라 텍스트가 바뀌고 색이 바뀌는 형식
  - 언어 선택에서 설정한 언어에 따라 앱의 모든 텍스트의 언어가 변경되는 것 

<hr>

> MultiProvider

- Provider를 여러개 쓰고 싶을 때 사용하면 된다. 
- providers 속성으로 Provider의 리스트를 담을 수있다.
- 이 MultiProvider를 사용하지 않는다면 Provider끼리 중첩해야 사용해야한다. 

