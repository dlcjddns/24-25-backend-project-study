# 1주차 - 환경 구축, 프로그램 설정, HTTP/API에 대한 기초적인 이해와 기본 API 생성하기

## 목표

- HTTP에 대한 기초적인 이해 확인(Header, Body, Method 등)
- HTTP API 서버를 만들기 위한 Local 환경 구축
- HTTP API 서버를 만들기 위한 기본적인 코드 작성

> 혹시나, 아래 내용에 대해 이미 알고 계시다면, 지나쳐서 바로 문제와 환경 구축으로 넘어가시면 됩니다!

## HTTP란 무엇인가?

> ### MDN Web Document - HTTP Overview
>
> https://developer.mozilla.org/ko/docs/Web/HTTP/Guides/Overview

HTTP(Hypertext Transfer Protocol)는 인터넷에서 정보를 주고받는 가장 기본적인 방법입니다. 웹 브라우저로 웹사이트를 방문할 때마다 HTTP가 작동하고 있습니다.

HTTP에 대해해 여러분들이 가장 먼저 이해해야 하는 것은, Server-Client 구조입니다. 이는 음식점에 비유하면, 음식점을 Server에, 손님을 Client에 비유할 수 있습니다.

Client가 Server가 제공해 줄 수 있는 기능(메뉴) 중 하나의 규격에 맞춰 요청하면, 서버는 그 요청에 대한 결과를 제공하는 Request-Response 구조로 되어 있습니다.

이 기능에는 이미지, HTML 등 웹사이트 파일을 제공하는 것, 로그인을 위한 ID/비밀번호를 확인하는 것, 그 외에 서버의 용도에 따라 다양한 기능(쇼핑몰 서버일 경우 현재 판매 중인 제품 리스트 등)이 예시가 될 수 있습니다.

이 외에 조금 더 HTTP에 대해 깊게 알아보고 싶으시다면, 위의 MDN 링크를 참고하시면 좋습니다.

> 여담: 이 Server-Client는 Client에서 요청하지 않으면 아무것도 이뤄지지 않는 구조이기 때문에, 최근의 채팅/알림 등의 특수한 수요를 만족하기 위해서, Websocket, Server-Sent HTTP Event 등의 구조가 추가적으로 제안되었습니다. 이는 다음에 기회가 있을때 추가적으로 다뤄보도록 하죠!

## API란 무엇인가?

API(Application Programming Interface)는 서로 다른 소프트웨어 프로그램이 통신할 수 있게 해주는 방법입니다. API는 프로그램 간의 '대화 방식'을 정의하는 규칙 모음이라고 생각하면 됩니다.

> 많은 API 서비스가 HTTP를 기반으로 제공되지만, 그렇지 않은 API들도 많이 있습니다!(SOAP, 일반 소켓 통신 등)

API를 쓰는 이유가 무엇일까요?

1. 클라이언트가 사용하기 복잡한 기능을 단순화하여 제공
1. 비슷하거나 같은 기능을 반복해서 개발하지 않고, API를 통해 재사용 가능
1. DB, 컴퓨팅 자원 등을 외부에 노출하지 않고, API를 통해 제한적으로만 접근 가능하게 함으로써 보안성 강화

## REST API
REST API(Representational State Transfer Application Programming Interface)는 웹 서비스를 구축하기 위한 아키텍처 스타일로, HTTP 프로토콜을 기반으로 하는 API 설계 방식입니다. 2000년에 Roy Fielding이 자신의 박사학위 논문에서 처음 소개한 개념으로, 오늘날 웹 API 개발의 표준으로 자리 잡았습니다.
### REST API의 주요 특징
1. 자원(Resource) 중심의 설계

    REST API는 모든 것을 '자원(Resource)'으로 간주합니다. 각 자원은 고유한 URI(Uniform Resource Identifier)로 식별됩니다.

    예: `/users`, `/products/123`, `/posts/456/comments`

1. HTTP 메서드를 활용한 행위 정의

    REST API는 HTTP 메서드를 사용하여 자원에 대한 행위(CRUD 작업)를 정의합니다.

    - GET: 자원 조회
    - POST: 자원 생성
    - PUT/PATCH: 자원 수정 (PUT은 전체 수정, PATCH는 부분 수정)
    - DELETE: 자원 삭제

1. 무상태성(Statelessness)

    각 요청은 이전 요청과 독립적이며, 서버는 클라이언트의 상태를 저장하지 않습니다. 필요한 모든 정보는 요청 자체에 포함되어야 합니다.
