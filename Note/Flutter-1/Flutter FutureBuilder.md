<h2>Flutter FutureBuilder</h2>

<hr>

- Future 연산은 결과 값을 수행하고 반환하는데 시간이 걸리는 작업이다. 
- 이를 핸들하기 위해 비동기 함수로 사용한다고 했었다. 
- 비동기 함수를 통해 비동기 작업을 사용해서 현재 작업이 수행되는 동안 프로그램은 다른 작업을 계속 이어서 할 수 있다. 그래서 Dart는 Future 오브젝트를 사용해서 비동기 작업의 결과를 이후에 반환을 해준다. 이러한 처리를 위해 async & await를 사용할 수 있지만 **위젯**에 대해서 Future를 다루는 것은 까다롭다. 



- 일반적으로 비동기 작업을 한다면 특히, Future가 있는 위젯을 만드려면 Stateful 위젯으로 만들고, Future 오브젝트가 작업을 완료하고 데이터를 리턴할 때, 그 값에 따라 setState() 메소드를 호출했어야 했다. (변경된 사항을 업데이트하여 build 메소드를 호출하여 새로고침)
- 그러나 setState()를 무분별하게 호출하는 것은 좋지 않기 때문에
- 딱 비동기 작업을 하여, 데이터를 기다렸다가 데이터가 다 받아졌으면 그 데이터를 위젯으로 뿌려줄 수 있는 것이 필요하다. 
- 그렇게 이러한 상황에서 사용하는 것이 **FutureBuilder** 위젯이다.



- 예시로 서버로부터 데이터를 받아올 때, 일반적으로 오래걸리는 작업에 명시적으로 시각적으로 보여주지 않으면 앱이 멈춘 것이라고 인지할 수가 있다.
- 이 FutureBuilder를 사용해서 서버로부터 받아온 데이터를 사용하는 부분만 데이터가 **도착한 후에** 표시하고, 그렇지 않은 부분은 화면에 바로 표시할 수 있다. 

- 즉, 비동기 작업 요청을 해서 응답을 받기 전까지는 로딩 위젯을 보여주고, 
- 성공 응답을 받으면 응답 받은 데이터를 보여주는 위젯을 보여주고, 
- 실패 응답, 에러를 받으면 에러메세지를 보여주는 위젯을 보여주고 싶을 때 사용이 가능하다. 

<i>**Future인데, 위젯을 보여주는데 사용하는 Future 오브젝트에 사용될 때**</i>



<hr>

> FutureBuilder는 가장 중요한 2가지의 속성을 가진다.

- future
  - future 속성에는 우리가 원하는 Future&lt;T&gt;를 리턴하는 함수를 넣는다.
- builder
  - builder 속성에는 context와 snapshot을 parameter로 갖고 **위젯**을 리턴하는 함수를 넣는다. 
  - snapshot은 Future&lt;T&gt;의 리턴 값을 의미한다. 
  - snapshot의 타입은 AsyncSnapshot이다. 
  - 그래서 future 값에 따라 원하는데로 위젯을 build하도록 설정하면, 작업이 다 처리가 될 때, 다시 build가 된다. 



- AsyncSnapshot은 3가지의 상태를 가진다. (builder속성의 함수의 매개변수인 snapshot의 상태)
  1.  connectionState.none 
     - future가 null인 상태
  2. connectionState.waiting
     - future는 null이 아니지만, 아직 Future 작업이 완료되지 않은 상태 
  3. connectionState.done
     - future가 null이 아니며, Future작업이 완료된 상태
     - AsyncSnapshot.data가 완료된 정상적인 데이터 값으로 설정된다.
     - 만약 완료됐지만, 에러를 반환하는 경우, AsyncSnapshot.hasError는 true가 되고, 
     - AsyncSnapshot.error가 error 오브젝트로 설정된다. 
     - 만약 정상적인 데이터를 받으면 AsynSnapshot.error는 null이 되고, AsyncSnapshot.hasError는 false가 된다. 



- 이 AsyncSnapshot의 connectionState를 통해 waiting 또는 hasData 속성을 이용해
- future의 값이 도착하지 않았을 경우에는 로딩중임을 나타내는 위젯을 리턴하고,
- future 값이 완료되었을 경우네는 이제 정상적으로 보여줄 위젯을 리턴하면 된다. 
- 또한 future 값이 완료되었을 경우에도 두가지의 상태(Data인지, Error인지)에 따라 각기 다른 처리, 그에 맞는 위젯을 리턴하도록 한다. 



- future의 상태가 바뀔 때마다 FutureBuilder의 builder 부분이 다시 실행된다!



- 사용 예시 1

```dart
FutureBuilder(
	future : _fetch(),
    builder : (BuildContext context, AsynSnapshot snapshot){
        if(snapshot.connectionState == ConnectionState.waiting){
            return Center(child : CircularProgressIndicator());
            // 작업이 끝나지 않아 기다리는 상태, 그래서 로딩 위젯을 리턴
        }
        else{
            if(snapshot.error != null){
                // error 값이 null이 아니라는 것은 error 오브젝트의 값이 존재한다는 의미므로, 에러 값이 반환된 경우
                return Center(child : Text('에러 발생!'));
                // 작업이 완료는 됐지만, 에러가 반환 된 경우, 그래서 그에 맞는 처리
            }
            else{
                return Center(child : Text(snapshot.data));
                // 작업이 완료되고, 정상적인 데이터를 받은 경우
            }
            
        }
    }
)
```



- 사용 예시 2

```dart
FutureBuilder(
	future : _fetch(),
    builder : (BuildContext context, AsynSnapshot snapshot){
        if(snapshot.hasData){ // 작업이 끝나고, 정상적인 데이터를 가지고 있는 상태
            return Center(child : Text(snapshot.data));
            // 작업이 완료되고, 정상적인 데이터를 받은 경우
            // 이곳에서는 snapshot.error가 null이다. 
        }
        else if(snapshot.hasError){// 작업이 끝나고, 에러를 가지고 있는 상태
            return Center(child : Text('에러 발생!'));
            // 작업이 완료는 됐지만, 에러가 반환 된 경우, 그래서 그에 맞는 처리
            // 이곳에서는 snapshot.data가 null이다. 
        }
        else{// 작업이 아직 끝나지 않은 상태 
         	return Center(child : CircularProgressIndicator());
            // 작업이 끝나지 않아 기다리는 상태, 그래서 로딩 위젯을 리턴  
        }
        //혹은 else문 없이 로딩 위젯 리턴하는 구문만 있어도 가능
    }
)
```



- 위 예시의 future 속성에 들어간 _fetch 함수

```dart
Future<String> _fetch() async{
    await Future.delayed(Duration(seconds : 2));
    return 'Call Data';
}
```

- 2초 동안은 로딩창이 뜨고, 2초가 지나면, 텍스트 위젯에 Call Data라는 String 값이 보여진다. 