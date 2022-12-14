
# HTTP 메시지
HTTP가 인터넷의 배달원이라면 HTTP 메시지는 무언가를 담아 보내는 소포와 같다.
이번 장은 HTTP 메시지의 모든 것에 대해 말해준다.

## 3.1 메시지의 흐름
HTTP 메시지는 HTTP 애플리케이션 간에 주고받은 데이터의 블록들이다.
이 데이터 블록들은 메시지의 내용과 의미를 설명하는 텍스트 메타 정보로 시작하고
그 다음 선택적으로 데이터가 올 수 있다.

해당 메시지가 클라이언트, 서버, 프락시 사이를 흐른다.

### 3.1.1 메시지는 원 서버 방향을 인바운드로 하여 송신된다.
HTTP는 인바운드와 아웃바운드라는 용어를 트랜잭션 방향을 표현하기 위해 사용한다.
**인바운드**: 메시지가 원 서버로 향하는 것
**아웃 바운드**: 모든 처리가 끝난 뒤에 메시지가 사용자 에이전트로 돌아오는 것은 아웃바운드로 이동하는 것이다.

### 3.1.2 다운스트림으로 흐르는 메시지
HTTP 메시지는 강물과 같이 흐른다.
요청 메시지냐 응답 메시지냐에 관계없이 모든 메시지는 다운스트림으로 흐른다.
메시지의 발송자는 수신자의 업스트림이다.

## 3.2 메시지의 각 부분
HTTP 메시지는 단순한, 데이터의 구조화된 블록이다.

각 메시지는 클라이언트로부터의 요청이나 서버로부터의 응답 중 하나를 포함한다.
또한 메시지는 시작줄, 헤더블록, 본문 세 부분으로 이루어진다.

시작줄: 이것이 어떤 메시지인지 서술
헤더 블록: 속성
본문: 데이터를 담음(아예 없을 수도 있다)

시작줄과 헤더는 줄 단위로 구분된 아스키 문자열이며 각 줄은 캐리지 리턴(\r)과 개행문자(\n)으로 구성된 두 글자의 줄바꿈 문자열로 끝난다.

### 3.2.1 메시지 문법
모든 HTTP 메시지는 요청 메시지나 응답 메시지로 분류된다.
클라이언트가 서버에 어떤 동작을 요청하면 서버가 요청의 결과를 클라이언트에 돌려주는 구조이다.

요청 메시지의 형식은 다음과 같다.
<메서드> <요청 URL> <버전>
<헤더>
<엔티티 본문>

응답 메시지의 형식은 다음과 같다.
<버전><상태 코드><사유구절>
<헤더>
<엔티티 본문>

- 메서드
클라이언트 측에서 서버가 리소스에 대해 수행해주길 바라는 동작이다.
'GET','HEAD','POST'와 같이 한 단어로 되어 있다.

- 요청 URL
요청 대상이 되는 리소스를 지칭하는 완전한 URL 혹은 URL의 경로 구성요소이다.

- 버전
이 메시지에서 사용 중인 HTTP의 버전이다. 형식은 다음과 같다.
HTTP/<메이저>.<마이너>

- 상태코드
요청 중에 무엇이 일어났는지 설명하는 세자리 숫자이다.
각 코드의 첫 번째 자리수는 상태의 일반적인 분류를 나타낸다.

- 사유구절
숫자로 된 상태코드의 의미를 사람이 이해할 수 있게 설명해주는 짧은 문구로
상태코드 이후부터 줄바꿈 문자열까지가 사유구절이다.

- 헤더들
이름, 콜론, 선택적인 공백, 값, CRLF가 순서대로 나타나는 0개 이상의 헤더들
이 헤더의 목록은 빈 줄로 끝나 헤더 목록의 끝과 엔티티 본문의 시작을 표시한다.

- 엔티티 본문
엔티티 본문은 임의의 데이터 블록을 포함한다.
모든 메시지가 엔티티 본문을 갖는것은 아니므로 때때로 메시지는 그냥 CRLF로 끝나게 된다.
*역사적으로 많은 클라이언트와 서버가 엔티티 본문이 없는 경우 실수로 마지막 CRLF를 빠뜨린다. 이와같이 널리 쓰이지만 규칙을 잘 지키지 않는 구현체와 호환을 위해, 클라이언트와 서버는 마지막 CRLF 없이 끝나는 메시지도 받아들일 수 있어야 한다.* 

### 3.2.2 시작줄
모든 HTTP 메시지는 시작줄로 시작한다.
요청 메시지의 시작줄은 무엇을 해야 하는지 말해주며
응답 메시지의 시작줄은 무슨 일이 일어났는지 말해준다.

