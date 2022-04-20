# 요청 매핑

`@Controller(@RestController)`가 사용된 컨트롤러 클래스에 `@RequestMapping`을 사용해 요청을 처리할 메소드에 매핑할 수 있다.

- URL, HTTP 메소드, 요청 매개 변수, 헤더 및 미디어 유형별로 일치시킬 수 있는 다양한 속성을 가지고 있다.
- 클래스 수준에 작성해 공유 매핑을 표현하거나 메소드 수준에서 사용해 특정 매핑으로 범위를 좁힐 수 있다.

```java
@RestController
@RequestMapping("/hello")
public class HelloController {

  // GET, PUT 방식인 '/hello/world' 요청을 처리하는 메소드
  @RequestMapping(value = "/world", method = {RequestMethod.GET, RequestMethod.PUT})
  public String helloFoo() {
    ...
  }

  // POST 방식 '/hello/foo', '/hello/bar' 요청을 처리하는 메소드
  @RequestMapping(value = {"/foo", "/bar"}, method = RequestMethod.POST)
  public String helloBar() {
    ...
  }

  // method 속성 생략 시 모든 HTTP 메소드를 처리 한다.
  @RequestMapping(value = "/good")
  public String helloWorld() {
    ...
  }

}
```

- 여러 경로를 한 메소드에서 처리하고 싶다면 배열로 경로 목록을 지정하면 된다.
- 클래스 레벨에 `@RequestMappging`이 있으면 메소드 레벨과 조합해 사용된다.
- 위 예제의 경우 `/hello/world`, `/hello/foo`, `/hello/bar` 요청을 처리한다.
- 특정 HTTP 메소드를 구분해서 처리할 수 있다. (GET, POST, HEAD, OPTIONS, PUT, PATCH, DELETE, TRACE)
- 요청 파라미터 또는 헤더도 조회해 활용할 수 있다.

스프링 4.3 부터는 `@RequestMapping`의 축약형 어노테이션이 제공된다.

- @GetMapping
- @PostMapping
- @PutMapping
- @DeleteMapping
- @PatchMapping

위 어노테이션은 모두 `@RequestMapping(method = RequestMethod.TYPE)`을 포함하고 있다.

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@RequestMapping(method = RequestMethod.GET)
public @interface GetMapping {
  ...
}

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@RequestMapping(method = RequestMethod.POST)
public @interface PostMapping {
  ...
}
```

- `@GetMapping("/hello")`과 `@RequestMapping(value = "/hello", method = RequestMethod.GET)`은 같은 기능을 한다.
- HTTP 메소드를 축약한 어노테이션을 사용하는 것이 `@RequestMapping`을 사용하는 것보다 더 직관적이다.
- @RequestMapping이 가지고 있는 속성을 모두 사용할 수 있다. (method는 이미 적용되어 있기 때문에 사용할 수 없다)

## 파라미터

`params` 속성을 사용해 특정 파라미터의 존재 여부를 조건으로 추가할 수 있다. (거의 사용하지 않는다)

```java
// 쿼리 파라미터에 "키 = 값"을 가지고 있는 경우 ex) /hello?name=yoon
@GetMapping("/hello", params = "name=yoon")
public String hello() {
  ...
}
```

- 예제의 경우 `/hello?name=yoon`으로 요청이 올 경우 `hello()` 메소드가 동작한다.
- 쿼리 파라미터에 특정 키를 가지고 있는 경우 ex) `params = "name"`
- 쿼리 파라미터에 특정 키를 가지고 있지 않는 경우 ex) `params = "!name"`

## 헤더

`params`처럼 `headers`를 사용해 특정 헤더의 존재여부를 확인해 요청을 처리할 수 있다.

```java
@GetMapping(value = "/hello", headers = "key=value")
public String findUser() {
  ...
}
```

- 특정한 헤더가 있는 요청을 처리하는 경우 `headers = "key`
- 특정한 헤더가 없는 요청을 처리하는 경우 `headers = "!key`
- 특정한 헤더 키/값이 있는 요청을 처리하는 경우 `headers = "key=value"`

## 미디어 타입

특정한 타입의 데이터를 담고 있는 요청만 처리하는 핸들러 메소드는 `consumes` 속성을 사용하면 된다.

```java
@GetMapping(value = "/hello", consumes = MediaType.APPLICATION_JSON_VALUE)
public String hello() {
  ...
}
```

- HTTP 요청의 `Content-type` 헤더를 기반으로 미디어 타입을 매핑한다.
- 매칭 되지 않는 경우 `415 Unspported Media Type`을 반환한다.

특정한 타입의 응답을 만드는 핸들러 메소드는 `produces` 속성을 사용하면 된다.

```java
@GetMapping(value = "/hello", produces = MediaType.APPLICATION_JSON_VALUE)
public String hello() {
  ...
}
```

- HTTP 요청의 `Accept` 헤더를 기반으로 미디어 타입을 매핑한다.
- 매칭 되지 않는 경우 `406 Not Acceptable`을 반환한다.

---

#### 참고

- [초보 웹 개발자를 위한 스프링 5 프로그래밍 입문](http://www.yes24.com/Product/Goods/62268795)
- [스프링 공식문서](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans)