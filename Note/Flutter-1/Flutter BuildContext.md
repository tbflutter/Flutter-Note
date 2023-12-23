<h2>Flutter BuildContext</h2>

<hr>

<h2>BuildContext, SnackBar, of(), Builder</h2>

<hr>


- 위젯 트리에서 현재 위젯의 위치를 알 수 있는 정보이다. 

- 모든 위젯은 build메소드를 가지고 있는데 BuildContext인 context를 argument로 가진다. 

  ```dart
  @override
  Widget build(BuildContext context){
      return Scaffold(
      ...
      );
  }
  ```

  위에서 build 메소드는 Widget타입이고 argument로 BuildContext 타입의 context라는 변수이다. 

  즉, 이 build메소드는 context라는 인자값을 대입한 Scaffold라는 위젯을 리턴한다는 의미이다. 

- build라는 메소드는 Scaffold라는 위젯을 리턴하는데, 이 때 위젯트리상에서 어디에 위치하는가에 대한 정보를 가지고 있는 context라는 것을 대입해서 리턴한다는 의미이다. 

  

- BuildContext는 stateless위젯이나 state 빌드 메소드에 의해서 리턴된 위젯의 부모가 된다. 

```dart
class MyPage extends StatelessWidget{
    @override
    Widget build(BuildContext context){
        return Scaffold(
        ...);
    }
}
```

- MyPage이라는 커스텀 위젯이 있는데 이 MyPage 위젯도 자신만의 BuildContext타입의 context를 가지고 있을 것이다. 

- 이제 build메소드를 통해서 Scaffold위젯이 리턴이 되는데

- 이 때 Scaffold 위젯은 부모인 MyPage의 context를 그대로 물려받는다. 

  

- 그렇다면 어떤 필요에 의해서 위젯트리상에서 Scaffold위젯의 위치가 필요하다면 현재 Scaffold 위젯의 context를 참조하면 어떻게 될까?

  => "Scaffold.of() called with a context that does not contain a Scaffold"라는 에러가 발생할 것이다. 

- Scaffold위젯이 존재하고 있으니 당연히 현재 Scaffold위젯이 위젯 트리상에서 어디에 위치하고 있는지를 알기 위해서 Scaffold위젯이 가지고 있는 context를 참조하는 것이 마땅한데

- 정작 Scaffold위젯의 context는 위젯트리내에서 Scaffold가 어디에 위치하고 있는지에 대한 정보를 전혀 가지고 있지않기 때문이다. 

build메소드에 의해서 리턴된  Scaffold위젯은 그 부모의 BuildContext타입인 context를 그대로 물려받는다고 했으니 

Scaffold위젯 밑에서 build 메소드로 무언가 위젯을 리턴하면 그 위젯은 부모인 Scaffold위젯의 진짜 context를 물려받을 수 있을 것이다. 

<hr>

- <h2>SnackBar</h2>

  - SnackBar는 화면의 하단에 짧게 보여주는 메세지(혹은 인터렉션 가능한 칸)이다.

- SnackBar를 구현하기 위해서는 Scaffold.of(context).showSnackBar()를 불러와야만 사용할 수 있다. 
- 반드시 Scaffold.of()메소드를 통해서 Scaffold 위치를 참조한 후
- 그 다음에 showSnackBar() 메소드내에서 SnackBar를 구현해야한다.
- SnackBar는 결국 Scaffold위에서 그려져야하기 때문에 Scaffold의 정확한 context를 참조해서 그곳에 SnackBar를 그릴수있도록 알려주어야한다. 

> Something.of(context) 메소드

- 현재 주어진 context에서 위젯트리에서 위로 올라가면서 가장 가까운 Something을 찾아서 반환하라는 의미이다. 

> SnackBar 구현하기 

```dart
class MyPage extends StatelessWidget{
	@override
    Widget build(BuildContext context){
        return Scaffold(
        appBar: AppBar(
        title:Text('Snack Bar'),
        centerTitle : true,
        ),
        body: Center(
        child:FlatButton(
        child:Text('show me'),
        color : Colors.red,
        onPressed: (){
            Scaffold.of(context).showSnackBar(SnackBar(
    content:Text('Content'),
));
        })))
    }
}
```

- 단순히 이렇게만 작성하면 

  **Scaffold.of() called with a context that does not contain a Scaffold.** 라는 에러 메세지가 발생하고

  **The context used was : MyPage** 라는 메세지도 뜨게 된다. 

- 즉 Scaffold 위젯 아래에서 context를 사용해서 위젯 트리상의 Scaffold 위치를 찾아서 올라가려고 했는데 정작 of() 메소드가 사용한 context는 "MyPage" 위젯의 것이다. 

  => 이는 build 메소드를 불러왔을 때 인자값으로 전달되는 BuildContext는 리턴되는 위젯의 것 아니라 이 build메소드를 불러오는 위젯의 BuildContext라는 것이다. 즉, 사용한 context는 리턴되는 Scaffold의 것이 아니라 build 메소드를 불러오는 MyPage 위젯의 것이다. 