**요청줄**
요청 메시지는 서버에게 리소스에 대해 무언가를 해달라고 부탁한다.
요청줄은 
- 서버에서 어떤 동작이 일어나야 하는지 설명해주는 메서드
- 그 동작에 대한 대상을 지칭하는 요청URL
- 클라이언트가 어떤 HTTP 버전으로 말하고 있는지 서버에게 알려주는 HTTP 버전
을 포함하며 이 모든 필드는 공백으로 구분된다
 
 **응답줄**
 응답 메시지는 수행 결과에 대한 상태 정보와 결과 데이터를 클라이언트에게 돌려준다.
응답 메시지의 시작 줄 혹은 응답 줄에는 
- 응답 메시지에서 쓰인 HTTP의 버전, 숫자로 된 상태코드
- 수행 상태에 대해 설명해주는 텍스트로 된 사유 구절
이 들어있다.
모든 필드는 모두 공백으로 구분된다.

**메서드**
요청의 시작줄은 메서드로 시작하며 서버에게 무엇을 해야 하는지 말해준다.
HTTP 명세는 공통 요청 메서드의 집합을 정의한다.
또한 HTTP는 쉽게 확장할 수 있도록 설계되었기 때문에, 다른 서버는 그들만의 메서드를 추가로 구현했을 수도 있으며 이러한 추가 메서드는 HTTP 명세를 확장하는 것이기 때문에 확장 메서드라고 불린다.

**상태 코드**
메서드가 서버에게 무엇을 해야 하는지 말해주는 것처럼 상태코드는 클라이언트에게 무엇이 일어났는지 말해준다.
상태 코드는 각 응답 메시지의 시작줄에 담겨 반환된다. 숫자로 된 코드와, 문자열로 되어 있어서 사람이 이해하기 쉬운 메시지 두 형태 모두로 반환된다.

**사유 구절**
사유 구절은 응답 시작줄의 마지막 구성요소이다. 이것은 상태 코드에 대한 글로 된 설명을 제공한다.

**버전 번호**
버전 번호는 HTTP/x,y 형식으로 요청과 응답 메시지 양쪽 모두에 기술된다.
이것은 HTTP 애플리케이션들이 자신이 따르는 프로토콜 버전을 상대방에게 말해주기 위한 수단이 된다.
버전 번호는 HTTP로 대화하는 애플리케이션들에게 대화 상대의 능력과 메시지의 형식에 대한 단서를 제공해주기 위한 것이다.

### 3.2.3 헤더
HTTP 헤더 필드는 요청과 응답 메시지에 추가 정보를 더한다.
그들은 기본적으로 이름/값 쌍의 목록이다.

***헤더 분류***
HTTP 헤더 명세는 여러 헤더 필드를 정의한다. 애플리케이션은 또한 자유롭게 자신만의 헤더를 만들어 낼 수 있다.

**일반 헤더**
요청과 응답 양쪽에 모두 나타날 수 있음

**요청 헤더**
요청에 대한 부가 정보를 제공

**응답 헤더**
응답에 대한 부가 정보를 제공

**Entity 헤더**
본문 크기와 콘텐츠, 혹은 리소스 그 자체를 서술

**확장 헤더**
명세에 정의되지 않은 새로운 헤더

***헤더를 여러 줄로 나누기***
긴 헤더 줄은 그들을 여러 줄로 쪼개서 더 읽기 좋게 만들 수 있는데 
추가 줄 앞에는 최소 하나의 스페이스 혹은 탭 문자가 와야 한다.

### 3.2.4 엔티티 본문
HTTP 메시지의 세 번째 부분은 선택적인 엔티티 본문이다.
엔티티 본문은 HTTP 메시지의 화물이라고 할 수 있다.

### 3.2.5 버전 0.9 메시지
HTTP 버전 0.9는 HTTP 프로토콜의 초기 버전이다.
그것은 오늘날 HTTP가 갖고 있는 요청과 응답 메시지의 시초이지만, 훨씬 단순한 프로토콜로 되어있다.

## 3.3 메서드
모든 서버가 모든 메서드를 구현하지는 않는다.

### 3.3.1 안전한 메서드(Safe Method)
HTTP는 안전한 메서드라 불리는 메서드의 집합을 정의한다.
대표적인 예로 GET과 HEAD 메서드가 있는데 이는 해당 메서드를 사용함으로서
서버에 일어나는 일이 아무것도 없다.

### 3.3.2 GET
GET은 가장 흔히 쓰이는 메서드이며 주로 서버에게 리소스를 달라고 요청하기 위해 쓰인다.

### 3.3.3 HEAD
HEAD 메서드는 정확히 GET처럼 행동하지만 서버는 응답으로 헤더만 돌려주며 본문은 반환되지 않는다.
- 리소스를 가져오지 않고도 그에 대해 무엇인가를 알아낼 수 있다.
- 응답의 상태 코드를 통해, 개체가 존재하는지 확인할 수 있다.
- 헤더를 확인하여 리소스가 변경되었는지 검사할 수 있다.

