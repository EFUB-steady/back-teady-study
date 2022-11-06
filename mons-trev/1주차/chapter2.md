# Chapter2 스프링 부트에서 테스트 코드를 작성하자

### TDD =/= 단위 테스트

### TDD : 테스트가 주도하는 개발
1. 항상 실패하는 테스트코드를 작성
2. 테스트가 통과하는 프로덕션 코드를 작성
3. 테스트가 통과하면 프로덕션 코드를 리팩토링

### 단위 테스트 : TDD 의 첫 번째 단계인 기능 단위의 테스트 코드를 작성

### 테스트 코드를 쓰지 않는다면
1. 코드 작성
2. 프로그램 실행
3. POSTMAN 같은 API 도구로 HTTP 요청 후
4. 요청 결과를 SYSTEM.OUT.PRINTLN() 으로 눈으로 검증
5. 결과가 다르면 프로그램 중지 후 코드를 수정

매우 번거롭다.

### 하지만,
테스트 코드를 작성한다면 톰캣(내장 웹 서버)를 내렸다가 다시 실행하지 않아도 자동검증이 되며 기존의 기능을 안전하게 보장해준다.

### 자바용 테스트 작성 코드를 도와주는 프레임워크 : JUNIT


### 롬복 설치 후 테스트 코드 작성

1. lombok build.gradle 의존성 추가

```
compile('org.projectlombok:lombok')
```

2. 롬복 dto(data transfer object) 작성

```
@Getter//선언된 모든 필드의 get 메소드 생성
@RequiredArgsConstructor//선언된 모든 final 필드가 포함된 생성자를 생성해준다. final 명심
public class HelloResponseDto {
  private final String name;
  private final int amount;
}
```

3. dto 테스트 코드 작성

```
public class HelloResponseDto {
  
  @Test
  public void 롬복_기능_테스트() {
    //given
    String name= "test";
    int amount = 1000;
    
    //when
    HelloResponseDto dto= new HelloResponseDto(name,amount);
    
    //then
    assertThat(dto.getName()).isEqualTo(name);
    assertThat(dto.getAmount()).isEqualTo(amount);
  }
}
```

4, 컨트롤러 코드 작성

```
  @GetMapping("/hello/dto")
  public HelloResponseDto helloDto(@RequestParam("name") String name, @RequestParam("amount") int amount) { //외부에서 api로 넘긴 파라미터 가져옴
    return new HelloResponseDto(name,amount);
  }
```

5. 컨트롤러 테스트코드 작성

```
@RunWith(SpringRunner.class)
@WebMvcTest
public class HelloControllerTest {
  @Autowired
  private MockMvc mvc;
  
  @Test
  public void helloDto가_리턴된다() throws Exception {
    String name = "hello";
    int amount = 1000;
    
    mvc.perform(get("/hello/dto").param("name", name).param("amount" , String.valueOf(amount)))//API테스트할 때 사용될 요청 파라미터를 설정, 값은 String 만 허용.
        .andExpect(status().isOk())
        .andExpect(jsonPath("$.name",is(name)))//json 응답값을 필드별로 검증할 수 있는 메소드 $를 기준으로 필드명을 명시한다
        .andExpect(jsonPath("$.amount",is(amount)));  
  }
}
```

