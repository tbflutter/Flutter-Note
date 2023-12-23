<h2>Flutter Techniques</h2>

<hr>

<h2>conditional work in WebView with Indicator</h2>

<hr>

- 웹페이지를 끌어올 때, 바로 UI에 뿌려지지 않고 어느정도 가져오는데, 시간이 걸릴 수가 있다. 
- 그런데, 이런 웹 데이터를 가져올 때, 아무런 조치없이 빈화면이 오래 지속되다가 갑자기 웹 페이지가 뜨면, 앱이 멈춘 것처럼 보일 수가 있기 때문에, 시각적으로 로딩 중이다라는 것을 보여줄 필요가 있다. 

```dart
Widget getWebView(String url){
    return Stack(
    	children : [
            WebView(
            	initialUrl : url,
            	onPageStarted : (start){ // 시작될 때 다시 state를 바꾸는 이유는 다시 이 WebView를 보여줄 때는 상태가 이미 변한 상태여서 일회성이 되버린다. 그렇기 때문에, 웹 데이터를 가져오기 시작할 때 데이터를 가져온다는 즉, 로딩을 하고 있다라는 의미로 bool 값을 true로 설정해준다.
                    setState((){
                        web_loading = true;
                    });
                },
                onPageFinished : (finish){ //핵심 1
                    setState((){
                        web_loading = false;
                    });
                },
            ),
            web_loading ? Center(child : CircularProgressIndicator()) : Stack(); //핵심 2
        ]
    );
}
```

- 핵심은 WebView 위젯에서 onPageStarted 속성과 onPageFinished 속성이다.
- 말 그대로 Page가 시작될 때와 끝날 때, 즉, 이 의미는 웹 데이터를 가져오기 시작할 때와 가져오기가 끝났을 때에 실행하는 속성이다. 
- 그래서 bool타입의 flag값을 하나 설정해서 데이터를 가져오기 시작할 때는 아직 데이터를 가져오지 않았다고 해서 
- true 값을 주고, 데이터 가져오는 것이 끝났을 때, false값을 준다. (loading 중이냐의 의미에서) 
- 또한 이 값을 줄 때에는 setState() 메소드를 호출하여, 상태를 변화시켜줘서 그 상태에 따른 작업을 보여주도록 한다. 

- 또한 핵심은 Stack 위젯이다. 
- Stack 위젯으로 위젯끼리 겹쳐져서 아직 보여지지 않은 WebView 위젯에 Indicator를 보여주다가, WebView의 데이터가 다 가져와져서 flag값을 전환하여 state를 변경하면, 가져온 WebView를 보여주는 것이다. 
- 위에서 web_loading이 true일 때는 Indicator를 보여주는데, 웹 데이터를 다 가져와서 web_loading이 false가 되면 state가 변화되어 단순한 Stack() 위젯을 보여주는데, 이 삼항연산자에 있는 Stack()위젯은 "무"의 존재이므로 
- 아무것도 보이지 않는다. 그러나 부모 위젯으로 Stack을 주지 않았다면 이 Stack 위젯이 공간을 차지했을 것이다.
- 그렇기에 부모 위젯에 Stack 위젯을 줌으로써, 내부적으로는 위젯트리상에서는 겹치게 되지만, 실제로 보여지는 것은  단 하나의 화면일 것이다. 

<hr>

> 시행착오

- ChangeNotifierProvider를 이용해서 onPageFinished 속성으로 상태 값을 변화시켜 구독자들에게 알려주는 형식으로 하려고 했었다. 
- 그러나 이 Provider에 의해 제공된 데이터로 삼항연사자로 WebView를 보여주려고 했었다.

```dart
var current = Provider.of<LoadingData>(context);
...
children : [
    current.loading ? CircularProgressIndicator() : getWebView(...),
    ...
]
```

- 그러나 이 current.loading이라는 상태를 변화시키는 trigger는 getWebView에서 WebView의 onPageFinished속성에 의해 변화된다. 
- 그러나 상태를 변화시키려면 getWebView() 함수를 호출해야만 하는데, 상태가 변하지 않는 한 current.loading의 상태가 변하지 않는다. 
- 그렇기 때문에 실제 WebView가 보여지는 곳에서 조건을 건 것이 아니라, 우선 getWebView 함수를 호출하고 그 내부에서 상태를 변화시키도록 하였다. 
- (결국 Provider는 쓸 필요가 없었다는..)