### 3.3.4 PUT
GET 메서드가 서버로부터 문서를 읽어 들이는데 반해 PUT 메서드는 서버에 문서를 쓴다.
PUT 메서드의 의미는 서버가 요청의 본문을 가지고 요청 URL의 이름대로 새 문서를 만들거나 이미 URL이 존재한다면 본문을 사용해서 교체하는 것이다.

### 3.3.5 POST
POST 메서드는 서버에 입력 데이터를 전송하기 위해 설계되었다.
실제로 HTML 폼을 지원하기 위해 흔히 사용된다.
채워진 폼에 담긴 데이터는 서버로 전송되며 서버는 이를 모아서 필요로 하는 곳으로 보낸다.

### 3.3.6 TRACE
클라이언트가 어떤 요청을 할 때, 그 요청은 방화벽, 프락시, 게이트웨이 등의 애플리케이션을 통과할 수 있으며 이들은 원래의 HTTP 요청을 수정할 수 있는 기회가 있다.

TRACE 메서드는 클라이언트에게 자긴의 요청이 서버에 도달했을 때 어떻게 보이게 되는지 알려준다.

TRACE 요청은 목적지 서버에서 '루프백' 진단을 시작한다. 요청 전송의 마지막 단계에 있는 서버는 자신이 받은 요청 메시지를 본문에 넣어 TRACE 응답을 되돌려준다.
이를 통해 클라이언트는 자신이 보낸 메시지가 망가졌는지 수정되었는지 알 수 있다.

따라서 TRACE 메서드는 주로 진단을 위해 사용된다.

### 3.3.7 OPTIONS
OPTIONS 메서드는 웹 서버에게 여러 가지 종류의 지원 범위에 대해 물어본다.
서버에게 특정 리소스에 대해 어떤 메서드가 지원되는지 물어볼 수 있다.

위 메서드를 통하여 여러 리소스에 대해 실제로 접근하지 않고도 그것들을 어떻게 접근하는 것이 최선인지 확인할 수 있는 수단을 제공한다.

### 3.3.8 DELETE
DELETE 메서드는 서버에게 요청 URL로 지정한 리소스를 삭제할 것을 요청한다.

### 3.3.9 확장 메서드
HTTP는 필요에 따라 확장하여도 문제가 없도록 설계되어 있으므로 새로 기능을 추가해도 과거에 구현된 소프트웨어의 오동작을 유발하지 않는다.

이 때  모든 확장 메서드가 형식을 갖춘 명세로 정의된 것은 아니라는 점에 주의해야 한다.
클라이언트가 정의한 메서드와 HTTP 애플리케이션이 호환되지 않는 경우가 대부분일 것이다.

이런 상황에서는 확장 메서드에 대해 관용적인 것이 최고다.
확장 메서드를 다룰 때는 "엄격하게 보내고 관대하게 받아들여라"라는 규칙을 따르는 것이 가장 좋다.

## 3.4 상태코드
HTTP의 상태 코드는 크게 다섯 가지로 나뉜다. 이 절에서는 HTTP 상태 코드의 각 분류에 대해 요약한다.

### 3.4.1 100-199: 정보성 상태 코드
정보성 상태 코드는 HTTP/1.1에서 도입되었다.
비교적 새로운 것이며 복잡함을 감수할 만한 가치가 있는지에 대해 논란이 되고 있다.
|                상태코드       |        사유 구절                                                                                                                                                    | 의미          |
| :---------- | :------------------------------------------------------------------------------------------------------------------------------------------------------ | :--------------- |
| 100   | Continue                                                                                                           | 요청의 시작 부분 일부가 받아들여졌으며 클라이언트는 나머지를 계속 이어서 보내야 함을 의미한다.             |
| 101 | Switching Protocools                                                                                              | 클라이언트가 Upgrade 헤더에 나열한 것 중 하나로 서버가 프로토콜을 바꾸었음을 의미한다.        |

특히 100 상태 코드는 약간 혼란스럽다.
해당 코드는 서버에 본문을 전송하기 전 서버가 본문을 받아들일 것인지 확인하려고 할 때,
그 확인 작업을 최적화 하기 위한 의도로 도입된 것이다.
이는 프로그래머를 혼란스럽게 하는 경향이 있으므로 아래에서 좀 더 자세히 다룰 것이다.

***클라이언트와 100 Continue***
만약 클라이언트가 엔티티를 서버에 보내려 하고 그 전에 100 Continue 응답을 기다리겠다면 클라이언트는 값을 100-continue로 하는 Except 요청 헤더를 보낼 필요가 있다.

만약 엔티티를 보내지 않으려 한다면 100-continue Except 헤더를 보내지 않아야 한다.
이는 클라리언트가 엔티티를 보낼 것이라고 생각하게 만들어 서버를 혼란에 빠뜨릴 뿐이다.

100-continue는 여러 측면에서 최적화를 위한 것이다.
클라이언트는 100-continue를 서버가 다루거나 사용할 수 없는 큰 엔티티를 서버에게 보내지 않으려는 목적으로만 사용해야 한다.

