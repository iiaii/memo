# Reflection

클래스 로더 프로세스에서 크게 3가지를 진행한다.

- 로딩 (바이트 코드를 읽고 메모리에 배치)
- 링크 (레퍼런스를 연결)
- 초기화 (static 값들 초기화)

여기서 로딩하는 시점에 생성되는 클래스의 Class 타입 객체를 생성하여 힙에 배치하는데 Class 클래스에서 제공하는 메서드를 Reflection API라고 한다.


---
### 활용되는 곳

- 의존성 주입 (DI)
- MVC 뷰에서 넘어온 데이터를 객체에 바인딩 할 때
- @Entity 클래스에 Setter가 없는 경우 리플렉션을 사용
- JUnit


---
### DI

DI는 Reflection을 사용하여 구성되어 있다. 간단하게 @Inject라는 커스텀 어노테이션으로 DI를 구현한다면 다음과 같다.

```java

// @Inject가 포함된 클래스인지 확인하고 의존 객체를 주입해 준다
public static <T> T getObject(Class<T> classType) {
    T instance = createInstance(classType);
    Arrays.stream(classType.getDeclaredFields())
            .forEach(f -> {
                if (f.getAnnotation(Inject.class) != null) {
                    Object fieldInstance = createInstance(f.getType());
                    f.setAccessible(true);
                    try {
                        f.set(instance, fieldInstance);
                    } catch (IllegalAccessException e) {
                        throw new RuntimeException(e);
                    }
                }
            });
    return instance;
}

// 인스턴스 생성
private static <T> T createInstance(Class<T> classType) {
    try {
        return classType.getConstructor(null).newInstance();
    } catch (InstantiationException | IllegalAccessException | InvocationTargetException | NoSuchMethodException e) {
        throw new RuntimeException(e);
    }
}

```

---
### 주의사항

- 지나친 사용은 성능 이슈를 야기할 수 있으므로 반드시 필요한 경우에만 사용해야 한다
- 컴파일 타임에 확인되지 않으면서 런타임 시에만 발생하는 문제를 만들 가능성이 있다
- 접근 지시자를 무시할 수 있다



