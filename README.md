# 섹션 28, Ajax - 비동기식 JS기반 Http 요청

    1.   JavaScript 기반 Http Reqeust
    2.   Handling Response
    3.   Error 처리

## 1) Ajax란? (Asynchronous JavaScript And XML)

    1.   브라우저 측 자바스크립트를 통한 Http Request
    2.   <form action>이 아닌 JavaScript Http Request
    3.   JavaScript 2가지 내장 기능
        -   XMLHttpRequest 객체
        -   fetch() 함수

## 2) JavaScript 내부에서 Http Request가 필요한 이유

    1.   Ajax 없는 Http Request
        -   URL 입력 (GET request)
        -   링크 클릭 (GET request)
        -   Form submission (GET or POST Request)
        *** 항상 요청을 보내고 새 HTML 페이지를 반환 (서버side: 템플릿, HTML 코드, 문서 랜더링, 다른 페이지로 리다이렉션)

    2.   Ajax 사용 Http Request
        -   JavaScript에서 요청과 응답 처리
        -  페이지 로드 없이, 데이터를 보내고 다시 가져올 수 있음.
        -   로드 된 페이지의 필요한 부분만 업데이트

## 3) Ajax (Asynchronous JavaScript And XML)

    1.   XMLHttpRequest (자바스크립트 내장 객체)
        -   XML data 전송
        -   투박한 사용성 (복잡)
        -   타사 패키지(라이브러리) 사용 ('axios')
    2.   fetch() 함수 (XML 대안)

### 3-1) XML (Extensible Markup Language)

    1.  생김새 (HTML과 유사)

```XML
<post-comment>
    <title>This was great!</title>
    <comment-text>Thanks a lot - this was really helpful!</comment-text>
</post-comment>
```

    2.  개념
        -   머신이 읽을 수 있는 방식으로 텍스트 데이터 형식화(또는 구조화)
        -   HTML은 기본적으로 XML을 기반으로 함.
        -   HTML은 표준화 되어 있지만 XML은 아님.
        -   HTML은 표준화된 XML이라고도 말할 수 있음
        -   XML은 구조화되지 않은 텍스트 데이터를 구조화된 머신이 읽을 수 있는 형식으로 바꾸는데 유용함.
        -   XML은 너무 장황하여 더이상 사용되진 않음.
        -   XML -> JSON (JavaScript Object Notation) 으로 이동.
    3.  JSON의 생김새
        -   자바스크립트 객체와 매우 유사
        -   자바스크립트 객체에서 사용하는 구조를 따름
        -   모든 키(key)에 큰따옴표("")가 쓰이는 차이 작은따옴표('')는 허용되지 않음
        -   중첩 객체도 허용 됨.
```JSON
{
    "title": "This was great!",
    "commentText": "Thanks a lot - this was really helpful!"
}
```
    4.  Http Request에서는 XML과 JSON 둘 다 작동함
    5. Ajax에서의 'x'는 이제 애매할 수 있다 (대체로 JSON을 사용)

### 3-2) fecth() 함수
    -   내장 XMLHttpRequest 객체의 대안
    -   브라우저, 자바스크립트 내장 기능
    -   비교적 최근 자바스크립트에 추가된 현대적인 함수
    -   XMLHttpRequest는 프로미스를 지원하지 않음
        -   'axios' 라이브러리를 통해 프로미스를 지원함.
    -   비교적 편리한 사용

## 4) GET Ajax 요청
    1. `public/scripts/comments.js` 생성
        -   browser-side 자바스크립트 코드 작성
    
    2.  템플릿 내용 (post-details.ejs)
        -   `Form tag`에서 POST`가 아닌 `GET` 요청을 사용
        -   버튼 클릭할 떄 새 URL을 얻기 위한 동작.
```html
<form action="/posts/<%= post._id %>/comments" method="GET">
    <button class="btn btn-alt">Load Comments</button>
</form>
```
        -   `<Form>`을 제거하고 `EventListener` 추가
        -   `id="load-comments-btn"` 추가