100-continue 상태에 대한 초창기의 혼란 떄문에 Except 헤더를 보낸 클라이언트는 서버가 100 continue 응답을 보내주기를 막연히 기다리기만 해서는 안 된다.

또한 클라이언트는 몇몇 부적절한 애플리케이션 코드로 인해 
예상하지 못한 100 continue 응답에도 대비해야 한다.

***서버와 100 continue***
100-continue 응답을 받을 것을 의도하지 않은 클라이언트에게 100 Continue 상태 코드를 보내서는 안된다.
서버가 100 continue 응답을 보낼 기회를 갖기 전에 어떤 이유로 인해 앤티티의 일부를 수신하였다면 서버는 이 상태 코드를 보낼 필요가 없다.
클라이언트는 이미 계속 전송하기로 결정하였기 때문이다
그러나 거버가 요청을 끝까지 다 읽은 후에는 그 요청에 대한 최종 응답을 보내야 한다.

마지막으로 만약 서버다 100 continue 응답을 받을 것을 의도한 요청을 받고 난 상태에서 엔티티 본문을 읽기 전에 요청을 끝내기로 결정했다면 
서버는 응답을 보내고 연결을 닫아서는 안된다. 클라이언트가 응답을 받을 수 없게 되기 때문이다.

***프락시와 100 Continue***
클라이언트로부터 100-continue 응답을 의도한 요청을 받은 프락시는 몇 가지 해야 할 일이 있다. 만약 다음 홉 서버가 HTTP/1.1을 따르거나 혹은 어떤 버전을 따르는지 모른다면 Except 헤더를 포함시켜서 다음으로 전달해야한다

만약 다음 홉의 서버가 1.1보다 이전 버전의 HTTP를 따른다면 프락시는 417 에러로 응답해야 한다.

만약 프락시가 HTTP/1.0이나 이전 버전을 따르는 클라이언트를 대신하여 Expect 헤더와 100-continue 값을 요청에 포함시키기로 결정했다면 프락시는 100 Continue 응답을 클라이언트에 전달 해서는 안된다. 클라이언트는 그것을 어떻게 해야 할지 모를 것이기 때문이다.

프락시가 다음 홉 서버들에 대한 상태 몇가지와 그들이 지원하는 HTTP 버전을 기억해둔다면
100-continue 응답을 기대한 요청을 더 잘 다룰 수 있게 되므로 프락시에게 이득이 된다.

### 3.4.2 200-299 성공 상태 코드
클라이언트가 요청을 보내면 그 요청은 대개 성공한다.
서버는 대응하는 성공을 의미하는 상태코드의 배열을 갖고 있으며 각각의 요청에 대응한다.
상태 코드|사유 구절|의미
:---|:---|:---
200|OK|요청은 정상이고, 엔터티 본문은 요청된 리소스를 포함하고 있다.
201|Created|서버 개체를 생성하라는 요청(예: PUT)을 위한 것. 응답은, 생성된 리소스에 대한 최대한 구체적인 참조가 담긴 Location 헤더와 함께, 그 리소스를 참조할 수 있는 여러 URL을 엔터티 본문에 포함해야 한다.
202|Accepted|요청은 받아들여졌으나 서버는 아직 그에 대한 어떤 동작도 수행하지 않았다. 서버는 엔터티 본문에 요청에 대한 상태와 가급적이면 요청의 처리가 언제 완료 될 것인지에 대한 추정도 포함해야 한다.
203|Non-Authoritative Information|엔터티 헤더에 들어있는 정보가 원래 서버가 아닌 리소스의 사본에서 왔다. 이 응답 코드는 필수적으로 사용되어야 하는 것은 아니다. 엔티티 헤더가 원래 서버에서 온 것이었다면 응답이 200 상태였을 애플리케이션을 위한 선택사항이다.
204|No Content|응답 메시지는 헤더와 상태줄을 포함하지만 엔터티 본문은 포함하지 않는다. 주로 웹 브라우저를 새 문서로 이동시키지 않고 갱신하고자 할 때(폼을 리프레시) 사용한다.
205|Reset Content|브라우저에게 현재 페이지에 있는 HTML 폼에 채워진 모든 값을 비우라고 말한다.
206|Partial Content|부분 혹은 범위 요청 성공. 206 응답은 Content-Rage와 Date 헤더를 반드시 포함해야 하며, Etag와 Content-Location중 하나의 헤더도 반드시 포함해야한다.

### 3.4.3 300-399: 리다이렉션 상태 코드
리다이렉션 상태 코드는 클라이언트가 관심있어 하는 리소스에 대해 다른 위치를 사용하라고 말해주거나 그 리소스의 내용 대신 다른 대안 응답을 제공한다.

리다이렉션 상태 코드 중 몇몇은 리소스에 대한 애플리케이션의 로컬 복사본이 원래 서버와 비교했을 때 유효한지 확인하기 위해 사용된다.