1. 표현(Representation)

    자원의 상태는 JSON, XML, HTML 등의 형식으로 표현됩니다. 일반적으로 현대 API에서는 JSON 형식이 가장 널리 사용됩니다.
1. 자체 설명적(Self-descriptive)

    REST API는 메시지만 보고도 그 의미를 이해할 수 있어야 합니다.
1. HATEOAS(Hypermedia as the Engine of Application State)

    클라이언트가 서버로부터 어떤 요청을 할 수 있는지 응답에 포함된 링크를 통해 동적으로 알 수 있어야 합니다.
1. HTTP 상태 코드를 통한 API 결과 전달

    서버는 클라이언트에게 메시지 뿐만 아니라 HTTP 상태 코드를 같이 전달하여, API 실행의 결과를 정량적으로 나타낼 수 있습니다.
    - 2xx: 성공
    - 3xx: 리다이렉트(URL 이동)
    - 4xx: 클라이언트 요청 오류
    - 5xx: 서버 처리 중 오류
    
    자세한 내용은 [아래](#추가2-http-상태-코드)에서 설명합니다!
### REST API 설계 원칙
1. URI는 자원을 나타내야 함
    ```
    좋은 예: /users
    나쁜 예: /getUsers
    ```
1. 행위는 HTTP 메서드로 표현
    ```
    사용자 조회: GET /users
    사용자 생성: POST /users
    사용자 수정: PUT /users/123
    사용자 삭제: DELETE /users/123
    ```
1. 계층 구조는 URI 경로로 표현
    ```
    특정 사용자의 게시물 목록: GET /users/123/posts
    특정 게시물의 댓글 목록: GET /posts/456/comments
    ```
1. 일관된 형식 사용(컨벤션)

    - URI는 소문자를 사용
    - 복수형 명사 사용 권장 (예: /users, /products)
    - 언더스코어(_) 대신 하이픈(-) 사용 (예: /recently-updated)

    이런 컨벤션은, 프로젝트 혹은 회사에 따라 모두 다를 수 있기 때문에, 사전에 정하고 진행하는 것도 나쁘지 않을 수 있습니다! 통일된 API는 사용자들이 기능을 예측하고 사용하는데 무리가 없도록 합니다.

### REST API의 장점

1. 간단하고 직관적: HTTP 프로토콜의 기본 원리를 따르기 때문에 이해하기 쉽습니다.
1. 플랫폼 독립적: 어떤 프로그래밍 언어나 플랫폼에서도 사용할 수 있습니다.
1. 확장성: 클라이언트와 서버가 독립적으로 개발될 수 있어 확장하기 쉽습니다.
1. 캐싱 가능: HTTP의 캐싱 기능을 활용할 수 있습니다.
1. 보안성: HTTPS를 통한 암호화와 인증 메커니즘을 쉽게 구현할 수 있습니다.

### REST API의 실제 예시
회원 관리 API를 예로 들면:

- 회원 목록 조회: `GET /members`
- 특정 회원 조회: `GET /members/123`
- 새 회원 등록: `POST /members` (요청 본문에 회원 정보 포함)
- 회원 정보 수정: `PUT /members/123` (요청 본문에 전체 회원 정보 포함)
- 회원 정보 일부 수정: `PATCH /members/123` (요청 본문에 수정할 정보만 포함)
- 회원 삭제: `DELETE /members/123`

### (추가)REST API에서의 URL 구성 요소
REST API에서 URL은 자원을 식별하는 중요한 수단입니다. URL은 여러 구성 요소로 이루어져 있으며, 특히 Path Parameter와 Query Parameter는 API 설계에서 핵심적인 역할을 합니다.

#### URL의 주요 구성 요소
```
https://api.example.com/v1/resources/items/123?sort=desc&limit=10
```
이 URL은 다음과 같은 구성 요소로 나눌 수 있습니다:

1. 프로토콜(Protocol): `https://`
1. 호스트(Host): `api.example.com`
1. 경로(Path): `/v1/resources/items/123`

    - 기본 경로: /v1/resources/{items}
    - Path Parameter: /123


1. 쿼리 문자열(Query String): `?sort=desc&limit=10`

    - Query Parameter: `sort=desc`, `limit=10`
    - 각 파라미터는 {key}={value} 형식으로 이루어져 있으며, 이를 &를 통해 구분하는 형식([x-www-form-urlencoded](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods/POST#url-encoded_form_submission))

#### Path Parameter와 Query Parameter 비교
1. Path Parameter

    - URL 경로에 포함되는 변수
    - 형식: `/resources/{parameter}`
    - 주로 특정 자원을 식별하는 데 사용
    - 필수적인, 그리고 뒤에 추가적으로 확장하는 값에 적합
    - 예: `/users/123`, `/books/978-3-16-148410-0`

1. Query Parameter

    - **URL의 끝에** ? 다음에 오는 키-값 쌍
    - 형식: `?key1=value1&key2=value2`
    - 주로 정렬, 필터링, 페이지네이션 등의 옵션 지정에 사용
    - 선택적인 값에 적합
    - 예: `/users?role=admin&status=active`, `/products?category=electronics&sort=price`

### (추가2) HTTP 상태 코드
HTTP 상태 코드는 서버가 클라이언트의 요청에 대한 처리 결과를 알려주는 3자리 숫자입니다. REST API에서 상태 코드는 매우 중요한 역할을 하며, 클라이언트에게 모든 시스템에서 요청 처리 상태에 대한 일관되고 명확한 정보를 제공합니다.

#### 상태 코드의 분류
HTTP 상태 코드는 첫 번째 숫자에 따라 5개의 클래스로 분류됩니다:
- 1xx (정보 응답)

    요청이 수신되어 처리 중임을 나타냅니다.
    
    거의 사용되지 않으며, REST API에서는 일반적으로 볼 수 없습니다.

- 2xx (성공 응답)

    요청이 성공적으로 처리되었음을 나타냅니다.
    
    REST API에서 가장 자주 사용되는 코드들입니다.

- 3xx (리다이렉션)

    클라이언트가 요청을 완료하기 위해 추가 작업이 필요함을 나타냅니다.

    주로 리소스의 위치가 변경되었을 때 사용됩니다.

- 4xx (클라이언트 오류)

    클라이언트의 요청에 문제가 있음을 나타냅니다.

    잘못된 요청, 인증 실패, 권한 부족 등의 상황에서 사용됩니다.

- 5xx (서버 오류)

    서버가 유효한 요청을 처리하지 못했음을 나타냅니다.

    서버 내부 오류, 서비스 사용 불가(데이터베이스 연결 오류, 서버 리소스 부족 등)의 상황에서 사용됩니다.

#### 자주 사용되는 코드
1. 2xx (성공)

    - 200 OK: 요청이 성공적으로 처리됨 (GET, PUT, PATCH)
    - 201 Created: 새 리소스가 성공적으로 생성됨 (POST)
    - 204 No Content: 요청은 성공했지만 응답 본문이 없음 (DELETE)

1. 3xx (리다이렉션)

    - 301 Moved Permanently: 리소스가 영구적으로 다른 위치로 이동함(브라우저나, 진보된 HTTP 클라이언트일 경우 Redirection 실행)
    - 304 Not Modified: 캐시된 버전의 리소스가 여전히 유효함

1. 4xx (클라이언트 오류)

    - 400 Bad Request: 요청의 형식이 잘못됨(JSON 파싱 에러 등)
    - 401 Unauthorized: 인증이 필요함
    - 403 Forbidden: 인증은 됐지만 권한이 없음
    - 404 Not Found: 요청한 리소스를 찾을 수 없음
    - 405 Method Not Allowed: 해당 리소스에 대해 요청한 HTTP 메서드가 허용되지 않음
    - 409 Conflict: 요청이 현재 서버의 상태와 충돌함 (예: 이미 존재하는 이메일로 회원가입)
    - 429 Too Many Requests: 너무 많은 요청을 보냄 (API 요청 제한)

1. 5xx (서버 오류)

    - 500 Internal Server Error: 서버 내부 오류
    - 502 Bad Gateway: 게이트웨이나 프록시 서버가 잘못된 응답을 받음
    - 503 Service Unavailable: 서버가 일시적으로 요청을 처리할 수 없음
    - 504 Gateway Timeout: 게이트웨이나 프록시 서버의 요청 시간 초과. 보통은 서버에서 직접 보내지 않는 결과.



## 문제
아래 링크 안 문제들을 answers.md 파일로 새로 저장 후 풀어서, 본인 이름 폴더의 week1 폴더에 과제와 함께 커밋해 주세요!

[문제 풀기](./problems.md)

## 과제
아래 링크 안 과제 과정을 따라해서, REST API 개발 기반을 위한 작업과, 스스로의 기술 스택에 맞는 초기 프로젝트 설정을 진행 후, 이를 개인 폴더의 task 폴더에 업로드해 주세요!

[과제 확인하기](./tasks.md)