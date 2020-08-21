# Annotation Processor


### Lombok

```java
@Getter @Setter
public class Member {
    private String name;
    
    private String age;
}
```
Lombok은 어노테이션을 추가하는 것 만으로 실제 바이트 코드에 Getter, Setter 코드가 생성된다.
하지만 Lombok의 Retention은 Source이다. 

```java
@Target({ElementType.FIELD, ElementType.TYPE})
@Retention(RetentionPolicy.SOURCE)
public @interface Getter {
    ...
}
```

즉, Lombok 어노테이션은 현재 소스파일에만 존재할 뿐 바이트코드나 런타임까지 존재하지 않는다. 
따라서 Lombok은 컴파일 시점에 Anotation Processor가 소스코드의 [AST(Abstract Syntax Tree)](https://javaparser.org/inspecting-an-ast/)를 조작한다. 
하지만 공개된 API가 아닌 컴파일러 내부 클래스를 사용하여 기존 소스코드를 조작하기 때문에 버전 호환성에 문제가 생길 수 있고(필연적으로), 기존 코드를 조작하는 것 때문에 해킹이다 아니다에 대한 논쟁도 있다고 한다.
 
  
> 어쨋거나 엄청난 편의성 때문에 이미 널리 사용되고 있음


---
### 예제

- AbstractProccessor 
- Filer
- AutoService
- Javapoet (프로그래밍으로 소스코드 생성할때)

```java
@AutoService(Processor.class)
public class MagicMojaProcessor extends AbstractProcessor {

    @Override
    public Set<String> getSupportedAnnotationTypes() {
        return Set.of(Magic.class.getName());
    }

    @Override
    public SourceVersion getSupportedSourceVersion() {
        return SourceVersion.latestSupported();
    }

    @Override
    public boolean process(Set<? extends TypeElement> set, RoundEnvironment roundEnvironment) {
        Set<? extends Element> elements = roundEnvironment.getElementsAnnotatedWith(Magic.class);
        for (Element element : elements) {
            Name elementName = element.getSimpleName();
            if (element.getKind() != ElementKind.INTERFACE) {
                processingEnv.getMessager().printMessage(Diagnostic.Kind.ERROR, "Magic annotation can not be used on " + elementName);
            } else {
                processingEnv.getMessager().printMessage(Diagnostic.Kind.NOTE, "Processing "+elementName);
            }

            TypeElement typeElement = (TypeElement) element;
            ClassName className = ClassName.get(typeElement);

            MethodSpec pullOut = MethodSpec.methodBuilder("pullOut")
                    .addModifiers(Modifier.PUBLIC)
                    .returns(String.class)
                    .addStatement("return $S", "Rabbit!")
                    .build();

            TypeSpec magicMoja = TypeSpec.classBuilder("MagicMoja")
                    .addModifiers(Modifier.PUBLIC)
                    .addSuperinterface(className)
                    .addMethod(pullOut)
                    .build();

            Filer filer = processingEnv.getFiler();
            try {
                JavaFile.builder(className.packageName(), magicMoja)
                        .build()
                        .writeTo(filer);
            } catch (IOException e) {
                processingEnv.getMessager().printMessage(Diagnostic.Kind.ERROR, "FATAL ERROR: " + e);
            }
        }
        return true;
    }
}
```
 
 
---
인프런 | 더 자바, 코드를 조작하는 다양한 방법 (백기선)