일반적으로 HEAD가 아닌 요청에 대해 리다이렉션 상태 코드를 포함한 응답을 할 때
리다이렉트 될 URL에 대한 링크와 설명을 포함시키는 것은 좋은 습관이다.

상태 코드|사유 구절|의미
:---|:---|:---
300|Multiple Choices|클라이언트가 동시에 여러 리소스를 가리키는 URL을 요청한 경우, 그 리소스의 목록과 함께 반환한다.
301|Moved Permanently|요청한 URL이 옮겨졌을 때 사용한다. 응답은 Location 헤더에 현재 리소스가 존재하고 있는 URL을 포함해야 한다.
302|Found|301 과 같다. 그러나 클라이언트는 Location 헤더로 주어진 URL을 리소스를 임시로 가리키기 위한 목적으로 사용해야 한다.
303|See Other|클라이언트에게 리소스를 다른 URL에서 가져와야 한다고 말해주고자 할 때 쓰인다. 주 목적은 POST 요청에 대한 응답으로 클라이언트에게 리소스의 위치를 알려주는 것이다.
304|Not Modified|요청한 리소스가 최근에 수정된 일이 없다면, 이 코드는 리소스가 수정되지 않았음을 의미한게 된다. 이 상태 코드를 동반한 응답은 엔터티 본문을 가져가서는 안된다.
305|Use Proxy|리소스가 반드시 프락시를 통해서 접근되어야 함을 나타내기 위해 사용한다. 프락시의 위치는 Location 헤더를 통해 주어진다. 해당 요청에 대해서만 이 프락시를 사용해야 한다고 해석한다.
306|(사용되지 않음)|현재 사용되지 않음
307|Temporary Redirect|301 상태 코드와 비슷하다. 그러나 클라이언트는 Location 헤더로 주어진 URL을 리소스를 임시로 가리키기 위한 목적으로 사용해야 한다.

위 표에서 302, 303, 307 상태 코드 사이에서 중복되는 부분이 있음을 알 수 있을것이다.
위 코드들이 어떻게 사용되는가에 대해서는 미묘한 차이가 있다.
이는 주로 HTTP/1.0과 HTTP/1.1이 상태 코드를 다루는 방식의 차이점에 기인한다.

-   302 -> HTTP/1.0 클라이언트가 POST 요청을 보내고 302 리다이렉트 상태 코드가 담긴 응답을 받으면, Location 헤더에 들어있는 리다이렉트 URL을 GET 요청으로 따라간다. (POST->GET)
-   303 -> HTTP/1.1 에서는 302대 신 303을 사용
-   307 -> HTTP/1.1 에서는 일시적인 리다이렉트를 위해 307 상태 코드를 사용하라고 한다

결국 서버는 리다이렉트 응답에 들어갈 가장 적절한 리다이렉트 상태 코드를 선택하기 위해 클라이언트의 HTTP 버전을 검사할 필요가 있다.

### 3.4.4 400-499: 클라이언트 에러 상태 코드
가끔 클라이언트는 서버가 다룰 수 없는 무엇인가를 보낸다.
웹 브라우징을 하면서 404 에러를 만난 일이 있을 것이다. 이것은 서버가 우리에게 우리가 알 수 없는 리소스에 대해 요청을 했다고 말해주는 것이다.

아래 표는 다양한 클라이언트 에러 상태코드이다.

상태 코드|사유 구절|의미
:---|:---|:---
400|Bad Request|클라이언트가 잘못된 요청을 보냈다.
401|Unauthorized|리소스를 얻기 전 클라이언트에게 인증하라고 요구
402|Payment Required|현재는 쓰이지 않지만, 미래를 위해 준비해둠
403|Forbidden|요청이 서버에 의해 거부되었다. 보통 서버가 거절의 의미를 숨기려고 사용
404|Not Found|서버가 요청한 URL을 찾을 수 없음을 알려주기 위해 사용
405|Method Not Allowed|요청 URL에 대해, 지원하지 않는 메서드로 요청받았을 때 사용
406|Not Acceptable|주어진 URL에 대한 리소스 중 클라이언트가 받아들일 수 있는 것이 없는 경우 사용
407|Proxy Authentication Requried|401과 같으나, 리소스에 대해 인증을 요구하는 프락시 서버를 위해 사용
408|Request Timeout|요청 완수하기에 시간이 너무 많이 걸리는 경우, 연결을 끊을 수 있다.
409|Conflict|서버는 요청이 충돌을 일으킬 염려가 있다고 생각될 때 이 요청을 보낸다.
410|Gone|404와 비슷하나 서버가 한때 그 리소스를 갖고 있었다는 차이점이 있다.
411|Length Required|서버가 요청 메시지에 Content-Length 헤더가 있을 것을 요구할 때 사용
412|Precondition Failed|조건부 요청을 받았는데 그중 하나가 실패했을 때 사용한다.
413|Request Entity Too Large|서버가 처리할 수 있는 한계를 넘은 크기의 요청을 클리아언트가 보냈을 때 사용
414|Request URI Too Long|서버가 처리할 수 있는 한계를 넘은 URL이 포함된 요청을 했을 때 사용
415|Unsupported Media Type|서버가 이해하거나 지원하지 못하는 내용 유형의 엔터티를 보냈을 때 사용
416|Requested Range Not Satisfiable|리소스의 특정 범위를 요청했는데, 그 범위가 잘못되었을 때 사용
417|Expectation Failed|요청의 Expect 요청 헤더에 서버가 만족시킬 수 없는 기대가 담겨있는 경우 사용

