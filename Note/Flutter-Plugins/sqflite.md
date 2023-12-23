<h2>Flutter Plugins</h2>

<hr>

<h2>sqflite</h2>

<hr>

- SQLite의 플러터 버전 - 데이터베이스 시스템 (내부 저장소)
- Raw SQL 쿼리문을 작성할 수도 있고, DB Helper를 이용해서 작성할 수도 있다. 
- Raw SQL이 더 편하기 때문에..

- sqflite를 사용하려면 "path" 라이브러리도 필요하다. 

>  데이터베이스 열기 

- 파일내에 데이터베이스 파일(.db)의 경로를 가져와야한다.

```dart
String dbPath = join(await getDatabasePath(), '데이터베이스 파일 이름');
```

- 그 경로에 대해서 그 파일에 접근하여 열어야 한다.

```dart
Database database = await openDatabase(dbPath, onCreate : (db, version){
    return db.execute('CREATE TABLE IF NOT EXISTS test (a INTEGER PRIMARY KEY, b TEXT)');
    // 테이블 생성 SQL문
}, version : 1);
```

- await 키워드를 사용하였기 때문에, 또한 파일 입출력과 같은 데이터베이스 접근에 대한 작업은 비동기로 이루어져야 하기 때문에 위의 문장을 호출하는 함수는 **async** 키워드가 붙어야한다. 

- 이 데이터베이스 열기 작업은 앱 중간에 실행하는 것이 아닌 처음에 실행되어져야 하기 때문에 
- **initState()** 메소드에 위의 작업을 호출한다. 
- 또한, 비동기로 이루어져야 하므로 Future 오브젝트를 반환하므로 이 데이터베이스 열기 작업은 Future 함수내에서 이루어진다.

```dart
Database database;
@override
void initState(){
    super.initState();
    _openDB();
}

Future<void> _openDB() async{
    String dbPath = join(await getDatabasePath(), '데이터베이스 파일 이름');
    database = await openDatabase(dbPath, onCreate : (db, version){
    	return db.execute('CREATE TABLE IF NOT EXISTS test (a INTEGER PRIMARY KEY, b TEXT)');
    // 테이블 생성 SQL문
	}, version : 1);
}
```



> 데이터베이스 데이터 삽입 (INSERT)

- 위에서 데이터베이스를 열어서 Database 클래스의 객체에 값을 받았으면 이 객체로 SQL INSERT문을 작성한다.
- 또한, 이 데이터 삽입 과정도 비동기적으로 진행되어야 하기 때문에, 이 작업을 호출하는 함수는 **async**여야한다. 

```dart
await database.transaction((txn) async {
    int id1 = await txn.rawInsert(
    	'INSERT INTO test(a,b) VALUES(?,?)',
        [
            10,
            'Hello World'
        ]
    );
});
```

- rawInsert메소드는 첫번째 인자로 SQL INSERT문을 작성하는 것이고, 두번째 인자로는 각 값에 넣을 인자 값을 리스트로 넣어줘야한다. 
- INSERT문에서 무슨 값을 넣을지는 ?로 대체하면된다. 두번째 인자의 리스트에 각각 맵핑됨
- 이 rawInsert() 메소드는 정수 값을 리턴하는데 "마지막으로 삽입한 행(row) ID(몇번째 행인가)를" 리턴한다. 



> 데이터베이스 데이터 업데이트(UPDATE)

- 마찬가지로 SQL UPDATE문을 작성하는 것이다. 

```dart
await database.transaction((txn) async{
   int count = await database.rawUpdate(
    	'UPDATE test SET a = ? WHERE b = ?',
        [
         	100,
            'Hello World'
        ]
    );
});
```

- INSERT와 마찬가지로 첫번째 인자는 SQL UPDATE문, 두번째 인자로는 각각 ?에 맵핑될 값을 넣는다. 
- 이 rawUpdate() 메소드는 정수 값을 리턴하는데, "변경된 사항의 수"를 리턴한다. 



> 데이터베이스 데이터 검색, 쿼리문(SELECT)

- 마찬가지로 SQL SELECT문을 작성하는 것이다.
- 이번에는 결과 값(검색한 결과)를 가져오는 것이므로 이 작업을 하면 리턴을 받아야한다.
- 이 때 리턴받는 값의 타입은 **List&lt;Map&gt;**타입이다. 
- => 발견된 행(row)의 리스트를 반환함. 

```dart
List<Map> result = await database.rawQuery(
	'SELECT * FROM test WHERE a = ?',
    [
        100
    ]
);
```

- Map은 key값을 인덱스로 접근하기 때문에 리턴받은 List&lt;Map&gt;의 각 인덱스를 key값, 즉, 테이블의 Attribute값으로 접근해야한다. 

```dart
print(result[0]['b']);
//결과 : "Hello World"
```

- 리스트의 의미는 행(row)의 개수만큼의 배열(리스트)라는 뜻이고
- 각 리스트의 인덱스는 Map 타입의 데이터가 들어있는 것이다. 
- 각 인덱스의 Map타입의 데이터중 원하는 데이터를 갖고 오려면 DB 테이블의 Attribute 값으로 접근하면 된다. 



- 검색 쿼리문 중에서 WHERE절에서 **LIKE**를 사용하고 싶다면

```dart
List<Map> list = await database.rawQuery(
	"SELECT * FROM test WHERE b LIKE '%${temp_text}%'"
);
```

- 위와 같이 사용하면 된다. (두번째 인자값에 값을 넣을 필요 없이 쿼리문에 바로 값 전달)





> 데이터베이스 집계함수 사용법 (Aggregate function)

- 집계함수를 사용하면 결과 값이 단 하나만 나오므로 
- 변수 값에 저장할 수 있다.

```dart
int count = Sqflite.firstIntValue(await database.rawQuery('SELECT COUNT(*) FROM test'));
```

