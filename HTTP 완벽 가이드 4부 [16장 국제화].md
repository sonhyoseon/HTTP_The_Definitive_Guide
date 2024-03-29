# 국제화
매일 수십억의 사람들이 수백 가지 언어로 문서를 작성하기 때문에 
HTTP는 여러 언어와 문자로 된 국제 문서들의 처리 및 전송을 지원해야 한다.

이 장에서는 두가지 주요 국제화 이슈인 문자집합 인코딩과 언어태그를 다룬다.

## 16.1 국제적인 콘텐츠를 다루기 위해 필요한 HTTP 지원
HTTP 메시지는 어떤 언어로 된 콘텐츠든, 이미지, 동영상 혹은 그 외 다른 종류의 미디어처럼 실어 나를 수 있다. HTTP에서 엔티티 본문이란 그저 비트들로 가득 찬 상자에 불가하다.

국제 콘텐츠를 지원하기 위해 서버는 클라이언트에게 각 문서의 문자와 언어를 알려주어
클라이언트가 올바르게 문서를 이루고 있는 비트들을 문자들로 풀어내고 올바르게 처리해서 사용자에게 콘텐츠를 제공해줄 수 있도록 할 필요가 있다.

서버는 클라이언트에게 문자와 언어를 Content-Type charset 매개변수와 Content-Language 헤더를 통해 알려준다.
동시에 클라이언트는 서버에게 사용자가 어떤 언어를 이해할 수 있고 어떤 알파벳의 코딩 알고리즘이 브라우저에 설치되어 있는지 말해줄 필요가 있다. 이 때 Accept-Charset과 Accept-Language 헤더를 보낸다.

## 16.2 문자집합과 HTTP
국제 알파벳 스크립트와 그들의 문자집합 인코딩에 대해 알아보자

### 16.2.1 차셋(Charset)은 글자를 비트로 변환하는 인코딩이다.
HTTP 차셋 값은 어떻게 엔티티 콘텐츠 비트들을 특정 문자 체계의 글자들로 바꾸는지 말해준다.
각 차셋 태그는 비트들을 글자들로 변환하거나 혹은 그 반대의 일을 해주는 알고리즘을 명명한다.

### 16.2.2 문자집합과 인코딩은 어떻게 동작하는가
우리는 문서를 이루는 비트들을 화면에 보여줄 수 있는 글자들로 변환하기를 원한다.
세상에는 여러 문자가 있고 글자를 비트로 인코딩하는 여러가지 방법이 있기 때문에 디코딩 알고리즘을 지칭하는 표준화된 방법이 필요하다.

국제화된 문자 시스템의 핵심 목표는 표현에서 의미를 분리하는 것이다.
HTTP는 문자 데이터 및 그와 관련된 언어와 차셋 라벨의 전송에만 관심을 갖는다.
글꼴 등은 사용자의 디스플레이 소프트웨어가 결정한다.

### 16.2.3 잘못된 차셋은 잘못된 글자들을 낳는다.
만약 클라이언트가 잘못된 charset 매개변수를 사용한다면, 클라이언트는 깨진 글자를 보여줄 것이다.

### 16.2.4 표준화된 MIME 차셋 값
특정 문자 인코딩과 특정 코딩된 문자집합의 결합을 MIME 차셋이라고 부른다.
HTTP는 표준화된 MIME 차셋 태그를 Content-Type과 Accept-Charset 헤더에 사용한다.

### 16.2.5 Content-Type charset 헤더와 META 태그
웹 서버는 클라이언트에게 MIME 차셋 태그를 charset 매개변수와 함께 Content-Type 헤더에 담아 보낸다.
만약 문자집합이 명시적으로 나열되지 않았다면 수신자는 문서의 콘텐츠로부터 문자집합을 추측하려 시도한다.
HTML 콘텐츠에서 문자 집합은 문자 집합을 서술하는 META Content-Type 태그를 사용하고
HTML이 아니라면 일반적인 패턴을 찾기 위해 실제 텍스트를 스캐닝하여 문자 인코딩을 추측한다.
만약 문자 인코딩을 추측하지 못했다면 iso-8859-1인 것으로 가정한다.

### 16.2.6 Accept-Charset 헤더
HTTP 클라이언트는 서버에게 정확히 어떤 문자 체계를 그들이 지원하는지 Accept-Charset 요청 헤더를 통해 알려준다. Accept-Charset 헤더의 값은 클라이언트가 지원하는 문자 인코딩의 목록을 제공한다.