### 3.4.5 500-599: 서버 에러 상태 코드
때때로 클라이언트가 올바른 요청을 보냈음에도 서버 자체에서 에러가 발생하는 경우가 있다.
이것은 클라이언트가 서버의 제한에 걸린 것일 수도 있고 혹은 게이트웨이 리소스와 같은 서버의 보조 구성요소에서 발생한 에러일 수도 있다.

프락시는 클라이언트의 입장에서 서버와 대화를 시도할 때 자주 에러를 만나게 되는데
이때 발생하는 문제를 설명하기 위해 5XX 서버 에러 상태코드를 생성한다.

상태 코드|사유 구절|의미
:---|:---|:---
500|Internal Server Error|서버가 요청을 처리할 수 없게 만드는 에러를 만남
501|Not Implemented|클라이언트가 서버의 능력을 넘은 요청을 했을 때 사용(예: 서버가 지원하지 않는 메서드 사용)
502|Bad Gateway|프락시나 게이트웨이처럼 행동하는 서버가 그 요청 응답 연쇄에 있는 다음 링크로부터 가짜 응답에 맞닥뜨렸을 때 사용(예: 자신의 부모 게이트웨이에 접속하는 것이 불가능할 때)
503|Service Unavailable|현재는 서버가 요청을 처리해 줄 수 없지만 나중에는 가능함을 의미하고자 할ㄷ 때 사용(Retry-After 헤더 사용)
504|Gateway Timeout|408과 비슷하지만, 다른 서버에게 요청을 보내고 응답을 기다리다 타임아웃이 발생한 게이트웨이나 프락시에서 온 응답이라는 차이점이 존재
505|HTTP Version Not Supported|서버가 지원할 수 없거나 지원하지 않으려 하는 버전의 프로토콜 요청을 받았을 경우 사용

## 3.5 헤더
헤더와 메서드는 클라이언트와 서버가 무엇을 하는지 결정하기 위해 함께 사용된다.
헤더는
- 특정 종류의 메시지에만 사용할 수 있는 헤더
- 더 일반 목적으로 사용할 수 있는 헤더
- 응답과 요청 메시지 양쪽 모두에서 정보를 제공하는 헤더
가 있다.

**일반 헤더**
일반 헤더는 클라이언트와 서버 양쪽 모두가 사용한다.
이들은 클라이언트, 서버, 어딘가에 메시지를 보내는 다른 애플리케이션들을 위해 다양한 목적으로 사용된다.
 
 **요청 헤더**
 이름에서 드러나는 것과 같이 요청 헤더는 요청 메시지를 위한 헤더이다.
 그들은 서버에게 클라이언트가 받고자 하는 데이터의 타입이 무엇 인지와 같은 부가 정보를 제공한다.

** 응답 헤더**
응답 메시지는 클라이언트에게 정보를 제공하기 위한 자신만의 헤더를 갖고 있다.

**엔티티 헤더**
엔티티 헤더란 엔티티 본문에 대한 헤더를 말한다.

**확장 헤더**
확장 헤더는 애플리케이션 개발자들에 의해 만들어졌지만 아직 승인된 HTTP 명세에는 추가되지 않은 비 표준 헤더이다.

### 3.5.1 일반 헤더
어떤 헤더는 메시지에 대한 아주 기본적인 정보를 제공하며 이 헤더들을 일반 헤더라 부른다.
헤더|설명
:---|:---
Connection| 클라이언트와 서버가 요청/응답 연결에 대한 옵션을 정할 수 있게 해준다.
Date | 메시지가 언제 만들어졌는지에 대한 날짜와 시간을 제공
MIME_Version|발송자가 사용한 MIME의 버전을 알려준다.
Trailer chunked transfer|인코딩으로 인코딩된 메시지의 끝 부분에 위치한 헤더들의 목록을 나열
Transfer-Encoding|수신자에게 안전한 전송을 위해 메시지에 어떤 인코딩인 적용되었는지 알려준다.
Upgrade|발송자가 '업그레이드'하길 원하는 새 버전이나 프로토콜을 알려준다.
Via|이 메시지가 어떤 중개자(프락시, 게이트웨이)를 거쳐 왔는지 보여준다.

