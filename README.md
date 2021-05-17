# 11-Weekly-Report
교수님 코드 다운받아서 돌려보실때 브랜치를 master로 checkout하시고, sync project with gradle files 버튼을 누르셔야합니다. 카카오연동로그인은 노트북마다 keyhash를 입력해야 AVD에서 확인 가능합니다.


### 이번주차 활동

저희 앱은 큐알코드를 스캔하여 앱 속에서 주문을 할 수 있는 앱입니다. 이 앱을 사용하기 위해서는 각 카페의 주문서 정보가 들어있는 큐알코드 생성이 필요하고 소비자의 앱구현과 주문 정보를 받는 포스기 구현이 필요합니다. 저번주차까지 소비자가 사용하는 앱의 QR코드 스캐너와 카카오톡 연동까지 구현하였습니다.

이번주차는 

- 큐알 생성코드 작성
- 주문서 작성
- 데이터베이스 사용방식 조사

을 마무리하여 전체 프로젝트의 40% 정도 진행되었습니다. 다음주차에는 데이터베이스 연동 및 앱의 추가기능을 생각해볼 예정입니다.

##### 큐알코드생성 <br>
<img src="https://user-images.githubusercontent.com/75411735/118488281-8c121d80-b756-11eb-920b-40ed0ec97657.png" width="300" height="500">

##### 주문서코드<br>
<img src="https://user-images.githubusercontent.com/75411735/118487564-d050ee00-b755-11eb-8192-30a3104da487.png" width="500" height="400">

##### 주문서가 담겨있는 큐알코드를 앱으로 스캔하면 앱에 해당 카페의 주문서가 보여진다.<br>
<img src="https://user-images.githubusercontent.com/75411735/118484948-c4aff800-b752-11eb-8847-d29c4a43f9b8.png" width="300" height="500">

#### <데이터베이스 조사>

##### DB테이블 준비
- 테이블과 색인 등을 생성하려면 생성하길 원하는 DB Scheme에 대한 DDL 구문을 준비해서 SQLiteDatabase 인스턴스의 execSQL() 메소드의 인자로 넣어 호출.
ex) db.execSQL("CREATE TABLE constants (_id INTEGER PRIMARY KEY AUTOINCREMENT, title TEXT, value REAL);");

##### 데이터 추가
1. execSQL( ) 메소드를 사용하는 방법이다. 이는 값을 가져오는 문장이 아닌 INSERT, UPDATE, DELETE등의 모든 SQL 구문을 String 타입의 인자로 넣어 처리할 수 있다.
2. SQLiteDatabase 클래스에서 제공하는 insert( ), update( ), delete( ) 등의 개별 메소드를 사용하는 방법이 있다. 이와같은 개별 메소드는 인자로 입력받은 값을 조합해 최종적으로 SQL 문장을 동일하게 실핼하도록 만들어져 있다. 개별 메소드는 Map과 비슷한 구조로 만들어져 있으면서 SQLite의 데이터 타입에 맞춰 동작하도록 구성된 ContentValues 객체를 사용해 동작한다.
그리고 지정한 키에 해당하는 값을 찾아올때는 단순하게 get() 메소드를 사용하는 대신, getAsInteger( ), getAsString( ) 등의 메소드를 호출하면 된다.

##### 데이터 불러오기
rawQuery( ) 메소드를 사용해 SELECT 구문을 직접 실행하는 방법이고, 두번째 방법은 query( ) 메소드의 인자로 각 부분의 값을 넘겨 실행하는 방법이다. SQLiteQueryBuilder 클래스와 관련된 부분과 커서와 커서 팩토리에 관한 부분이 가장 복잡하다.
API 호출 방법만 놓고 보면 rawQuery() 메소드를 사용하는 방법이 가장 간단하다. rawQuery() 메소드에 SELECT 구문을 인자로 넘겨 주기만 하면 되기때문이다.

SQLiteQueryBuilder 클래스를 활용하면 훨씬 다양한 방법으로 UNION이나 하위 쿼리 등을 포함하는 복잡한 구문을 생성할 수 있다. SQLiteQueryBuilder 클래스가 ContentProvider 인터페이스와 완벽하게 맞아 떨어진다. 컨텐트 프로바이더의 query() 메소드를 구현하는 가장 일반적인 방법은 SQLiteQueryBuilder 인스턴스를 생성하고 기본값을 일부 채워넣은 다음 전체 쿼리를 생성하고 실행할수 있게 구성하는 것이다. 

1. SQLiteQueryBuilder 인스턴스를 생성함.
2. 쿼리에 사용할 테이블 이름을 설정함 -> setTables(getTableName( ))
3. 값을 가져올 기본 컬럼 이름의 목록을 지정하거나 (setProjectionMap( )), 또는 Uri값에 들어 있는 ID값으로 테이블 항목 가운데 특정한 값을 가져올 수 있도록 WHERE 구문을 추가했다. -> (appendWhere( ))
4. 마지막으로 기본값과 요청이 들어온 값을 조합해 생성된 쿼리 구문을 실행한다. -> qb.query(db, projection, selection, selectionArgs, null, null, orderBy)

##### 커서 활용
Cursor의 대표적인 메소드
* getCount() 메소드 : 전체 결과 건수가 몇개인지 확인할 수 있다.
* moveToFirst(), moveToNext(), isAfterLast() 등의 메소드 : 결과건을 모두 확인할수있다.
* getColumnNames() 메소드 : 결과에 포함된 전체 컬럼 이름을 알수 있다.
* requery() 메소드 : 쿼리를 재실행 할수 있다.
* close() 메소드 : 커서가 확보한 자원을 모두 해제한다.



