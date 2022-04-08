# IoC/DI 개념

제어의 역전(IoC, Inversion of Control)은 **프로그램의 제어 흐름을 직접 제어하는 것이 아닌 외부에서 관리**하는 것을 의미한다.

- IoC는 클래스를 느슨하게 결합하게 할 수 있도록 해 테스트 및 유지보수를 편리하게 도와주는 설계 원리이다.
- 의존 관계 주입(DI, Dependency Injection)은 IoC를 구현하는 디자인 패턴 중 하나이다.

## 의존 관계 주입(DI, Dependency Injection)

의존 관계 주입에서 **의존**은 프로그래밍 관점에서의 의존을 말한다.

```java
public class A {
  private B b;
}
```

한 클래스가 다른 클래스를 사용할 때 **의존**한다고 표현한다.

- 의존은 변경에 의해 영향을 받는 관계를 의미한다.
- 의존 관계에는 방향성이 있다. A는 B를 의존하고 있지만, B는 A를 의존하고 있지않다.
- A가 변해도 B는 영향을 받지 않지만 B가 변하면 A가 영향을 받는다.

의존 관계 주입은 개발자가 직접 `new`를 이용해 의존 객체를 생성하는 것이 아닌 **외부에서 의존 객체를 주입**해주는 것을 말한다.

```java
interface MovieFinder { ... }
class ActionMovieFinder implements MovieFinder { ... }

public class MovieListener {
  // 의존 관계 직접 생성
  private MovieFinder movieFinder = new ActionMovieFinder();
}
```

`new`를 이용해 의존 객체를 직접 생성하는 것은 클래스를 강하게 결합시킨다는 문제점이 있다.

- 만약 다른 장르의 영화를 찾고자 한다면 MovieListener를 수정해야 한다. 즉, 변경의 유연성이 떨어진다.
- 다른 장르의 영화를 찾기위해 멤버만 다르고 중복되는 MovieFinder 클래스를 추가하는 것은 좋지 못하는 방법이다.

위와 같은 문제를 **의존 관계 주입**을 통해 해결할 수 있다.

```java
interface MovieFinder { ... }
class ActionMovieFinder implements MovieFinder { ... }
class SIFIMovieFinder implements MovieFinder { ... }

public class MovieListener {

  private final MovieFinder movieFinder;
  
  // 생성자를 통한 의존성 주입
  public MovieListener(MovieFinder movieFinder) {
    this.movieFinder = movieFinder;
  }
  
}
```

의존 관계 주입을 사용하면 MovieListener는 MovieFinder의 구현체가 무엇인지 관계 없이 MovieFinder로 모든 요청을 처리할 수 있다.

- 클래스간 직접적인 의존 관계를 맺는 것보다 인터페스를 통해 의존 관계를 맺어 결합도를 낮춤으로써 변경에 유연함을 더해줄 수 있다.
- 다른 장르의 영화를 찾기 위해 새로운 구현체를 추가해도 전혀 문제가 없다.

MovieListener에 MovieFinder를 주입하기 위해서는 애플리케이션 실행 시점에 필요한 객체를 생성해야 한다.

- 실행 시점에 필요한 의존 객체를 생성하고 주입하는 역할을 스프링에서는 DI Container가 대신 해준다. 
- 이러한 개념을 제어의 역전(IoC, Inversion of Control)이라고 한다.

---

#### 참고

- <https://galid1.tistory.com/493?category=769011>
