<h2>Flutter Http communication with Rest API</h2>

<hr>

> Http method - CRUD

| method | function                                                     | example                |
| ------ | ------------------------------------------------------------ | ---------------------- |
| GET    | 데이터 읽기(Read), 검색(Retrieve)<br />form data를 URL에 포함 시킨다. (action URL - query string parameters) 요청에 body가 없음. 응답에 body가 있음. | 게시판 리스트 불러오기 |
| POST   | 새로운 리소스 생성(Create) 및 저장<br />추가적인 데이터를 body에 포함 시킨다. 응답에 body가 있음. | 회원가입, 로그인       |
| PUT    | 전체 데이터 수정(Update)                                     | 회원정보 전체 수정     |
| DELETE | 데이터 삭제(Delete)                                          | 회원정보 삭제          |
| PATCH  | 데이터 수정(Update)                                          | 회원정보 일부 수정     |



> Http Status Code (Header의 Entity header)

| code | state         | meaning                                                      |
| ---- | ------------- | ------------------------------------------------------------ |
| 1xx  | Informational | 요청 정보 처리 중                                            |
| 2xx  | Success       | 요청을 정상적으로 처리함                                     |
| 3xx  | Redirection   | 요청을 완료하기 위해 추가 동작 필요                          |
| 4xx  | Client Error  | 서버가 요청을 이해하지 못함 (클라이언트가 서버에 잘못된 요청을 했을 경우) |
| 5xx  | Server Error  | 서버가 요청 처리 실패함 (대부분의 에러 코드를 500 Error 처리) |

  

> Http Content-Type

