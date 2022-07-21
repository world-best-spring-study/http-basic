# Section 7. HTTP 헤더 - 일반 헤더
## HTTP 헤더 개요
### HTTP 헤더
```
header-filed = field-name ":" OWS fiels-value OWS (OWS: 띄어쓰기 허용)
```
- field-name은 대소문자 구분이 없다.
<br>

### HTTP 헤더 - 용도
- HTTP 전송에 필요한 모든 부가정보 포함
  - Ex) 메시지 바디의 내용, 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보 등등
- 표준 헤더가 너무 많다.
- 필요 시 임의의 헤더 추가 가능
<br>

### 과거 분류 방법 - RFC2616
- General 헤더: 메시지 전체(요청, 응답 상관없이)에 적용되는 정보
  - Ex) Connection: close
- Request 헤더: 요청을 보낼 때 들어가는 정보
  - Ex) User-Agent: Mozilla/5.0 (Macintosh; ..)
- Response 헤더: 응답에 들어가는 정보
  - Ex) Server: Apache
- Entity 헤더: 엔티티 바디 관련 정보
  - Ex) Context-Type: text/html, Content-Length: 3423
  - 메시지 바디는 엔티티 바디를 전달하는데 사용. 즉, 메시지 바디에 엔티티 바디를 담아 전달
  - 엔티티 바디는 요청이나 응답에서 전달할 실제 데이터
  - 엔티티 헤더는 엔티티 바디의 데이터를 해석할 수 있는 정보를 제공
    - 데이터 유형(html, json), 데이터 길이, 압축 정보 등
 <br>
 <br>
 
### HTTP 표준 변화
> RFC2616 폐기  
> 2014년 RFC7230 ~ 7235 등장
- 엔티티 → 표현(Respresentation)
- Representation = Representation Metadata + Representation Data
<br>
<br>

### HTTP 바디 - RFC7230(최신)
- 메시지 바디를 통해 표현 데이터 전달
- 메시지 바디 = 페이로드(payload)
- 표현(= 표현 헤더 + 표현 데이터): 요청이나 응답에서 전달할 실제 데이터
- 표현 헤더: 표현 데이터를 해석할 수 있는 정보 제공
  - Ex) 데이터 유형(html, json), 데이터 길이, 압축 정보 등
<br>
<br>
<br>
<br>

## 표현
```
Content-Type: 표현 데이터의 형식
Content-Encoding: 표현 데이터의 압축 방식
Content-Language: 표현 데이터의 자연 언어
Content-Length: 표현 데이터의 길이
```
- 표현 헤더는 전송, 응답 둘 다에 사용
<br>

### Content-Type
> 표현 데이터의 형식 설명
- 바디에 들어가는 데이터의 형식
- 미디어 타입, 문자 인코딩
- Ex) 
  - text/html; charset-utf-8
  - application/json
  - image/png
<br>

### Content-Encoding
> 표현 데이터 인코딩
- 표현 데이터를 압축하기 위해 사용
- 데이터를 전달하는 곳에서 압축 후 인코딩 헤더를 추가
- 데이터를 읽는 쪽에서 인코딩 헤더의 정보를 이용해 압축 해제
- Ex)
  - gzip
  - deflate
  - identity: 압축 X
<br>

### Content-Language
> 표현 데이터의 자연 언어
- 표현 데이터의 자연 언어를 나타낸다.
- Ex)
  - ko
  - en
  - en-US
<br>

### Content-Length
> 표현 데이터의 길이
- 바이트 단위
- Transfer-Encoding을 사용하면 Content-Length를 사용하면 안된다.
  - Transfer-Encoding 안에 정보가 다 들어있기 때문
<br>
<br>
<br>
<br>

## 콘텐츠 협상
> 클라이언트가 선호하는 표현 요청
```
Accept: 클라이언트가 선호하는 미디어 타입 전달
Accept-Charset: 클라이언트가 선호하는 문자 인코딩
Accept-Encoding: 클라이언트가 선호하는 압축 인코딩
Accept-Language: 클라이언트가 선호하는 자연 언어
```
- 협상 헤더는 요청 시에만 사용
<br>
<br>

### 협상과 우선순위 1
> Quality Values(Q)
- Quality Values(q)값 사용
- 0~1까지, 값이 클수록 높은 우선순위 의미
- 생략 시 1을 의미
<br>

```
GET /event
Accept-Language: ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
```
#### 우선순위
1. ko-KR;q=1(q 생략)
2. ko;q=0.9
3. en-US;q=0.8
4. en:q=0.7
<br>
<br>

### 협상과 우선순위 2
> Quality Values(q)
- 구체적인 것이 우선순위가 높다.
```
GET /event
Accept: text/*,text/plain,text/plain;format=flowed,*/*
```
#### 우선순위
1. text/plain;format=flowed
2. text/plain
3. text/*
4. */*
<br>

### 협상과 우선순위 3
> Quality Values(q)
- 구체적인 것을 기준으로 미디어 타입을 맞춘다.
```
Accept: text/*;q=0.3, text/html;q=0.7, text/html;level=1, text/html;level=2;q=0.4, */*;q=0.5
```
<img width="260" alt="스크린샷 2022-07-21 오후 5 46 26" src="https://user-images.githubusercontent.com/80838501/180171863-92fac62f-780e-4098-9292-2b1c13653b4c.png">
<br>
<br>
<br>
<br>

## 전송 방식
```
단순 전송
압축 전송
분할 전송
범위 전송
```
<br>

### 단순 전송
> Content-Length
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 3423

<html>
  <body>...</body>
</html>
```
- content에 대한 길이를 Content-Legnth로 지정해서 전송 <br>
→ content 길이를 알 수 있을 때 사용
- 한 번에 요청하고 한 번에 받는 것
<br>

### 압축 전송
> Content-Encoding
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Encoding: gzip
Content-Length: 521

asdkjfk;awjhnrefkljabnlkedfjblaksdjfgblkjdsb
```

- 압축해서 전송
- Content-Encoding 헤더를 추가해 어떤 것으로 압축했는지 알려줘야 한다.
<br>

### 분할 전송
> Transfer-Encoding
```
HTTP/1.1 200 OK
Content-Type: text/plain
Transfer-Encoding: chunked

5
Hello
5
Hello
0
\r\n
```
- 분할헤서 전송하면 오는대로 바로바로 사용 가능
- Content-Length 헤더를 포함하면 안된다.
<br>

### 범위 전송
> Range, Content-Range
```
HTTP/1.1 200 OK
Content-Type: text/plain
Conteent-Range: bytes 1001-2000 / 2000

qefoqwiefhoeiqhfoihoiqehfoihoqiehfoiqhfe
```
- 범위를 지정해 전송
- 중간에 전송하다 끊긴 경우, 처음부터 다시 보내지 않고 범위를 지정해 못받은 부분부터 전송할 때 사용
