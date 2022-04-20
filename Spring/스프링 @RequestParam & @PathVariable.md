## @RequestParam

쿼리 파라미터의 특정 키에 대한 값을 `@RequestParam`을 사용해 파라미터를 조회할 수 있다.

```java
@GetMapping("/hello")
public String hello(@RequestParam("name") String name) {
  ...
}
```

- `/hello?name=yoon`으로 요청이 올 경우 쿼리 파라미터의 `name` 키에 대한 값인 `yoon`을 받아 올 수 있다.
- 쿼리 파라미터의 키 이름과 변수 이름이 같으면 `@RequestParam`의 `name` 속성을 생략해도 된다.
- `String`, `int`, `Integer` 등의 단순한 타입의 경우 `@RequestParam`도 생략이 가능하다.
- 주의할 점은 `required` 속성의 기본 값이 `true`이기 때문에 파라미터가 필수로 있어야한다. (없으면 예외 발생)
- 쿼리 파라미터의 키만 사용해서 요청을 보낼 경우 빈 문자로 통과된다. ex) `/hello?name=`
- 파라미터가 없어도 동작하게 하려면 `required` 속성을 `false`로 설정하는 것이 좋다.

`defaultValue`를 사용해 파라미터의 기본 값을 정해 줄 수 있다. 

- 기본 값이 이미 있기 때문에 `required` 속성은 의미가 없다.
- 빈 문자로 요청이 올 경우에도 `defaultValue`에 설정한 기본 값이 적용된다.

```java
@GetMapping("/hello")
public String hello(@RequestParam(value = "name", required = false, defaultValue = "yoon") String name) {
  ...
}
```

파라미터가 여러 개 오는 경우 `@RequestParam`을 사용하게 된다면 가독성이 매우 떨어진다.

- `Map`을 사용해 여러 개의 파라미터를 받을 수 있다.
- 하나의 키에 여러 값을 받을 때는 `MultiValueMap`을 사용하는 것이 좋다.

```java
// /hello?name=yoon&age=19
@GetMapping("/hello")
public String hello(Map<String, String> params) {
  String name = params.get("name");
  String age = params.get("age");
  ...
}

// /hello?name=yoon&name=min&name=soo
@GetMapping("/hello")
public String hello(MultiValueMap<String, String> params) {
  // [yoon, min, soo]
  List<String> values = params.get("name");
  ...
}
```

## @PathVariable

요즘에는 리소스 경로에 식별자를 넣는 스타일을 많이 볼 수 있다. ex) `/users/1` 

- 스프링에서 제공하는 `@PathVariable`를 사용해 요청 URI 매핑에서 템플릿 변수를 처리하고 메서드 매개 변수로 설정할 수 있다.

```java
@GetMapping("/usres/{id}")
public String findUserById(@PathVariable(name = "id") Long id) {
  ...
}

// 두 개 이상의 경로 변수도 사용가능
@GetMapping("/users/{id}/{email}")
public String findUserByEmail(@PathVariable(name = "id") Long id, @PathVariable(name = "email") String email) {
  ...
}
```

경로 변수가 여러 개 오는 경우 `@PathVariable`을 사용하면 가독성이 매우 떨어진다.

- `Map`을 사용해 여러 개의 경로 변수를 받을 수 있다.

```java
@GetMapping("/users/{id}/{email}")
public String findUserByEmail(@PathVariable Map<String, String> pathVarsMap) {
  String id = pathVarsMap.get("id");
  String email = pathVarsMap.get("email");
  ...
}
```

사용법은 `@RequestParam`과 비슷하지만 주의할 점이 있다.

- `required` 속성의 기본 값이 `true`이기 때문에 값이 반드시 있어야한다.

```java
@GetMapping(value = {"/post", "/post/{num}"})
public String posts(@PathVariable(value = "num", required = false) Long num) {
  ...
}
```

- `required` 속성 값을 `false`로 주면 경로 변수가 있는 경우와 없는 경우를 처리할 수 있다.
- `@PathVariable`에는 `@RequestParam`처럼 `defaultValue`를 지정할 수 없다.
- 기본 값을 지정하려면 메소드 내에서 처리하거나 `Optional`을 사용하는 방법이 있다.

```java
@GetMapping(value = {"/post", "/post/{num}"})
public Stirng posts(@PathVariable(value = "num", required = false) Long num) {
  if (num == null) {
    num = 1L;
  }
  ...
}

@GetMapping(value = {"/post", "/post/{num}"})
public Stirng posts(@PathVariable(value = "num", required = false) Optional<Long> num) {
  if (num.isPresent()) {
    ... = num.get();
  }
  ...
}
```


---

#### 참고

- [인프런 | 스프링 MVC 1편 - 백엔드 웹 개발 핵심 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1/dashboard)
- [스프링 공식문서](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#spring-core)
- <https://www.baeldung.com/spring-pathvariable>