***일반 캐시 헤더***
HTTP/1.0은 HTTP 애플리케이션에게 매번 원 서버로부터 객체를 가져오는 대신 로컬 복시본으로 캐시 할 수 있는 최초의 헤더를 도입했다.

헤더|설명
:---|:---
Cache-Control|메시지와 함께 캐시 지시자를 전달하기 위해 사용한다.
Pragma|메시지와 함께 지시자를 전달하는 또 다른 방법. 캐시에 국한되지 않는다. (deprecated 예정?)

### 3.5.2 요청 헤더
요청 헤더는 요청 메시지에서만 의미를 가지는 헤더이다.
그들은 요청이 최초 발생한 곳에서 누가 혹은 무엇이 그 요청을 보냈는지에 대한 정보나 
클라이언트의 선호나 능력에 대한 정보를 준다.

헤더|설명
:---|:---
Client-IP|클라이언트가 실행된 컴퓨터의 IP를 제공한다
From|클라이언트 사용자의 메일 주소를 제공한다
Host|요청으 ㅣ대상이 되는 서버의 호스트 명과 포트를 준다
Referer|현재의 요청 URI가 들어있었던 문서의 URL을 제공한다
UA-Color|클라이언트 기기 디스플레이의 색상 능력에 대한 정보를 제공한다
UA-CPU|클라이언트 CPU의 종류나 제조사를 알려준다.
UA-Disp|클라이언트의 디스플레이(화면) 능력에 대한 정보를 제공
UA-OS|클라이언트 기기에서 동작 중인 운영체제의 이름과 버전을 알려준다
UA-Pixels|클라이언트 기기 디스플레이에 대한 픽셀 정보를 제공
User-Agent|요청을 보낸 애플리케이션의 이름을 서버에 알려준다

***Accept 관련 헤더***
클라이언트의 Accept  관련 헤더들은 자신의 선호와 능력을 알려줄 수 있다.
클라이언트가 무엇을 원하고 무엇을 할 수 있는지, 무엇을 원치 않는지에 대해 알려준다.
서버는 이 정보를 활용해 더 나은 결정을 내릴 수 있다.

헤더|설명
:---|:---
Accept|서버에게 서버가 보내도 되는 미디어 종류를 말해준다
Accept-Charset|서버에게 서버가 보내도 되는 문자집합을 말해준다
Accept-Encoding|서버에게 서버가 보내도 되는 인코딩을 말해준다
Accept-Language|서버에게 서버가 보내도 되는 언어를 말해준다
TE|서버에게 서버가 보내도 되는 확장 전송 코딩을 말해준다

***조건부 요청 헤더***
때때로 클라이언트는 요청에 몇몇 제약을 넣기도 한다.

예를 들어 클라이언트가 이미 어떤 문서의 사본을 갖고 있는 상태라면
클라이언트는 서버에게 그 문서를 요청할 때 자신이 갖고 있는 사본과 다를 때만 전송해 달라고 요청하고 싶을 수 있을 것이다.

조건부 요청 헤더를 사용하면 클라이언트는 서버에게 요청에 응답하기 전에 먼저 조건이 참인지 확인하게 하는 제약을 포함시킬 수 있다.

헤더|설명
:---|:---
Except|클라이언트가 요청에 필요한 서버의 행동을 열거할 수 있게 해준다.
If-Match|문서의 엔터티 태그가 주어진 엔터티 태그와 일치하는 경우에만 문서를 가져온다
If-Modified-Since|주어진 날짜 이후에 리소스가 변경되지 않았다면 요청을 제한한다.
If-None-Match|문서의 엔터티 태그가 주어진 엔터티 태그와 일치하지 않는 경우에만 문서를 가져온다
If-Range|문서의 특정 범위에 대한 요청을 할 수 있다.
If-Unmodified-Since|주어진 날짜 이후에 리소스가 변경되었다면 요청을 제한한다.
Range|서버가 범위 요청을 지원한다면, 리소스에 대한 특정 범위를 요청한다

***요청 보안 헤더***
HTTP는 자체적으로 요청을 위한 간단한 인증 요구/응답 체계를 갖고 있다.
이것은 요청하는 클라이언트가 어느 정도의 리소스에 접근하기 전에 자신을 인증하게 함으로써 트랜잭션을 약간 더 안전하게 만들고자 한다.

헤더|요청
:---|:---
Authorization|클라이언트가 서버에게 제공하는 인증 그 자체에 대한 정보를 담고 있다
Cookie|클라이언트가 서버에게 토큰을 전달할 때 사용한다. 진짜 보안 헤더는 아니지만, 보안에 영향을 줄 수 있다는 것은 확실하다.
Cookie2|요청자가 지원하는 쿠키의 버전을 알려줄 때 사용한다.

***프락시 요청 헤더***
인터넷에서 프락시가 점점 흔해지면서 그들의 기능을 돕기 위해 몇몇 헤더들이 정의되어 왔다.

