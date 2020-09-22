## Database Key

MySQL 에서 테이블의 데이터를 구분하기 위한 키의 종류 정리


##### KEY

가장 일반적인 Key는 DB의 Index와 동의어로서, 데이터의 검색을 위해 Index를 색인으로 사용하므로 중요한 역할을 한다. 중복을 허용하며 NULL 등의 허용도 가능하지만 NULL 이 허용될 경우, 
색인에 있어 비약적인 성능저하를 가져오기 때무에 일반적으로 Nullable 데이터는 Indexing 하지 않는다.

> 단순히 Key, 즉 Index로만 지정할 경우에는 별도의 제약조건을 가지지 않기 때문에 테이블 내에 데이터에 대해 엄격한 정합성이 요구될 경우에는 적합하지 않다.


##### PRIMARY KEY

NULL을 허용하지 않고, Unique 성질을 지니며 테이블 당 단 하나의 정의만 가질 수 있다. (NOT NULL & UNIQUE) 
PK로 단 하나의 칼럼이 지정되어 있다면 해당 칼럼의 데이터는 Table 내에서 유일성이 보장되며 여러개가 PK로 지정되어 있다면 해당 Key들의 조합에 대해 유일성이 보장된다.

> PK는 행을 테이블 내에서 고유하게 만든다


##### UNIQUE KEY
 
Unique Key는 Uniqueness를 지닌 Index를 의미한다. (= Unique Index) PK처럼 중복성이 허요오디지 않지만 NULL에 대한 허용이 가능하다. 테이블 당 여러개를 가질 수 있다.



##### FOREIGN KEY


Foreign Key 란 JOIN 등으로 다른 DB 와의 Relation 을 맺는 경우, 다른 테이블의 PK를 참조하는 Column 을 FK 라고 한다.

 여기서 Foreign Key Relation 을 맺는 다는 의미는 논리적 뿐 아니라 물리적으로 다른 테이블과의 연결까지 맺는 경우를 말하며, 이 때 FK 는 제약조건(Constraint)으로의 역할을 한다.
 Foreign Key Restrict 옵션을 줄 수 있고 다음과 같은 옵션들이 있다.

  - RESTRICT : FK 관계를 맺고 있는 데이터 ROW 의 변경(UPDATE) 또는 삭제(DELETE) 를 막는다.
  - CASCADE : FK 관계를 맺을 때 가장 흔하게 접할 수 있는 옵션으로, FK 와 관계를 맺은 상대 PK 를 직접 연결해서 DELETE 또는 UPDATE 시, 상대 Key 값도 삭제 또는 갱신시킨다. 이 때에는 Trigger 가 발생하지 않으니 주의하자.
  - SET NULL : 논리적 관계상 부모의 테이블, 즉 참조되는 테이블의 값이 변경 또는 삭제될 때 자식 테이블의 값을 NULL 로 만든다. UPDATE 쿼리로 인해 SET NULL 이 허용된 경우에만 동작한다.
  - NO ACTION : RESTRICT 옵션과 동작이 같지만, 체크를 뒤로 미룬다.
  - SET DEFAULT : 변경 또는 삭제 시에 값을 DEFAULT 값으로 세팅한다.


> PK 가 설계 시 많이 이용되지만, 정합성이 중요하지 않은 경우 Index 만 이용해서 테이블을 설계하기도 한다. UNIQUE Index 는 주로 복잡한 테이블에서 부분적인 정합성을 살리기 위해 많이 이용된다. 그리고 FK 의 물리적 연결은 서버의 안정성을 위해 지양하는 편이 많다. 상황에 알맞게 적합한 Key 를 잡아 설계하는 것이 최선의 튜닝 기법이라고 생각된다.



---
[MySQL Keys](https://jins-dev.tistory.com/entry/MySQL-%EC%97%90%EC%84%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-Key-%EC%9D%98-%EC%A0%95%EC%9D%98%EC%99%80-%EC%A2%85%EB%A5%98%EB%93%A4%EC%97%90-%EB%8C%80%ED%95%98%EC%97%AC)