Accept-Charset 요청 헤더에 대응하는 Content-Charset 응답 헤더는 존재하지 않는다는 것에 주의하라

### 16.4 언어 태그와 HTTP
언어 태그는 언어에 이름을 붙이기 위한 짧고 표준화된 문자열이다.
우리는 언어에 대한 표준화된 이름이 필요한다.
영어,독일어,한국어 등등 많은 표준화된 언어태그가 존재한다.

### 16.4.1 Content-Length 헤더
Content-Language 엔티티 헤더 필드는 엔티티가 어떤 언어 사용자를 대상으로 하고 있는지 서술한다.
이 헤더는 텍스트 문서 뿐만 아니라 영상 오디오 등을 대상으로도 할 수 있다.

### 16.4.2 Accept-Language 헤더
Accept-Language 헤더는 웹 서버에 우리의 언어 제약과 선호도를 전달한다.

### 16.4.4 서브태그
언어 태그는 하이픈으로 분리된 하나 이상의 서브태그로 이루어져있다.
첫 번째 서브태그 : 주 서브태그
두 번째 서브태그 : 선택적이고 자신만의 표준 이름을 따른다. 

### 16.4.5 대소문자의 구분 및 표현
모든 태그는 대소문자가 구분되지 않는다.
관용적으로는 언어를 나타낼 때 소문자 국가를 나타낼 때 대문자를 사용한다.

### 16.4.7 첫 번째 서브태그: 이름공간
첫 번째 서브태그는 보통 ISO 639 표준 언어 집합에서 선택된 표준화된 언어 토큰이다.
만약 첫 번째 서브태그가 
두 글자라면 ISO 639와 639-1 표준의 언어 코드다.
세 글자라면 ISO 639-2 표준과 확장에 열거된 언어 코드다.
글자 'i'라면 이 언어태그는 틀림없이 IANA에 등록된 것이다.
글자 'x'라면 이 언어 태그는 특정 개인이나 집단 전용의 비표준 확장 서브태그이다.

### 16.4.8 두 번째 서브태그: 이름공간
두 번째 서브태그는 보통 ISO 3166 국가 코드와 지역 표준 집합에서 선택된 표준화 된 국가 토큰이다.
만약 두 번째 서브태그가 
두 글자라면 ISO 3166에 정의된 국가/지역이다
3~8 글자라면 IANA에 등록 된것이다.

## 16.5 국제화된 URI
오늘날 URI는 대부분 US-ASCII의 부분집합으로 구성되어 있다.

### 16.5.1 국제적 가독성 vs 의미 있는 문자들
URI 설계자들은 리소스 식별자의 가독성과 공유 가능성의 보장이 대부분의 의미 있는 문자들로 구성될 수 있도록 하는 것보다 더 중요하게 여겨
오늘날의 ASCII 문자들의 제한된 집합으로 이루어진 URI를 갖게 되었다.

### 16.5.2 URI에서 사용될 수 있는 문자들
URI에서 사용할 수 있는 US-ASCII 문자들은 예약된 문자들, 예약되지 않은 문자들, 이스케이프 문자들로 나뉜다.
예약되지 않은 문자들은 일반적으로 사용된다.
예약된 문자들은 URI상 특별한 의미를 가져 일반적으로 사용될 수 없다.

| 문자 분류 | 사용 가능 문자집합
|---|:---:|
| `예약되지 않음` | [A-Za-z0-9], "-" , "_" , "." , "!" , "~" , "*" , "'" , "(" , ")"
| `예약됨` | ";", "/", "?", "!", "@", "&", "=", "+", "$", ","
| `이스케이프` | "%"

### 16.5.3 이스케이핑과 역이스케이핑
URI 이스케이프는 예약된 문자나 다른 지원하지 않는 글자들을 안전하게 URI에 삽입할 수 있는 방법을 제공한다.
이스케이프는 퍼센트 글자 하나와 뒤이은 16진수 글자 둘로 이루어진 세 글자 문자열이다.

### 16.5.4 국제 문자들을 이스케이핑하기
이스케이프 값들은US-ASCII 코드의 범위에 있어야 함에 주의하라

## 16.6 기타 고려사항

### 16.6.1 헤더와 명세에 맞지 않는 데이터
HTTP 헤더는 반드시 US-ASCII 문자집합의 글자들로만 이루어져야 한다.

### 16.6.2 날짜
HTTP 명세는 올바른 GMT 날짜 형식을 명확히 정의하고 있지만 모든 웹 서버와 클라이언트가 규칙을 따르고 있지 않음에 주의하라
