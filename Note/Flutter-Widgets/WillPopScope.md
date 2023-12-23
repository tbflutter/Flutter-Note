<h2>Flutter Widget</h2>

<hr>

<h2>WillPopScope</h2>

<hr>

- 스마트폰에 있는 뒤로가기 버튼(Back Button)에 대한 제어를 해주는 위젯 클래스이다.
- 또한 이전 Route로 돌아가는 것을 막는 용도이기도 하다. 

- **추가**(2021/08/28) : AppBar에 뒤로가기의 기능을 스마트폰 내장 뒤로 가기 버튼에도 같은 기능을 적용하고 싶을 때도 사용 가능!!

  

> 사용 방법

1. Scaffold를 WillPopScope로 감싸는 것
2. WillPopScope를 Scaffold의 body에 넣는 것



- 속성으로는 onWillPop과 child가 존재한다.

- onWillPop 속성의 맵핑 타입은  Future&lt;bool&gt; Function()이다. 

- 뒤로가기 버튼을 눌렀을 때에 대한 동작을 위의 맵핑타입의 콜백함수에 넣으면 된다. 



#예시

```dart
return WillPopScope(
	onWillPop : _onBackPressed,
    child : Scaffold(...)
)
    
....
    
Future<bool> _onBackPressed(){
	return showDialog(
        context : context,
    	builder : (context) => AlertDialog(
        	title : Text('정말로 종료하시겠습니까?'),
            actions : [
                FlatButton(onPressed : () => Navigator.pop(context, true), child: Text('예')),
                FlatButton(onPressed : () => Navigator.pop(context, false), child : Text('아니요'))
            ]
        ) 	
    );
}
```

- 뒤로가기 버튼(Back Button)을 눌렀을 때 팝업창이 뜨면서 종료할 것인지 말 것인지에 대한 선택지를 주고
- "예" 버튼을 누르면 이전화면으로 가고(혹은 종료)
- "아니요" 버튼을 누르면 화면이동을 취소함(아무 일도 없음)