헤더|설명
:---|:---
Max-Forwards|요청이 원 서버로 향하는 과정에서 다른 프락시나 게이트웨이로 전달될 수 있는 최대 횟수. TRACE 메서드와 함께 사용된다
Proxy-Authorization|Authorization과 같으나 프락시에서 인증을 할 때 쓰인다
Proxy-Connection|Connection과 같으나 프락시에서 연결을 맺을 때 쓰인다

### 3.5.3 응답 헤더
응답 메시지는 그들만의 응답 헤더를 가지며 응답 헤더는 클라이언트에게 부가 정보를 제공한다.
누가 응답을 보내고 있는지, 응답자의 능력은 어떻게 되는지, 응답에 대한 특별한 설명도 제공할 수 있다.

헤더|설명
:---|:---
Age|응답이 얼마나 오래되었는지
Public|서버가 특정 리소스에 대해 지원하는 요청 메서드의 목록
Retry-After|현재 리소스가 사용 불가능한 상태일 때, 언제 가능해지는지 날짜 혹은 시각
Server|서버 애플리케이션의 이름과 버전
Title|HTML 문서에서 주어진 것과 같은 제목
Warning|사유 구절에 있는 것보다 더 자세한 경고 메시지

***협상 헤더*** 
서버에 프랑스어와 독일어로 번역된 HTML 문서가 있는 경우와 같이 여러 가지 표현이 가능한 상황이라면 HTTP/1.1은 서버와 클라이언트가 어떤 표현을 택할 것인지에 대한 협상을 할 수 있도록 지원한다.

헤더|설명
:---|:---
Accept-Ragnes|서버가 자원에 대해 받아들일 수 있는 범위의 형태
Vary|서버가 확인해 보아야 하고 그렇기 때문에 응답에 영향을 줄 수 있는 헤더들의 목록

***응답 보안 헤더***
기본적으로 HTTP 인증요구/응답 체계에서 응답 측에 해당하는 요청 보안 헤더는 이미 본 적이 있을 것이다. 아래 표는 응답 보안 헤더를 나열하고 있다.

헤더|설명
:---|:---
Proxy-Authenticate|프락시에서 클라이언트로 보낸 인증요구의 목록
Set-Cookie|진짜 보안 헤더는 아니지만, 보안에 영향은 줄 수 있다. 서버가 클라이언트를 인증할 수 있도록 클라이언트 측에 토큰을 설정하기 위해 사용한다.
Set-Cookie2|Set-Cookie와 비슷하게 RFC 2965로 정의된 쿠키
WWW-Authenticate|서버에서 클라이언트로 보낸 인증요구 목록

### 3.5.4 엔티티 헤더
HTTP 메시지의 엔티티에 대해 설명하는 헤더들은 많다.
요청과 응답 양쪽 모두 엔티티를 포함할 수 있기 때문에 이 헤더들은 양 타입의 메시지에 모두 나타날 수 있다.

엔티티 헤더는 엔티티와 그것의 내용물에 대한 개체들의 타입부터 시작해서 주어진 리소스에 대해 요청할 수 있는 유효한 메서드들까지 광범위한 정보를 제공한다.

일반적으로는 엔티티 헤더는 메시지의 수신자에게 자신이 다루고 있는 것이 무엇인지 말해준다.
헤더|설명
:---|:---
Allow|이 엔터티에 대해 수행될 수 있는 요청 메서드들을 나열한다
Location|클라이언트에게 엔터티가 실제로 어디에 위치하고 있는지 말해준다. 수신자에게 리소스에 대한(아마도 새로운) 위치 URL을 알려줄 때 사용한다

***콘텐츠 헤더***
콘텐츠 헤더는 엔티티의 콘텐츠에 대한 구체적인 정보를 제공한다.
콘텐츠의 종류, 크기, 기타 콘텐츠를 처리할 때 유용하게 활용될 수 있는 것들이다.
헤더|설명
:---|:---
Content-Base|본문에서 사용된 상대 URL을 계산하기 위한 기저 URL
Content-Encoding|본문에 적영된 어떤 인코딩
Content-Language|본문을 이해하는데 가장 적절한 자연어
Content-Length|본문의 길이나 크기
Content-Location|리소스가 실제로 어디에 위치하는지
Content-MD5|본문의 MD5 체크섬
Content-Range|전체 리소스에서 이 엔터티가 해당하는 범위를 바이트 단위로 표현
Content-Type|이 본문이 어떤 종류의 객체인지

*** 엔티티 캐싱 헤더***
일반 캐싱 헤더는 언제 어떻게 캐시가 되어야 하는지에 대한 지시자를 제공한다.
엔티티 캐싱 헤더는 엔티티 캐싱에 대한 정보를 제공한다.
헤더|설명
:---|:---
ETag|엔터티에 대한 엔터티 태그
Expires|이 엔터티가 더 이상 유효하지 않아 원본을 다시 받아와야 하는 일시
Last-Modified|가장 최근 이 엔터티가 변경된 일시
