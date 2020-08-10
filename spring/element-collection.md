# @ElementCollection

![element-collection](http://www.thejavageek.com/wp-content/uploads/2014/01/EmployeeAndCourses.png)

해당 어노테이션이 붙으면 Jpa 구현체는 모든 컬렉션의 데이터를 모두 삭제하고 다시 모든 데이터를 추가한다. 
PK가 존재하지 않아 발생하는 현상이지만, @OrderColumn 을 사용하면 순서값을 같이 저장해서 조회할때 사용하기 때문에 모두 삭제하는 문제를 방지할 수 있다고 한다. (확실한지 확인 필요)


@ElementCollection 을 사용하면 DB에서는 별도의 테이블이 구성된다. (ElementCollectio n값은 항상 별도의 테이블에 저장)
(논리적으로는 한 엔티티 안이지만 DB에서는 별개의 테이블이 생성되어 관리된다. 코드상(논리적으로) 분리되어 있지만 한 테이블에 관리되는 Embedded 타입과 상반된 경우이다) 
별도의 테이블에 의해 관리되는 것으로 인한 성능 문제가 신경쓰인다면 실제 테스트를 해봐야겠지만 `String.join()` 를 이용해서 `,` 같은 구분자로 구분하는 전용 컨버터 클래스를 만들어서 String 형태로 저장 가능하다. (이 방식 역시 변환에서 발생하는 오버헤드를 고려해야 한다)


[jpa element collection](http://www.thejavageek.com/2014/01/31/jpa-elementcollection-annotation/)

[element collection - wikibooks](https://en.wikibooks.org/wiki/Java_Persistence/ElementCollection)
