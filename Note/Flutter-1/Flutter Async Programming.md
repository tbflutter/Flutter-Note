<h2>Flutter Async Programming</h2>

<hr>

- Dart에서 비동기 프로그래밍을 할 수 있는 방법은 두가지가 존재한다.
- Future 클래스는 비동기 작업을 할 때 사용한다. 
  1. Future API 사용
     - 긴 작업(DB, network, 통신요청등)을 요청하면 Future 오브젝트를 리턴하는데, 이 Future 오브젝트는 잠시 뒤로 넣어두고, 다른 작업을 순차적으로 처리한다. 그 사이에 뒤로 넣어둔 Future 오브젝트의 작업이 끝나 결과 값을 받는다. (async코드의 결과를 나타냄)
  2. async, await 키워드 사용 
     - 끝날 때까지 기다릴 수 있도록 선택권을 주는 것 (future를 리턴하는 함수가 있을 때 완료될 때까지 실행을 일시 중단하고 싶을 때)
     - await를 사용하면 await를 사용한 곳에서 future 작업이 끝날 때까지 기다린다. 
     - 하지만 await를 사용할 경우 다른 작업을 전부 멈추고, future 작업을 기다리기 때문에 앱의 성능을 악화시킬 수 있다. 
     - 이 때 async를 사용한다. 내부에서 await를 사용하는 함수는 async 함수가 되어야한다.
     - 

- Future의 상태는 3가지이다. 
  1. 개봉되지 않은 상태
  2. 개봉되었는데 올바른 값이 나온 상태
  3. 개봉되었는데 에러 값이 나온 상태

```dart
//Synchronous code
void printDailyNewsDigest(){
    var newsDigest = gatherNewsReports(); // Can take a while
    print(newsDigest);
}

main(){
    printDailyNewsDigest();
    printWinningLotteryNumbers();
    printWeatherForecast();
    printBaseballScore();
}
```

- 위와 같은 경우 시간이 걸리는 작업(gatherNewsReports)때문에 메인에서 아래 3개 함수가 진전하기가 어렵다는 것이다. 





```dart
Future<String> gatherNewsReports() =>
    Future.delayed(oneSecond, () => news);
```

- news를 1초 후에 리턴하는데 Future 오브젝트로 감싸서 사용한다.
- 제네릭으로 나중에 데이터를 받을 때 어떤 타입의 데이터를 받을 것인지에 대한 타입을 작성하는 것이다. 



- printDailyNewsDigest 함수를 호출하면 newsDigest가 gatherNewsReports로부터 받으니
- gatherNewsReports는 1초를 기다려야 되는데 Future이다. 
- 뭔가를 받아야되는 것은 아는데 1초가 걸리니깐 Future&lt;String&gt;이라는 선물박스를 가지고 와서 newsDigest에 던져준다.
- 그러면 newsDigest는 Future&lt;String&gt;를 받아서 그것을 출력을 하는데

- 문제는 Future&lt;String&gt;이라는 선물상자를 안열린 상태에서 출력을 하는 것이다. 
- 그렇기에 해줘야하는 async, await 키워드를 붙인다.

```dart
void printDailyNewsDigest() async{
    var newsDigest = await gatherNewsReports();
    print(newsDigest);
}
```

- await키워드를 붙였기 때문에 이 gatherNewsReport()를 다 실행할 때까지 기다려라는 의미이다. 

- async, await 키워드 없이 즉, 안 기다리고 Future&lt;String&gt;을 던져주고 출력을 하려니깐 상자가 안열린 상태로 출력하는 것이다. -> (결과 값 : Instance of '_Future&lt;String&gt;') 

<hr>

1. Future API 사용 => Future 자체를 사용

- 이 Future&lt;Strint&gt; 자체를 사용하는 것이다.

```dart
void printDailyNewsDigest(){
    var newsDigest = gatherNewsReports();
    newsDigest.then((value) => {
        print(value)
    });
}
```

- Future가 결과를 받았을 때 then() 메소드를 통해 파라미터에 결과를 받아온다.
  -  value의 타입은 String이 됨 
  - 위의 결과 값은 Future&lt;String&gt;라는 선물상자를 개봉한 결과를 의미한다. 



<hr>

2. async, await를 사용했을 때에는 코드상으로 순서대로 이동한다. 
3. Future API를 사용했을 때는 결과값은 외부에서 사용할 수 없다는 점이다. (블럭문 안에서만 사용 가능)

