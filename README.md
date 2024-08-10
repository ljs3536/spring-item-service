# /24-08-05
## 프로젝트 생성

## 요구사항 분석
상품을 관리할 수 있는 서비스를 만들어보자

#### 상품 도메인 모델
- 상품 ID
- 상품명
- 가격
- 수량

#### 상품 관리 기능
- 상품 목록
- 상품 상세
- 상품 등록
- 상품 수정

#### 서비스 제공 흐름
요구사항이 정리되고 디자이너, 웹 퍼블리셔, 백엔드 개발자가 업무를 나누어 진행한다.
- 디자이너 : 요구사항에 맞도록 디자인하고, 디자인 결과물을 웹 퍼블리셔한테 전달한다.
- 웹 퍼블리셔 : 디자이너에서 받은 디자인을 기반으로 HTML, CSS를 만들어 개발자에게 제공한다.
- 백엔드 개발자 : 디자이너, 웹 퍼블리셔를 통해서 HTML 화면이 나오기 전까지 시스템을 설계하고, 핵심 비즈니스 모델을 개발한다.
               이후 HTML이 나오면 이 HTML을 뷰 템플릿으로 변환해서 동적으로 화면을 그리고, 또 웹 화면의 흐름을 제어한다.

### 참고
React, Vue.js같은 웹 클라이언트 기술을 사용하고, 웹 프론트엔드 개발자가 별도로 있으면, 웹 프론트엔드 개발자가 웹 퍼블리셔 역할까지 포함해서 하는 경우도 있다.
웹 클라이언트 기술을 사용하면, 웹 프론트엔드 개발자가 HTML을 동적으로 만드는 역할과 웹 화면의 흐름을 담당한다.
이 경우 백엔드 개발자는 HTML 뷰 템플릿을 직접 만지는 대신에, HTTP API를 통해 웹 클라이언트가 필요로 하는 데이터와 기능을 제공하면 된다.

# /24-08-06
## 상품 도메인 개발

# /24-08-07
## 상품 서비스 HTML
핵심 비즈니스 로직을 개발하는 동안, 웹 퍼블리셔는 HTML 마크업을 완료했다.
다음 파일들을 경로에 넣고 잘 동작하는지 확인해보자.

#### 부트스트랩
참고로 HTML을 편리하게 개발하기 위해 부트스트랩을 사용했다

### 참고
부투스트랩은 웹사이트를 쉽게 만들 수 있게 도와주는 HTML, CSS, JS 프레임워크이다.
하나의 CSS로 휴대포나, 태블릿, 데스크탑까지 다양한 기기에서 작동한다.
다양한 기능을 제공하여 사용자가 쉽게 웹사이트를 제작, 유지, 보수할 수 있도록 도와준다.

# /24-08-08
## 상품 목록 - 타임리프 

### BasicItemController
컨트롤러 로직은 itemRepository에서 모든 상품을 조회한 다음에 모델에 담는다.
그리고 뷰 템플릿을 호출한다.
@RequiredArgsConstructor 애노테이션을 사용하면 
final이 붙은 멤버 변수만 사용해서 생성자를 자동으로 만들어준다.

이렇게 생성자가 딱 1개만 있으면 스플이이 해당 생성자에 @Autowired로 의존관계를 주입해준다.
### 주의
final 키워드를 빼면 안된다. 그러면 itemRepository 의존관계 주입이 안된다.
스프링 핵심원리 - 기본편 강의 참고

### 테스트용 데이터 추가
테스트용 데이터가 없으면 회원 목록 기능이 정상 동작하는지 확인하기 어렵다.
@PostContruct : 해당 빈의 의존관계가 모두 주입되고 나면 초기화 용도로 호출된다.
여기서는 간단히 테스트용 데이터를 넣기 위해서 사용했다.

# /24-08-09

### 타임리프 간단히 알아보기

#### 속성변경 - th:href
'th:href="@{/css/bootstrap.min.css}"'
- href="value1"을 th:href="value2"의 값으로 변경한다.
- 타임리프 뷰 템플릿을 거치게 되면 원래 값을 th:xxx 값으로 변경한다. 만약 값이 없다면 새로 생성한다.
- HTML을 그대로 볼 때는 href 속성이 사용되고, 뷰 템플릿ㅇ르 거치면 th:href의 값이 href로 대체되면서 동적으로 변경할 수 있다.
- 대부분의 HTML 속성을 th:xxx로 변경할 수 있다.

#### 타임리프 핵심
- 핵심은 th:xxx가 붙은 부분은 서버사이드에서 렌더링 되고, 기존 것을 대체한다. th:xxx이 없으면 기존 html의 xxx 속성이 그대로 사용된다.
- HTML을 파일로 직접 열었을 때, th:xxx가 있어도 웹 브라우저는 th:속성을 알지 못하므로 무시한다.
- 따라서 HTML을 파일 보기를 유지하면서 템플릿 기능도 할 수 있다.

#### URL링크 표현식 - @{...}
'th:href="@{/css/bootstrap.min.css}"'
- @{...} : 타임리프는 URL 링크를 사용하는 경우 @{...}를 사용한다. 이것을 URL 링크 표현식이라 한다.
- URL링크 표현식을 사용하면 서블릿 컨텍스트를 자동으로 표함한다.

#### 상품 등록 폼으로 이동
#### 속성 변경 : th:onclick
- onclick="location.href='addForm.html'"
- th:onclick="|location.href='@{/basic/items/add}'|"
  여기에는 다음에 설명하는 리터럴 대체 문법이 사용되었다.

#### 반복 출력 - th:each
- '<tr th:each="item : ${items}">'
- 반복은 th:each를 사용한다. 이렇게 하면 모델에 포함된 items 컬렉션 데이터가 item 변수에 하나씩 포함되고, 반복문 안에서 item 변수를 사용할 수 있다.
- 컬렉션의 수 만큼 <tr>...</tr>이 하위 태그를 포함해서 생성된다.

#### 변수 표현식 - ${...}
- '<td th:text="${item.price}">10000</td>'
- 모델에 포함된 값이나, 타임리프 변수로 선언한 값을 조회할 수 있다.
- 프로퍼티 접근법을 사용한다. (item.getPrice())

#### 내용 변경 - th:text
- '<td th:text="${item.price}">10000</td>'
- 내용의 값을 th:text의 값으로 변경한다.
- 여기서는 10000을 ${item.price}의 값으로 변경한다.

#### URL 링크 표현식2 - @{...}
- th:href="@{/basic/items/{itemId}(itemId=${item.id})}"
- 상품ID를 선택하는 링크를 확인해보자.
- URL링크 표현식을 사용하면 경로를 템플릿처럼 편리하게 사용할 수 있다.
- 경로 변수 ({itemId})뿐만 아니라 쿼리 파라미터도 생성한다.

#### URL 링크 간단히
- th:href="@{|/baisc/items/${item.id}|}"
- 상품 이름을 선택하는 링크를 확인해보자
- 리터럴 대체 문법을 활용해서 간단히 사용할 수도 있다.

### 참고 
타임리프는 순수 HTML을 파일을 웹 브라우저에서 열어도 내용을 확인할 수 있고, 서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과를 확인할 수 있다.
JSP를 생각해보면, JSP 파일은 웹 브라우저에서 그냥 열면 JSP 소스 코드와 HTML이 뒤죽박죽 되어서 정상적인 확인이 불가능하다. 
오직 서버를 통해서 JSP를 열어야 한다.
이렇게 순수 HTML을 그대로 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 네츄럴 템플릿 이라 한다.

