<h2>Flutter Reactive Programming</h2>

<hr>

<h2>1. Stream </h2>

<hr>

> What is Reactive Programming?

- 데이터 스트림과 변경 사항에 대한 Propagation를 중심으로 하는 프로그래밍
- 특정 동작이 일어난다면, 다른 부분에서도 이 특정 동작에 대한 반응 및 동적인 데이터 갱신을 하는 것이다.

- 반대되는 개념으로 Imperative Programming이 있다 

```dart
x = 1
y = 1
x + y = 2
x * y = 1
2 * x + y = 3
```

- Imperative Programming의 경우 x의 값을 2로 변경하면 연산의 결과인 x + y = 2과 같은 것들은 변하지 않는다.
- 일일이 직접 수정을 해줘야한다. 

- 그러나 Reactive Programming의 경우 x의 값을 2로 변경하는 순간 연산의 결과도 즉시 동시에 변경된다. 
- 값의 변경에 따라 반응하여 진행되는 것이다. 

<hr>

> Stream?

- 스트림은 데이터나 이벤트가 들어오고 나가는 파이프(pipe) 통로이다. 
- 앱을 만들다보면 데이터를 처리할일이 많은데, 언제 들어올지 확실하게 알 수가 없다.
- Stream은 이와 같은 비동기 작업에 주로 쓰인다. 

```
관찰대상(데이터 생산) <-관찰(구독) 관찰자(데이터 소비)
				  바뀐점 알려주기->
```

- 함수에다가 리스너를 스트림에 연결을 한다. 
- 이 함수가 스트림에 구독(subscribe)을 해서 듣고 있을 때, 데이터가 오는 것이다. 
- 데이터가 들어오면 그 데이터를 처리한다. 
- 또 다른 작업을 처리하다가 데이터가 또 들어오면 그 데이터를 처리한다. 
- 구독을 하고 들어오는 데이터마다 데이터를 처리하는 것이다. 
- Future는 데이터를 단일적으로 데이터 "하나"만 처리하지만
- Stream은 list처럼 여러개의 데이터를 처리할 수 있다. 

```dart
import 'dart:async'; // 이를 적용해야 stream을 사용할 수 있다. 
```

- Stream은 한개의 Listener만 가질 수 있다. 
- 여러개의 Listener를 가지려면 Broadcast Stream을 사용하면 된다. 

- 구독이 안된 상태에서 데이터가 도착하면 Stream을 타고 들어오면 기다렸다가, 만약에 리스너가 구독을 하면
- 그 때 한꺼번에 데이터를 처리한다. 
- 에러 조차도 Stream을 타고 들어올 수 있다. 
- 에러가 들어왔을 때 에러를 처리하고 나서 Stream의 구독을 취소를 할 것인가 그래서 데이터를 더 이상 받지 않을 것인가, 아니면 계속 데이터를 받을 것인가에 대해 선택권을 준다. 

- Stream의 최고 장점인 "Manipulation"인 데이터가 도착했을 때 이 데이터를 바로 사용하기 전에 
- 중간에 데이터를 가공할 수가 있다. -> A라는 데이터가 Stream을 타고 들어왔는데 도착했더니 A`으로 도착함

- Stream을 통해 특정 데이터만을 받을 수가 있다. (제네릭) (그 이외의 데이터는 버림)
- 데이터가 도착했는데, 데이터들 중에 같은 데이터는 다 제거할 수가 있다. -> Distinct

<hr>

- 보통 UI의 변경작업을 setState() 메소드를 호출하는데, setState의 경우 동작하는 방식은
- build() 메소드를 호출하여, 즉 가장 위부터 전부 다 다시 그리는 것이다. 

- 그러나 Stream을 사용하면 위젯트리가 전부 그려지는게 아니라 Stream와 연결된 위젯만 다시 그려진다!

<hr>

> 실제 코드상에서 How?

```dart
Future<int> sumStream(Stream<int> stream) async{
    var sum = 0;
    await for(var value in stream){
        print(sum);
        sum += value;
    }
    return sum;
}
Stream<int> countStream(int to) async*{
    for(int i=1; i<= to; i++){
        await Future.delayed(const Duration(seconds : 1));
        yield i;
    }
}

main() async{
    var stream = countStream(10);
    var sum = await sumStream(stream);
    print(sum);
}
```





```dart
var transformer = StreamTransformer<int, String>.fromHandlers(handleData : (value, sink){
    sink.add("value : $value");
});

stream.transform(transformer).listen
```



<hr>

1. 1초마다 데이터를 만드는 스트림 -> 반복적인 작업을 할 때

```dart
void main(){
    var stream = Stream.periodic(Duration(seconds: 1), (x) => x).take(3);
    //1. 스트림 생성하기 -> 1초마다 데이터를 1개씩 만듦 (3개까지만 -> take(3))
    // stream의 타입은 Stream<int>
    stream.listen((x) => print('take : $x'));
    //2. 이벤트 처리 
}