- 헤더에 포함되는 속성; 리소스의 미디어 타입
- Application/x-www-form-urlencoded : HTML Form 형태
- Application/json : JSON 형태
- multipart/form-data : 파일 첨부
- text/* : 단순 html, css, js 파일 



<hr>

<h2>REST API</h2>

- Element들로 Resource, Method, Message 3가지로 구성

- name이 John인 사용자를 생성한다. 

  → 사용자 : 생성되는 resource

  → 행위 : method

  → name이 John인 사용자 : message

- HTTP POST, http://~~/users/{

  ​	"users" : {

  ​		"name" : "John"	

  ​	}

  }

https://bcho.tistory.com/953

https://meetup.toast.com/posts/92





<hr> 

<h2> Flutter에서의 사용</h2>

- http plugin 필요 

- android의 경우 manifest에 두가지 설정 필요 

  - android / app / src / main / AndroidManifest.xml

    ```xml
    <application
                 ...
                 android:usesCleartextTraffic="true"
    ```

    ```xml
    <uses-permission android:name="andrid.permission.INTERNET"/>
    ```

    

- ios도 추가 설정 필요

- Back-end는 php 사용

> 데이터 전송하기 POST

- Client

```dart
import 'package:http/http.dart' as http;

Future<void> _postRequest() async{
    String url = 'http://xxx/test1.php'; // 요청할 url
    
    http.Response response = await http.post(url, headers : <String, String>{
        'Content-Type' : 'application/x-www-form-urlencoded',
    },
        body : <String, String>{
            'user_id' : 'user_id_value',
            'user_name' : 'user_name_value',
            'user_age' : 'user_age_value'
        });
    // POST 방식으로 하기 때문에 body에 데이터를 같이 전송해줌. 헤더 정보와 url 넣어줌
    if(response.statusCode == 200){ // 요청 코드가 200, 즉, 요청에 성공했다면
        print('전송 성공');
    }
    else{
        print('문제 발생');
    }
}
```

- Server - test1.php

```php
<?php
$id = $_POST["user_id"]; // user_id_value 
$name = $_POST["user_name"]; // user_name_value
$age = $_POST["user_age"]; // user_age_value;

include "../db.php";
	
$sql = ("INSERT INTO test_table (id, name, age) VALUES ('$id', '$name', '$age')");

$result = mysql_query($sql);
echo mysql_error();
?>
```



> 데이터 가져오기 GET

- Client

```dart
import 'package:http/http.dart' as http;
Future<void> _getRequest() async{
    String uri = 'http://xxx/get_data.php'; // 데이터를 가져올 uri
    
    final response = await http.get(uri+'?id=user_id_value', header : <String, String>{
        'Content-Type' : 'application/x-www-form-urlencoded',
    });
    // get method로 요청 파라미터를 uri에 붙임 + 헤더 정보도 추가
    if(response.statusCode == 200){ // 요청 결과 코드가 200, 즉 성공이라면
        print(response.body); // 결과 내용 출력
   		// body에 결과 값이 들어 있음. 이 데이터를 가공하면 됨
    }
}
```

- Server - get_data.php

```php
<?php
include "../db.php"; // DB에 연결 (db.php는 DB에 연결하는 코드)

$id = $_GET["id"]; // GET METHOD로 값을 가져옴

$sql = ("SELECT age FROM test WHERE id = '$id'"); // DB에 query 실행
$result = mysql_query($sql); // 쿼리문 결과
echo mysql_fetch_row($result)[0]; // 결과 가공
echo mysql_error();

?>
```



> 이미지 파일 전송하기 (or Binary files)

- multipart/form-data에 대한 작업

- MultipartRequest 이용

  ```dart
  import 'package:http/http.dart' as http;
  ...
  var request = http.MultipartRequest('POST',Uri.parse('uri'));
  ```

  - 이 요청은 Content-Type 헤더를 "**multipart/form-data**"로 자동으로 설정함
  - 이 요청은 문자열과 같은 일반적인 필드가 포함된다.
  - 동시에, Potentially streamed 이진 파일(Binary files)를 포함한다. 

- 리턴 받은 MultipartRequest 타입의 request 객체의 fields 멤버로 일반적인 문자열 필드를 전송할 수 있다.

  - fields는 Map<String,String> 타입

  ```dart
  request.fields['name'] = 'test_name';
  request.fields['price'] = '1,000';
  ```

- 이미지 파일 포함시키기

  ```dart
  var picture = await http.MultipartFile.fromPath("field", imageFile.path, filename : 'imageTest.jpg');
  
  // 1st param : 필드라고 하는데 정체를 모르겠음
  // 2nd param : 이미지 파일의 경로에 대한 문자열(File or XFile 객체의 path 멤버)
  // 3rd param :  이미지 파일의 이름을 변경하고 싶을 때는 filename 속성에 입력
  
  ```

  - request 객체의 멤버인 files(이 요청에 대해 업로드할 파일 리스트) -> List&lt;MultipartFile&gt;
  - 의 add 메소드를 통해 위의 MultipartFile 객체, 즉, 이미지 파일을 추가한다. 

  ```dart
  request.files.add(picture);
  ```

- 요청을 서버로 전송 - send() 메소드 이용

  ```dart
  var response = await request.send();
  // response 타입은 StreamedResponse
  ```

- 요청에 대한 응답 데이터 받기

  ```dart
  var responseData = await response.stream.toBytes();
  var responseString = String.fromCharCodes(responseData);
  print(responseString);
  ```

- 전체 code

  ```dart
  uploadImage(imageFile) async{
      var request = http.MultipartRequest('POST',Uri.parse('uri'));
      request.fields['name'] = 'test_name';
      request.fields['price'] = '1,000';
      
      var picture = await http.MultipartFile.fromPath('field', imageFile.path, filename : 'imageTest.jpg');
      request.files.add(picture);
      
      var response = await request.send();
      
      if(response.statusCode == 200){
          var responseData = await response.stream.toBytes();
          var responseString = String.fromCharCodes(responseData);
          print(responseString);
      }
  }
  ```

- Server - save_image.php

  ```php
  <?php
  $name = $_POST['name']; // request.fields['name'] 
  $price = $_POST['price']; // request.fields['price'] 를 통해 전송한 필드 값
  // 즉, $name은 test_name이고, $price는 1,000의 값을 가진다. 
  
  $image_name = $_FILES['userfile']['name'];
  // 이미지 파일의 이름; $image_name : imageTest.jpg
  $target = '/host/a/b/c'.$img_name;
  // 서버에 저장할 이미지 파일의 경로를 설정
  $image_type = $_FILES['userfile']['type'];
  // 파일의 mime 형식? : application/octet-stream 
  $image_size = $_FILES['userfile']['size'];
  // 이미지 파일의 크기를 바이트로 표현한 크기 : 3057482
  $temp = $_FILES['userfile']['tmp_name'];
  // 서버에 저장될 때 임시 파일에 저장되므로 그 임시 파일 경로 및 이름
  // /tmp/php5qWa1Y
  $error =$_FILES['userfile']['error'];
  // 파일 업로드에 관련한 에러 코드
  
  move_uploaded_file($temp, $target);
  // 임시 파일을 저장할 경로에 이미지 파일을 저장(이동)하는 함수 
  
  $delete = unlink($temp); // 임시 파일 제거(?)
      
  /* 
  $_FILES의 첫번째 [ ]에 들어가는 값은 파일 업로드 이름(?)인데
  어떠한 이름이라도 가질 수가 있다고 한다.
  */
  ?>
  ```

  

- Response 객체 멤버
  1. body - 요청의 결과 값이 들어가 있음 검색결과, 데이터 전송 성공 여부, 추가로 화면에 뿌려지는 것들
  2. statusCode - http 요청 코드
  3. headers - 헤더 정보
  4. contentLength - 내용 길이
     - 시행 착오 : 왜 respone.body에 헤더 정보까지 왜 같이 딸려오는가..

<hr>

- php에 존재하는 추가 기능

1. json_encode
2. curl
3. array
4. date
5. mysql_num_rows
6. str_replace
7. explode
8. sizeof