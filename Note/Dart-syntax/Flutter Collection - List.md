<h2>Flutter Collection - List</h2>

<hr>

- 데이터들을 모아서 가지고 있는 자료구조이다. 

<hr>

<h2>List</h2>

- 다른 언어에서의 array를 플러터에서는 List로 사용한다. 
- 다른 언어에서의 array처럼 인덱스로 [와 ]로 접근할 수 있다. 
- [ 와 ]로 열고 닫아 접근할 수 있다. 
- List는 두가지로 나뉘는데
  - **Fixed-length List** : 리스트내의 데이터의 개수가 지정한 개수만큼 올수있음
  - **Growable List**  : 데이터의 개수의 제한이 없음

```dart
void main(){
    var number = new List();
    
    List number2 = new List();
}
```

- 위에는 List를 만드는 기본적인 구조이다. 
- List 클래스 생성자에 숫자값을 넣어주면 Fixed-length list가 되고 그렇지 않고 비워두면 Growable list가 된다. 
- 위에서 두번째처럼 선언하면 타입은 "List&lt;dynamic&gt;"이 된다. 
- dynamic은 어떤 변수가 여러 타입으로 지정될 수 있어야할 때 사용한다.
- 현재 저 위의 List는 어떤 타입의 데이터들이 들어가야할지는 지정을 하지 않았다.
- 그래서 이 경우에 dynamic은 List안에 다양한 타입의 데이터들이 마음대로 들어올 수 있다는 의미이다. 

※ new List()로 초기화 하는 작업은 deprecated됨 - 2021/09/18 수정

```dart
void main(){
    var number = [];
    
    List number2 = [];
}
```



<h3>데이터 추가</h3>

- .add() 메소드를 이용하여 인자값에 List의 타입에 맞는 데이터값을 하나를 넣어주면 List에 넣어준 데이터가 추가된다. 

```dart
void main(){
    List number = new List();
    number.add(2);
    number.add('test');
    number.add(3.14);
    print(number);
    // 출력 : [2, test, 7.4]
}
```

- Dart에서는 변수에 넣을 수 있는 모든 것을 객체로 취급한다. 
- 이는 list내에서 데이터로 나열될 수 있다는 의미이다. 
- Dart에서는 함수나 boolean 조차도 객체로 취급하기 때문에 List에 넣을 수 있다.

```dart
int addNum(int n1,int n2){
    return n1 + n2;
}
void main(){
    List number = new List();
    number.add(2);
    number.add('test');
    number.add(3.14);
    number.add(addNum(3,4));
    print(number);
    // 출력 : [2, test, 7.4, 7]
}
```



- List 클래스 바로 앞에 &lt; &gt;(제네릭)를 통해 List에 들어갈 값의 타입을 지정할 수 있다. (구체적인 타입으로 사용하는 것이 좋다! No dynamic, var)

```dart
void main(){
    List<int> number = new List();
    number.add(2);
    number.add('test'); // 에러 - String타입이기 때문
    number.add(3.14); // 에러 - double 타입이기 때문
    number.add(addNum(3,4));
    print(number); 
}
```



- .addAll() 메소드를 통해 모든 데이터를 List에 넣을 수 있다. 
- 데이터의 나열이기 때문에 [와 ]로 열고닫아 이 안에 데이터 값을 넣어준다.

```dart
void main(){
    List<String> names = List();
    name.addAll(['James','John','Tom']);
    print(names);
    // 출력 : [James, John, Tom]
}
```



<h3>여러가지 멤버들</h3>

- length -> int : 현재 list에 들어있는 개수를 반환함 
- isEmpty -> bool : 현재 list가 비어있는지 안비어있는지 bool 값 반환
- remove(Object obj) : 리스트에 들어있는 obj값을 삭제함
- removeAt(int index)  : index에 해당하는 값을 리스트에서 삭제
- removeLast() : 리스트에 마지막으로 들어간 값을 삭제 
- removeRange(int start, int end) : 인덱스 start부터 end까지의 값을 삭제 
- clear() : 리스트에 있는 모든 값 제거
- contain(Object obj) : obj 값이 리스트내에 있는지 bool값으로 리턴
- elementAt(int index) : index에 있는 값 리턴
- insert(int index, int element) : index 위치에 element값 삽입