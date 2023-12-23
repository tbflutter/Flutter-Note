<h2>Flutter Plugins</h2>

<hr>

<h2>printing</h2>

<hr>

- pdf문서를 작성하는 라이브러리이다. 
- 추가적으로 모바일에서 인쇄가 가능하도록 하는 기능까지 내재되어 있는 라이브러리이다.

- 사용하기 위해서는 4가지의 패키지를 사용해야한다. 

```dart
import 'package:printing/printing.dart';
import 'package:pdf/pdf.dart';
import 'package:pdf/widget.dart' as pw;
import 'package:path_provider/path_provider.dart';
```

- printing plugin내에 pdf 라이브러리가 포함되어 있음
- 3번째 패키지는 pdf문서안에 들어갈 위젯에 대한 패키지이다. 
- 그렇기 때문에 pdf 위젯만의 모듈 이름을 지정해줘야한다. (그렇지 않으면 일반 플러터에서 사용하는 위젯과 겹쳐지기 때문에, 에러가 발생함) (as pw)
- 지정해준 모듈이름으로 위젯에 접근해서 사용하면 된다. (분리된 위젯 구조로 됨 pdf문서에 들어갈 위젯)
- 그렇기 때문에, 이 문서 페이지안에 작성되는데 사용되는 모든 위젯은 모듈이름 위젯으로 사용해야 한다. 



<hr>

<h3>1. 문서 오브젝트 가져오기</h3>

```dart
final doc = pw.Document()
```

- 위와 같이 만든 문서 오브젝트로 pdf 문서를 다룰 수 있다. 
- **addPage()** 메소드로 문서 페이지를 추가할 수 있다. 



<hr>

<h3>2. 페이지 추가하기 </h3>

- addPage 메소드로 페이지를 추가하면서, 내부의 데이터, 메타데이터등을 조작할 수 있다. 
- 페이지를 여러개를 반복적으로 추가하고 싶은 경우에는 반복문으로 addPage 메소드를 반복 호출하면 된다. (각기 다른 내용이 되도록 logic을 짜면서)

- addPage 메소드의 인자값에는 추가할 페이지 오브젝트를 넣으면 된다. 
- 페이지를 추가할 때, 단일 페이지(Page)와 다중 페이지(MultiPage), 두 가지로 나뉜다. 

> MultiPage 정의, 특징

- 페이지에 오버플로우가 발생하면 자동으로 다른 페이지로 다중페이지를 만든다.
- 페이지내에 작성되는 내부 위젯 트리는 한 페이지의 크기보다 클 수 없다. 
- 위젯은 한 페이지에 부분적으로 그릴 수 있지만, 다 못그린 나머지 부분은 다른 페이지에 그릴 수 없다. (insecable)

- 작은 위젯의 집합은 자동으로 여러 페이지에 걸쳐서 작성될 수 있다.

- build 메소드를 직접적인 자식으로 사용될 수 있다. 

  -> Flex, Partition, Table, Wrap, GridView, Column

- 여기서의 Wrap 위젯은 하위 항목을 여러 페이지에 걸쳐 재정렬할 수 있다.

- 하지만 Wrap의 자식이 한 페이지에 들어가야 한다. (must fit in a page)

- 그렇지 않으면 오류가 발생한다. 

<h3>즉, 여러개의 위젯을 감싸는 Multi Child Widget의 경우에는 총 크기가 한 페이지의 크기보다 크면 그려줄 수 없다. 
한 페이지보다 작으면서, 그러한 위젯들이 여러개 있어서 오버플로우가 날 상황이라면 자동으로 다른 페이지에 이어서 그려준다.
</h3>



```
            pw.Column
                |
    ---------------------------
    |           |             |
pw.Column(1)  pw.Column(2)  pw.Column(3)
```



- 한 페이지의 높이가 100이라고 가정하면,

- 1번 Column의 높이가 40인 채로 작성한다면, 페이지 한개로 내용이 다 채워질 것이다. (1번 Column만 사용했을 때)

