# 실습을 위한 개발 환경 세팅
* https://github.com/slipp/web-application-server 프로젝트를 자신의 계정으로 Fork 한다. Github 우측 상단의 Fork 버튼을 클릭하면 자신의 계정으로 Fork 된다.
* Fork 한 프로젝트를 eclipse 또는 터미널에서 clone 한다.
* Fork 한 프로젝트를 eclipse 로 import 한 후에 Maven 빌드 도구를 활용해 eclipse 프로젝트로 변환한다.(mvn eclipse:clean eclipse:eclipse)
* 빌드가 성공하면 반드시 refresh(fn + f5)를 실행해야 한다.

# 웹 서버 시작 및 테스트
* webserver.WebServer 는 사용자의 요청을 받아 RequestHandler 에 작업을 위임하는 클래스이다.
* 사용자 요청에 대한 모든 처리는 RequestHandler 클래스의 run() 메서드가 담당한다.
* WebServer 를 실행한 후 브라우저에서 http://localhost:8080으로 접속해 "Hello World" 메시지가 출력되는지 확인한다.

# 각 요구사항별 학습 내용 정리
* 구현 단계에서는 각 요구사항을 구현하는데 집중한다.
* 구현을 완료한 후 구현 과정에서 새롭게 알게된 내용, 궁금한 내용을 기록한다.
* 각 요구사항을 구현하는 것이 중요한 것이 아니라 구현 과정을 통해 학습한 내용을 인식하는 것이 배움에 중요하다.

### 요구사항 1 - http://localhost:8080/index.html로 접속시 응답

| 제목              | 내용                                                                                                                                                                                                                                                                                                       |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HTTP Header (예) | GET /index.html HTTP/1.1<br/>Host: localhost:8080<br/>Connection: keep-alive<br/>Accept: \*/\*                                                                                                                                                                                                           |
| (HINT) 1단계      | - InputStream 을 한 줄 단위로 읽기 위해 BufferedReader 를 생성한다.<br/> - BufferedReader.readLine() 메서드를 활용해 라인별로 HTTP 요청 정보를 읽는다.<br/> - HTTP 요청 정보 전체를 출력한다.<br/> - 헤더 마지막은 while(!"".equals(line)) {} 로 확인 가능하다.<br/> - line 이 null 값인 경우에 대한 예외 처리도 해야한다.<br/> 그렇지 않을 경우 무한루프에 빠진다.(if (line == null) { return; }) |
| (HINT) 2단계      | - HTTP 요청 정보의 첫 번째 라인에서 요청 URL( 위 예의 경우 /index.html 이다)을 추출한다.<br/> - String[] tokens = line.split(" ");를 활용해 문자열을 분리할 수 있다.<br/> - 구현은 별도의 유틸 클래스를 만들고 단위 테스트를 만들어 진행하면 편하다.                                                                                                                            |
| (HINT) 3단계      | - 요청 URL 에 해당하는 파일을 webapp 디렉토리에서 읽어 전달하면 된다.<br/> - byte[] body = Files.readAllBytes(new File("./webapp" + url).toPath());                                                                                                                                                                              |

요구사항대로 구현했으나, 예제의 `response200Header`메서드는 일괄적으로 헤더를 동일한 값(`text/html;charset=utf-8
`)으로 내보내고 있어 js, css 같은 추가적인 파일들의 요청은 제대로 처리하지 못하고 있다. 이건 차후의 요구사항6에서 처리할 예정이다.

#### 요청 헤더(Request Headers)
요청 헤더는 <필드 이름> : <필드 값> 쌍으로 이루어져 있다. 만약 필드 이름 하나에 여러개의 필드 값을 전달하고 싶다면 쉼표(,)를 구분자로 전달할 수 있다.
> ex) Accept: text/html,application/xhtml+xml 등등

이와 같이 클라이언트에서 요청을 받으면 서버는 클라이언트 요청에 대한 응답을 한다. HTTP 응답 또한 요청과 같이 헤더와 본문으로 구성되어 있다. 서버에서
클라이언트로 보내는 응답 메시지를 분석해 보면 다음과 같다.
```text
HTTP/1.1 200 OK                             <-- 상태라인
Content-Type: text/html; charset=utf-8      <-- 응답헤더
Content-Length: 20                          <-- 응답헤더
                                            <-- 헤더와 본문 사이의 빈 공백 라인
<h1>Hello World</h1>                        <-- 응답 본문
```
##### 상태 라인(Status Line)
응답 헤더의 첫 번째 라인은 상태 라인이라고 부른다. 응답 라인은 "HTTP-버전 상태코드 응답구문"으로 구성되어 있다. HTTP-버전은 HTTP 요청 라인(Request Line)의
HTTP-버전과 같은 의미이다. 상태코드는 응답에 대한 상태를 의미하는 코드 값으로 200은 성공을 의미한다. 응답 구문은 응답 상태에 대한 설명이다. 상태코드는 200
이외에도 다양한 상태코드가 있다.


### 요구사항 2 - get 방식으로 회원가입
* 

### 요구사항 3 - post 방식으로 회원가입
*

### 요구사항 4 - redirect 방식으로 이동
*

### 요구사항 5 - cookie
*

### 요구사항 6 - stylesheet 적용
*

### heroku 서버에 배포 후
* 