<h2>Flutter Widget</h2>

<hr>

<h2>Opacity</h2>

<hr>

- opacity라는 단어는 "불투명한"이라는 뜻을 가지고 있다. 
- 그래서 이 Opacity라는 위젯의 역할은 바로 이 위젯의 자식위젯의 투명도를 지정해주는 역할을 해준다. 
- 투명하게할 것인지, 불투명하게 할 것인지, 아니면 그 사이로 할 것인지 값을 정해줄 수 있다. 
- 그 값을 통해서 자식 위젯의 투명도가 결정된다.



> 속성

- opacity -> double
  - 가장 중요한 속성으로 투명도 값을 정해주는 속성이다.
  - default 값은 1.0이다.
  - 1.0이 불투명상태, 즉 잘 보이는 값이고
  - 0.0이 투명상태, 즉, 안 보이게 하는 값이다. 
  - 이제 0과 1 사이 값에서 상대적으로 자식위젯의 투명도를 설정해준다.

```dart
Opacity(
	opacity : 1.0 
    child : Text('see me');
)
```



- 이 Opacity 위젯을 통해 double 타입의 변수를 하나 설정해서 상태에 따라 투명도를 변경시켜 동작시킬 수도 있다. 

```dart
double _opacity = 1.0;

Column(
	children : [
        Opacity(
        	opacity : _opacity,
            child : Text('see me')
        ),
        FlatButton(
        	child : Text('opacity up'),
            onPressed : (){
                setState((){
                    if(_opacity > 1.0){
                    	_opacity = 0.0;
                	}
                	else{
                    	_opacity += 0.1;
                	}
                });
            }
        ),
        FlatButton(
        	child : Text('opacity down'),
            onPressed : (){
                setState((){
			  	if(_opacity < 0){
                    	_opacity = 1.0;
                	}
                	else{
                    	_opacity -= 0.1; 
                	}
                 });
            }
        )
    ]
)
```





