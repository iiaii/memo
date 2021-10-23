# Replication

리플리케이션 블로그에 다시 정리함 -> [리플리케이션(Replication)](https://iiaii.tistory.com/7)

사용자가 많아지고 하나의 DB에서 많은 쿼리를 처리하기에는 힘든 상황이 발생한다.
쿼리의 대부분이 Select 즉, 데이터를 조회하는 것인데 이를 해결하기 위해 Replication이라는 방법이 나오게 되었다.
또한 데이터의 안정성을 위해 복제본을 갖는다는 의미도 있다. 물론 약간의 딜레이로 데이터 손실이 있지만 마스터/슬레이브 구조로 마스터 서버와 동일한 데이터를 유지하기 때문에 데이터 소실이 최소화 된다.
리플리카 서버는 마스터 서버로 승격이 가능하기 때문에 기존 마스터 서버를 대체하는 방식으로 복구가 진행된다.

``Replication이란 2개 이상의 DBMS 시스템을 Master / Slave 로 나누어서 동일한 데이터를 저장하는 방식이다``

![replication](https://nesoy.github.io/assets/posts/20180216/2.png)


Master DBMS에는 데이터의 수정사항을 반영만하고 Replication해서 Slave DBMS에 실제 데이터를 복사한다.

##### 데이터 복사 방법 [로그 기반 복제 (Binary Log)]

- Statement Based : SQL 문장을 복사하여 진행, SQL에 따라 결과가 달라지는 경우 Timestamp, UUID ...
- Row Based : SQL에 따라 변경된 Row 라인만 기록하는 방식, 데이터가 많이 변경된 경우 데이터가 커질 수 밖에 없다
- Mixed : 기본적으로 Statement Based 로 진행하면서 필요에 따라 Row Based를 사용한다


### 장점

![advantage](https://nesoy.github.io/assets/posts/20180216/3.png)

실제로 쿼리의 대부분은 Select이다. 이 부하를 낮추기 위해 Slave Database를 생성하게 된다면 조회 성능을 향상 시킬 수 있다.
또한 Master Database 영향 없이 로그를 분석할 수 있다. 


> 정리하자면, 리플리카 서버는 기본적으로 읽기전용으로 운용되어 데이터베이스 서버의 부하를 줄이고 데이터베이스를 접근하는 어플리케이션의 동작 성능을 개선할 수 있다. 안정성과 속도를 모두 확보할 수 있다.


---
- [리플리케이션이란](https://www.coovil.net/db-replication/)
- [DB 리플리케이션](https://nesoy.github.io/articles/2018-02/Database-Replication)
- [MySQL Replication](http://cloudrain21.com/mysql-replication)
