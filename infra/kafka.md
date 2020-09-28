# Kafka


![kafka](https://t1.daumcdn.net/cfile/tistory/99B7A03C5C20888D04)


> Consumer가 데이터를 가져가도 데이터는 삭제 되지 않는다. 만약 컨슈머 그룹이 다르고 `auto.offset.reset`이 earliest 이면 동일 데이터를 2번 처리할 수 있다 (카프카를 사용하는 큰 이유)

---
### 용어 정리

![kafka arch](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile26.uf.tistory.com%2Fimage%2F99745A4B5E633AF32148ED)


##### Zookeeper

- 클러스터 최신 설정정보 관리, 동기화, 리더 채택 등 클러스터의 서버들이 공유하는 데이터를 관리하기 위해 사용된다 (broker에 분산 처리된 메시지 큐의 정보들을 관리)
- 클러스터를 관리하는 Zookeeper가 있어야만 Kafka 구동이 가능하다 (Zookeeper를 먼저 실행해야 함)


##### Broker

- Kafka Server를 의미한다
- 한 클러스터 내에서 Kafka server를 여러대 띄울 수 있다


##### Topic

- 메시지가 생산되고 소비되는 주제로서 파일 시스템에서 폴더와 같은 역할을 한다고 보면된다. (데이터 별로 구분지어서 복잡도를 낮추기 위함)
- 주제에 따라 여러 Topic을 생성하면 된다 (email topic, sms topic)

##### Partition

- Topic 내에서 메시지가 분산되어 저장되는 단위로서 Partition이 3개라면 3개의 Partition에 대해 메시지가 분산되어 저장된다
- 기본적으로는 Round-Robin 으로 파티션이 지정되지만 키를 지정해서 특정 파티션에 위치하도록 할 수 있다
- 순서가 보장되어야 하는 경우에는 파티션이 1개여야 한다 (RR로 순서가 섞일 수 있음)
- 파티션을 늘리는 것은 가능하지만 줄이는 것은 불가능 하다 
- 파티션 데이터는 옵션에 따라 삭제되는 시점을 지정할 수 있다


---
### Producer

- Topic에 해당하는 메시지를 생성
- 특정 Topic으로 데이터를 publish
- 처리 실패/재시도 (성공 여부도 확인 가능)


```java
// 프로듀서 기본 형태 : 데이터 전송
public class Producer {
  public static void main(String[] args) throws IOException {
    
    Properties configs = new Properties();
    configs.put("bootstrap.servers", "localhost:9092");
    configs.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
    configs.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
    
    KafkaProducer<String, String> producer = new KafkaProducer<String, String>(configs);
    
    ProducerRecord record = new ProducerRecord<String, String>("click_log", "login");   // 어느 토픽에 넣을 것인지 선언
    
    producer.send(record);
    
    producer.close();
  }
}
```


```java
// 직접 키를 지정
producer.send(new ProducerData<String, String>("click_log", "1", "buy"));
producer.send(new ProducerData<String, String>("click_log", "2", "buy"));
```

키를 지정하고 파티션을 추가하면 키-파티션의 일관성이 보장되지 않는다.





---
[아파치 카프카](https://www.youtube.com/watch?v=waw0XXNX-uQ&list=PL3Re5Ri5rZmkY46j6WcJXQYRlDRZSUQ1j)
