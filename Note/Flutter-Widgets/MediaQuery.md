<h2>Flutter Widget</h2>

<hr>

<h2>MediaQuery</h2>

<hr>

- 앱을 사용하는 기기마다 크기가 다르기 때문에, 어느 한 고정된 숫자값으로 사용한다면, 어떤 기기는 원하는 배치가 될테지만, 또 어떤 기기는 배치가 뒤틀린다거나, 짤릴 수가 있다. 
- 이러한 UI작업에서 모든 기기의 크기에 맞춰지도록 유동적인 UI를 그리기 위해 MediaQuery 위젯 클래스가 존재한다.
- 이 MediaQuery 클래스를 통해 앱을 실행하는 기기의 화면 size를 가져올 수 있다.
- 또 더 다른 기능이 있다.(아직은 모름) ; orientation, padding, textScaleFactor, disableAnimations등..



```dart
final Size size = MediaQuery.of(context).size; 
```

- build 메소드내에서 위와 같이 Size클래스에 대한 size값을 받아온다. 
- (확실치 않지만 build 메소드내에서만 MediaQuery 클래스를 불러와야만 사용이 가능하다?..)



- 이제 이 size 변수로 너비와 높이의 값으로 UI를 비율대로 맞출 수가 있다.

```dart
Container(
width : size.width * 0.2
height : size.height * 0.7,
...)
```

- 위의 예시는 현재 앱을 실행한 기기의 화면에서 Container가 너비가 20% 차지하고, 높이는 70프로 차지한다는 의미이다. 