- of() 메소드는 위젯트리상에서 MyPage위젯의 위치부터 Scaffold를 찾아나서게 되고, MyPage위젯의 부모위젯들을 따라올라가서 찾아보겠지만 Scaffold는 존재하지 않는다. 

  (MyPage 위젯은 MyApp에서 MaterialApp의 home 속성으로 가짐)

```dart
		      MyApp
                        |
                    MaterialApp
                        |
                      MyPage  <--------------
                        |                   |
                     Scaffold               |
                        |                   |
                     --- ---                |
                     |     |                |
                   AppBar Center            |
                            |               |
                         FlatButton         |
                   [Scaffold.of(context)] __|
```

- 이러한 위젯트리상에서 Scaffold.of() 메소드는 주어진 context를 가지고 Scaffold를 찾아나서게 된다. 
- 그러나 이 context는 MyPage 위젯의 context이므로 MyPage위젯부터 Scaffold를 찾게 되고 계속 위로 위젯트리를 거슬러 올라가면서 Scaffold를 찾아보지만 끝내 찾지 못하고 Scaffold가 포함되지 않은 context 라는 에러 메세지가 발생하는 것이다. 

**Scaffold.of() 메소드가 위젯트리상에서 Scaffold를 찾을 수 있게 하려면 어떻게 해야할까?**

=> Scaffold.of() 메소드가 위젯트리상에서 Scaffold보다 밑에 있는 위젯의 context를 사용할 수 있도록 하면 된다.

(그곳에서부터 거슬러 올라가면 Scaffold를 찾을 수 있을 것이다.)

> Builder 위젯

- 위의 상황을 위해서 존재하는 것이 Builder 위젯이다. 
- 핵심적인 역할은 지금까지 사용했던 context가 무엇이었든간에 다 무시하고 새로운 context로 새로운 위젯을 만들라는 것이다. 

```dart
		      MyApp
                        |
                    MaterialApp
                        |
                      MyPage  
                        |                   
                     Scaffold               
                        |                   
                     --- ---                
                     |     |                
                   AppBar Builder  <---------
                           |                |
                         Center             |
                           |                |
                        FlatButton          |
                   [Scaffold.of(context)] __|
```

- 그래서 Builder위젯 밑에 존재하는 Scaffold.of() 메소드가 더 이상 MyPage위젯의 context가 아니라 이 Builder 위젯의 context를 사용하게 만드는 것이다. 
- 그래서 결과적으로 Scaffold.of() 메소드가 위젯트리상에서 Builder 위젯 위로 거슬러 올라가면서 Scaffold 위젯을 찾게 되는 것이다. 

> Builder 위젯 사용

- builder 속성

  - builder 속성은 함수를 값으로 가지는데
  - 이 함수는 인자 갑승로 BuildContext 타입의 변수를 가지는데
  - 주의할점은 상위 위젯트리에 존재하는 context변수이름과 같으면 안되므로 다르게 지정한다. 
  - 그리고 이 함수는 위젯을 리턴해야한다. 

  ```dart
  Builder(builder : (BuildContext ctx){
     	return Widget(..);
  })
  ```

> 이를 이용한 최종적인 코드 수정

```dart
class MyPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Test'),
        backgroundColor: Colors.blue,
      ),
      body: Builder(builder: (BuildContext ctx){
         return Center(
           child: FlatButton(
             child: Text('show'),
             color: Colors.red,
             onPressed: (){             Scaffold.of(ctx).showSnackBar(SnackBar(content: Text('Hello'),));
             },
           ),
         )
      },),
    );
  }
}

```

- context와 ctx를 잘 구분해야한다. 
- context는 MyPage 위젯의 context이고
- ctx는 Builder 위젯의 context이다. 
- 그렇기에 위젯트리 상에서 Scaffold를 찾기 위해서는 ctx를 사용하여  Builder상위에 존재하는 Scaffold를 찾을 수  있을 것이다. 

<hr>

> Builder 위젯 없이 SnackBar 구현하기 

- 차라리 Builder 위젯을 사용하지 않고
- Scaffold 위젯 밑에 새로운 커스텀 위젯을 생성하여 이 커스텀 위젯의 context를 사용하면 될 것이다. 

```dart
		      MyApp
                        |
                    MaterialApp
                        |
                      MyPage
                    (context1)
                        |                   
                     Scaffold               
                        |                   
                     --- ---                
                     |     |                
                   AppBar MySnackBar  <--------
                          (context2)          |
                             |                |
                           Center             |
                             |                |
                          FlatButton          |
                     [Scaffold.of(context2)] _|
```



```dart
class MySnackBar extends StatelessWidget{
    @override
    Widget build(BuildContext context){
        return Center(
        child : RaisedButton(
            child : Text('Show'),
            onPressed : () {
                Scaffold.of(context).showSnackBar(SnackBar(
                content : Text('Hello')));
            }));
    }
}
```

- MySnackBar의 build 메소드를 통해 Center위젯이 리턴되는데 
- Scaffold.of(context)는 MySnackBar의 context를 전달받고
- 이에 근거하여 MySnackBar 위젯의 위치부터 거슬로 올라가며
- 위에 있는 Scaffold를 찾을 수 있다. 