//출력
//take 0
//take 1
//take 2
```

- <i>Stream.periodic()</i>에서 Duration을 통해 그 정한 시간마다 데이터(이벤트)를 생성한다. 
- 생성한 스트림과 이벤트를 연결(listen)한다.
- 또한 listen의 인자값으로는 데이터 혹은 이벤트를 처리하는 부분이 들어간다. 

```
Stream의 과정 : 스트림 생성 -> 연결(listen) -> 데이터 또는 이벤트 처리
```



2. 일반적인 데이터를 다룰 때 

```dart
void main(){
	var stream = Stream.fromIterable([1,2,3,4,5]);
    stream.listen((int x) => print('iterable : $x'));
}
//출력
//iterable : 1
//iterable : 2
//iterable : 3
//iterable : 4
//iterable : 5
```



3. 비동기 데이터를 처리할 때 

```dart
void main(){
    var stream = Stream.fromFuture(getData());
    stream.listen((x) => print('from Future : $x'));
}
Future<String> getData() async{
    await Future.delayed(Duration(seconds: 3));
    print('Fetched Data');
    return '3초 기다렸다가 온 데이터';
}
//출력
//(3초 뒤에) 3초 기다렸다가 온 데이터
```



4. 다양하게 다뤄보기

```dart
var stream = Stream.fromIterable([1,2,3,4,5]);
stream.first.then((value) => print('stream.first : $value'));
//결과 : stream.first : 1 
stream = Stream.fromIterable([1,2,3,4,5]);
stream.last.then((value) => print('stream.last : $value'));
//결과 : stream.last : 5
stream = Stream.fromIterable([1,2,3,4,5]);
stream.isEmpty.then((value) => print('stream.isEmpty : $value'));
//결과 : stream.isEmpty : false
stream = Stream.fromIterable([1,2,3,4,5,6]);
stream.length.then((value) => print('stream.length : $value'));
//결과 : stream.isEmpty : 6
```

- 스트림을 사용할 때마다 계속 새로 만들어주는데, 

- stream.first를 실행하고 stream.last를 바로 사용할 수는 없는걸까?

  => Stream은 Single Subscription으로 한군데에서만 listen할 수 있다. 

  여러군데에서 listen하려면 "Broadcast"로 변경시켜줘야한다. 

5. Stream 변경, 변형

```dart
var transformer = StreamTransformer<int, String>.fromHandlers(handleData : (value, sink){
    sink.add('value : $value');
});

var stream = Stream.fromIterable([1,2,3,4,5]);
stream.transform(transformer).listen((value) => print('listen : $value'));
```

- sink는 Stream이라는 파이프의 입구라는 생각하면 된다. (이벤트를 받아들이는 곳)
- 변형을 하게 되면 제네릭으로 두가지가 있는데, 이는 첫번째 타입에서 두번째 타입으로 바꾸겠다는 의미이다. 

- 스트림을 통해 받아온 값 (정수형 타입인 1,2,3,4,5)를 문자열 타입의 'value : $value'으로 바꿔서 스트림에 넣는다. 



6. Stream - async*, yield

- 함수를 사용한 스트림

```dart
Stream<int> createStream(List<int> numbers) async*{
    for(var number in numbers){
        yield number;
    }
}
void main(){
    var numStream = createStream([1,3,5,7,9]);
    numStream.listen((number) => print(number));
}
```

- 이전에 사용한 Stream클래스의 메소드와 같은 동작을 한다.
- 차이점은 **async***와 **yield**를 사용해서 만들었다는 것이다.
  - async* : 제너레이터를 만드는다는 의미로, 게으르게 데이터 연산을 할 때 쓰인다. 게으르다는 것은 미리 연산을 다 하는 것이 아니라, 요청이 있을 때까지는 연산을 미뤄뒀다가 필요할 때 처리하는 것을 뜻한다. 또한 yield를 사용하겠다는 의미도 있다.
  - yield : return과 유사하다. return은 한번 리턴하면 함수가 종료되지만, yield는 열린 채로 있어서 필요할 때 다른 연산을 할 수 있다. 

7. Stream - BroadCast
   - 기본적으로 만들어지는 스트림은 하나의 listener를 가질 수 있다(한 곳에서만 listen을 연결할 수 있다)

```dart
var stream = Stream.periodic(DUration(seconds: 1), (x) => x).where((x) => x%2 == 0).take(3);
stream.listen(print);
stream.listen(print);
// 두 군데 연결 -> 에러 발생!
```

- 위와 같은 상황은 에러를 발생하기 때문에 여러 곳에서 listen을 하려면 Broadcast로 사용해야한다.

```dart
var stream = Stream.fromIterable([1,2,3]);
var bcStream = stream.asBroadcastStream();
bcStream.listen((value) => print('value : $value'));
bcStream.listen((value) => print('again : $value'));
```

- 싱글 스트림을 asBroadcastStream() 메소드를 통해 broadcast 스트림으로 변경할 수 있다.
- broadcast 스트림을 사용함으로써, 여러개의 listen이 가능해진다. 



8. StreamController

- Stream을 매번 열었다가(listen) 닫는 것(cancel)은 비효율적이다.
- 게다가 스트림이 여러개 일때는 모든 스트림을 일일이 닫는 것은 비효율적이다.
- 여러 스트림을 관리하기 위해 StreamController를 사용한다.

```dart
final StreamController controller = StreamController();
final StreamSubscription subscription = controller.stream.listen((value) => print(value));

controller.add(10);
controller.add(200);
controller.add(1000);

controller.close();

controller.add(90); // 닫혔기 때문에 아무것도 출력 X
```

- 스트림 컨트롤러에 데이터가 더해질 때마다 값을 출력한다. 
- 스트림 컨트롤러가 닫기 전까지만 이벤트를 처리한다. 