- 1번 Column의 높이가 40이고, 2번 Column의 높이가 40으로 작성한다면, 한 페이지안에 두개의 컬럼이 다 채워질 것이다. (1번, 2번만 사용)

- 1번 Column의 높이가 40이고, 2번 Column의 높이가 40이고, 3번 Column의 높이가 40으로 작성한다면, 2개의 페이지로 나뉘면서, 첫번째 페이지에는 1번 컬럼과 2번 컬럼이 채워지고, 두번째 페이지에는 3번 컬럼이 채워진다. 

- 1번 Column의 높이가 50이고, 2번 Column의 높이가 60이고, 3번 Column의 높이가 60으로 작성한다면, 3개의 페이지로 나뉘면서, 첫번째 페이지에는 1번 컬럼, 두번째 페이지에는 2번 컬럼, 세번째 페이지에는 3번 컬럼만이 각각 채워진다. (1번 + 2번이 110, 즉 한 페이지의 높이를 초과되서 오버플로우되기 때문에 위젯 전체를 다른 페이지에 새로 그려주는 것이다. -> **insecable** 속성)

  

  **위젯 일부가 짤리는 것이 아니다!!**

  첫번째 페이지에 1번 컬럼 전체, 2번 컬럼 높이 50만큼

  두번째 페이지에 2번 컬럼 남은 높이 10, 3번 컬럼 전체

  * 위와 같은 결과가 나오는 것이 아니다

  

> MultiPage 속성

- theme 

  - 페이지의 테마를 설정
  - 폰트(font)가 예시
  - pdf에 작성될 글씨는 아무것이나 작성할 수 없기 때문에, 다른 문자나 언어, 폰트등을 사용해야한다면, 폰트 파일을 가져와서 불러와서 사용을 해야한다. 

  ```dart
  theme : pw.ThemeData.withFont(
  	base : pw.Font.ttf(await rootBundle.load('fonts/NanumGothic-Regular.ttf'))
  )
  //rootBundle은 
  import 'package:flutter/services.dart';
  //가 필요하다.
  ```

  - 경험으로써 작성 : 플러터 기본 폰트는 한글을 지원하지 않는 듯 하다. 그래서 외부 파일로 폰트 파일(.ttf)을 가져와서 불러오는 형식으로 사용함.(나눔고딕 폰트)

  - pubspec.yaml에도 설정해줘야 하고, 프로젝트 파일에 fonts 디렉토리를 만들어서 그 디렉토리안에 파일을 집어 넣으면 된다. 

    ```yaml
      fonts:
        - family: NanumGothic
          fonts:
            - asset: fonts/NanumGothic-Regular.ttf
    ```

    

- pageFormat

  - 페이지의 형식을 지정
  - a3인지, a4인지, a5인지에 대한 것 

  ```dart
  pageFormat : PdfPageFormat.a4
  ```

  

- build

  - Pdf 문서내에 위젯들을 그려주게 하는 곳 
  - MultiPage의 경우 List 형식으로 위젯들을 리턴해야 한다. 

  ```dart
  build : (pw.Context context){
      return [
          //pdf 문서에 그려줄 위젯들
      ]
  }
  ```

  

> 페이지 추가 완성

```dart
doc.addPage(pw.MultiPage(
	theme : pw.ThemeData.withFont(
    	base : pw.Font.ttf(await rootBundle.load('fonts/NanumGothic-Regular.ttf'))
    ),
    pageFormat : PdfPageFormat.a4,
    build : (pw.Context context){
        return [
            // Widgets
        ]
    }
))
```

<hr>

<h3>3. 완성한 Pdf 파일 저장하기</h3>

​	

```dart
await Printing.layoutPdf(
	name : 'file name'
    onLayout : (PdfPageFormat formate) async{
        return await doc.save();
    }
);
```

- Printing.layoutPdf으로 파일을 저장하는데,
- name 속성으로 파일 이름을 지정해줄 수 있다. (String 타입)
- onLayout 속성에 위에 만들었던 문서 오브젝트로 save() 메소드를 호출하면 된다. 

<hr>

<h2>pub.dev</h2>

<a>https://pub.dev/packages/printing</a>