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
        *** 항상 요청을 보내고 새 HTML 페이지를 반환 (서버-side: 템플릿, HTML 코드, 문서 랜더링, 다른 페이지로 리다이렉션)

    2.   Ajax 사용 Http Request
        -   JavaScript에서 요청과 응답 처리
        -   페이지 로드 없이, 데이터를 보내고 다시 가져올 수 있음.
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
        -   머신이 읽을 수 있는 방식으로 텍스트 데이터를 형식화(또는 구조화)
        -   HTML은 기본적으로 XML을 기반으로 함.
        -   HTML은 표준화 되어 있지만 XML은 아님.
        -   HTML은 표준화된 XML이라고도 말할 수 있음
        -   XML은 구조화되지 않은 텍스트 데이터를 구조화된 머신이 읽을 수 있는 형식으로 바꾸는데 유용함.
        -   XML은 너무 장황하여 더이상 사용되진 않음.
        -   XML -> JSON (JavaScript Object Notation) 으로 이동.

    3.  JSON의 생김새 (XML과 비교)
        -   자바스크립트 객체와 매우 유사
        -   자바스크립트 객체에서 사용하는 구조를 따름
        -   모든 키(key)에 큰따옴표("")가 쓰이는 차이 작은따옴표('')는 허용되지 않음
        -   중첩 객체도 허용 됨.
        -   JSON의 형태

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

## 4) GET Ajax 요청 기능 적용 (fetch() 함수 사용)

    1. `public/scripts/comments.js` 생성
        -   browser-side 자바스크립트 코드 작성

    2.  템플릿 내용 확인 (post-details.ejs)
        -   `Form tag`에서 POST`가 아닌 `GET` 요청을 사용
        -   버튼 클릭할 떄 새 URL을 얻기 위한 동작.

```html
<form action="/posts/<%= post._id %>/comments" method="GET">
  <button class="btn btn-alt">Load Comments</button>
</form>
```

    3.  템플릿 내용 변경 (post-details.ejs)
        -   `<Form>`을 제거하고 `EventListener` 추가
        -   `id="load-comments-btn"` 추가

```html
<button id="load-comments-btn" class="btn btn-alt">Load Comments</button>
```

    4.  이벤트 리스너 추가 (comments.js)
        -   `<button>` 참조 생성
        -   동작될 함수 추가

```JavaScript
const loadCommentsBtnElement = document.getElementById('load-comments-btn');

fuction fetchCommentsForPost() {

}

loadCommentsBtnElement.addEventListener('click', fetchCommentsForPost)
```

    5.  함수 정의 (fetchCommentsForPost()) - ajax 요청
        -   fetch() 함수 사용
            -   URL을 인수로 받아, 해당 URL에 대한 Http 요청을 보냄.
        -   fetch()함수는 비동기적으로 수행되며 `promise`를 반환하므로 `async` 사용 가능 (브라우저 측 코드 수행하므로 NodeJS와 관련 없음)
        -   자바스크립트에서 ID를 얻는 방법
            -   템플릿 버튼에 `data 속성` 추가하여 값 전달
            -   2가지 값 추출 방법
                -   이벤트 리스너로 전달되는 이벤트 객체
                -   `loadCommentsBtnElement`
            -   `템플릿 문자열` 사용 (`${name}`)
            -   promise를 반환하는 비동기 작업에선 `async-await` 사용 가능함.

```JavaScript
const loadCommentsBtnElement = document.getElementById('load-comments-btn');

async fuction fetchCommentsForPost() {
    const postId = loadCommentsBtnElement.dataset.postid;
    const response = await fetch(`/posts/${postId}/comments`);
}

loadCommentsBtnElement.addEventListener('click', fetchCommentsForPost)
```

```html
<button
  id="load-comments-btn"
  class="btn btn-alt"
  data-postid="<%= post._id %>"
>
  Load Comments
</button>
```

    6.  `const response`에 정보 저장
        -   response.headers : 메타데이터
        -   response.ok : 불리언(참,거짓)
        -   response.body : 서버에서 보낸 데이터 본문이지만 적절한 형식으로 변환 필요
        -   response.json() : 구문 분석된 자바스크립트 객체, 값으로 형식 변환, 프로미스로 반환하여 `await` 사용 필요.

```JavaScript
const loadCommentsBtnElement = document.getElementById('load-comments-btn');

async fuction fetchCommentsForPost() {
    const postId = loadCommentsBtnElement.dataset.postid;
    const response = await fetch(`/posts/${postId}/comments`);
    const responseData = await response.json();
}

loadCommentsBtnElement.addEventListener('click', fetchCommentsForPost)
```

### 7. 서버-사이드 응답 처리

        -   `res.json()`으로 필요한 데이터 전달
        -   `const post`가 필요하지 않음

```JavaScript
router.get('/posts/:id/comments', async function (req, res) {
  const postId = new ObjectId(req.params.id);
  // const post = await db.getDb().collection('posts').findOne({ _id: postId });
  const comments = await db
    .getDb()
    .collection('comments')
    .find({ postId: postId }).toArray();

  res.json(comments);
});
```

    8.  템플릿에 작성한 자바스크립트 코드 링크 추가
        -   `<script src="/scripts/comments.js" defer></script>` 추가

    9.  console.log(responseData) 값 확인
        -   데이터베이스에 저장된 댓글 배열을 콘솔에 표시

### 10. Ajax 응답을 기반으로 DOM 업데이트

    11.  `Load Comments`를 실제 댓글로 변경
        -   post-detail.ejs로 이동,
        -   댓글 섹션의 EJS가 적용되어 있지만 작동 안함.
        -   서버측에서 이 템플릿을 렌더링을 하지 않기 때문
        -   `EJS`는 템플릿을 렌더링할 때 서버에서 작동함

    12.  `Ajax`는 클라이언트 측 자바스크립트 코드가 댓글 정보를 가져 옴.
        -   댓글 섹션의 내용을 수동으로 교체 (EJS텍스트 제거)
        -   comments.js에서 DOM 수정
            -   <section id="comments"> 참조 생성하여 댓글 목록으로 교체
                -   `document.createElement()` 사용하여 `ol`, `li` 생성
                -   `ELS`는 템플릿 문자열로 교체
        -   백그라운드에서 데이터를 가져온 다음 현재 로드된 페이지의 일부를 업데이트 가능.

```JavaScript
//유틸리티 함수
const loadCommentsBtnElement = document.getElementById('load-comments-btn');
const commentsSectionElement = document.getElementById('comments');

function createCommentsList(comment) {
    const commentListElement = document.createElement('ol');

    for (const comments of comments) {
        const commentElemnet = document.createElement('li');
        commentElemnet.innerHTML = `
        <article class="comment-item">
            <h2>${comment.title}</h2>
            <p>${comment.text}</p>
        </article>
        `;
        commentListElement.appendChild(commentElement);
    }

    return commentListElement;
}

async fuction fetchCommentsForPost() {
    const postId = loadCommentsBtnElement.dataset.postid;
    const response = await fetch(`/posts/${postId}/comments`);

    const responseData = await response.json();

    const commentsListElement = createCommentsList(responseData);
    commentsSectionElement.innerHTML = '';
    commentsSectionElement.appendChild(commentListElement);
}

loadCommentsBtnElement.addEventListener('click', fetchCommentsForPost)
```

## 5) POST Ajax 요청 기능 적용 (fetch() 함수 사용)

- 댓글 작성 시, 전체 페이지를 다시 로드
- 로드됐던 모든 댓글이 사라짐

  1.  `post-detail.ejs` 댓글 작성 내용 확인
      - 시멘틱 HTML 고려하여 작업
      - `<form>` 태그에 직접 처리
      - `comments.js`에 `form 참조 변수` 추가
      - `querySelector`는 CSS 선택자 룰을 따름, 일치하는 첫번째 요소

```JavaScript
const commentFormElement = document.querySelector('#comments-form form')
```

    2. 이벤트 리스너 추가 (주의) & 콜백함수 작성
        -   `form`태그의 `버튼`은 `submit` 이벤트를 발생시킴.

```JavaScript
const commentFormElement = document.querySelector('#comments-form form')

function saveComment() {}

commentsFormElement.addEventListener('submit', saveComment)
```

        -   함수에서 기본 브라우저 동작(자동요청) 억제 필요.
            -   `event`객체로 `preventDefualt();` 메서드 사용
            -   서버에 자동으로 요청을 보내는 것을 방지
        -   수집할 데이터의 참조 생성 후 접근, 콘솔로그 확인

```JavaScript
const commentFormElement = document.querySelector('#comments-form form')
const commentTitleElement = document.getElementById('title');
const commentTextElement = document.getElementById('text');

function saveComment(event) {
    event.preventDefualt();

    const enteredTitle = commentTitleelement.value;
    const enteredText = commentTextElement.Value;

    console.log(enteredTitle, enteredText);
}

commentsFormElement.addEventListener('submit', saveComment)
```

    3.  `POST ajax` 요청 및 처리
        -   fetch 함수 작성
            -   `POST` 요청이므로 두번째 매개변수 추가
        -   `<form>`에 `data-postid` 속성 추가하여 postid 받기
        -   JSON으로 변환된 새 자바스크립트 객체 생성 (Ajax는 JSON으로 통신)
            -   `JSON` 메서드 사용
                -   parse : JSON을 원시 자바스크립트로 변환
                -   stringify : 원시 자바스크립트를 JSON으로 변환

```JavaScript
const commentFormElement = document.querySelector('#comments-form form')
const commentTitleElement = document.getElementById('title');
const commentTextElement = document.getElementById('text');

function saveComment(event) {
    event.preventDefualt();
    const postId = commentsFormElement.dataset.postid;

    const enteredTitle = commentTitleelement.value;
    const enteredText = commentTextElement.Value;

    const comment = { title: enteredTitle, text: enteredText};

    fetch(`/posts/${postId}/comments`, {
        method: 'POST',
        body: JSON.stringify(comment)
    })
}

commentsFormElement.addEventListener('submit', saveComment)
```

    4.  `서버-사이드` 응답 처리
        -   `req.body`는 `urlencoded`인 경우에만 작동
            -   `app.use(express.urlencoded)` 미들웨어 설정했었음
            -   기본 브라우저 실행을 억제했기때문에 작동 안함
            -   대신, JSON 데이터를 보내고 있음.
                -   JSON을 구문분석하는 미들웨어 추가
                -   들어오는 모든 요청을 확인 후, JSON 형식이면 구문분석

```JavaScript
    app.use(express.json());
```

        -   `라우트 응답` 수정

```JavaScript
    res.json({message: 'Comment added!'})
```

    5.  `미들웨어`의 작동 (중요)
        -   구문분석의 작동유무가 아닌 수신되는 요청의 헤더를 확인함.
        -   일부 추가 첨부된 메타데이터에 `req.body`의 인코딩에 관한 정보가 포함되어야 함. (브라우저에서는 자동 설정)
        -   fetch()로 직접 요청을 보냈기때문에 메타데이터 작업이 필요함.
        -   `네트워크탭-comments-header-metadata`처럼 설정필요함.
        -   메타데이터 설정

```JavaScript
    fetch(`/posts/${postId}/comments`, {
        method: 'POST',
        body: JSON.stringify(comment),
        header: {
            'Content-Type': 'application/json'
        }
    })
```

## 6) 사용자 경험(UX) 개선

    -   댓글 작성 후 작성된 댓글 포함하여 리스트 출력
    -   fetch()는 프로미스를 반환, 응답을 받으면 프로미스가 해결 됨.

```JavaScript
const commentFormElement = document.querySelector('#comments-form form')
const commentTitleElement = document.getElementById('title');
const commentTextElement = document.getElementById('text');

async function saveComment(event) {
    event.preventDefualt();
    const postId = commentsFormElement.dataset.postid;

    const enteredTitle = commentTitleelement.value;
    const enteredText = commentTextElement.Value;

    const comment = { title: enteredTitle, text: enteredText};

    const response = await fetch(`/posts/${postId}/comments`, {
        method: 'POST',
        body: JSON.stringify(comment),
        header: {
            'Content-Type': 'application/json'
        }
    })

    fetchCommentsForPost();
}

commentsFormElement.addEventListener('submit', saveComment)
```

    -   댓글이 없는 경우 처리
        -   `responseData`에 조건문(if) 사용
            -   필요한 단락에 접근 후 내용 수정
                -   `firstElementChild.textContent` 사용

```JavaScript
async fuction fetchCommentsForPost() {
    const postId = loadCommentsBtnElement.dataset.postid;
    const response = await fetch(`/posts/${postId}/comments`);

    const responseData = await response.json();

    if (responseData && responseData.length > 0) {
        const commentsListElement = createCommentsList(responseData);
        commentsSectionElement.innerHTML = '';
        commentsSectionElement.appendChild(commentListElement);
    } else {
        commentsSectionElement.firstElementChild.textContent =
        'We could not find any comments. Maybe add one?';
    }
}
loadCommentsBtnElement.addEventListener('click', fetchCommentsForPost)
```

    -   오류 처리 (서버-사이드)
        -   데이터베이스 등에 연결할 수 없는 경우
            -   `ok` 속성 사용

```JavaScript
    const response = await fetch(`/posts/${postId}/comments`, {
        method: 'POST',
        body: JSON.stringify(comment),
        header: {
            'Content-Type': 'application/json'
        }
    })

    if (response.ok) {
        fetchCommentsForPost();
    } else {
        alert('Could not send comment!');
    }

commentsFormElement.addEventListener('submit', saveComment)
```

    -   기술적인 에러 발생 (fetch() 함수 내부)
        -   POST 응답

```JavaScript
    try {
        const response = await fetch(`/posts/${postId}/comments`, {
            method: 'POST',
            body: JSON.stringify(comment),
            header: {
                'Content-Type': 'application/json'
            },
    });
    if (response.ok) {
        fetchCommentsForPost();
    } else {
        alert('Could not send comment!');
    }
    } catch (error) {
        alert('Could not send request - maybe try again later!');
    }


commentsFormElement.addEventListener('submit', saveComment)
```

        -   GET 응답

```JavaScript
async fuction fetchCommentsForPost() {
    const postId = loadCommentsBtnElement.dataset.postid;

    try {
        const response = await fetch(`/posts/${postId}/comments`);

        if(!response.ok){
            alert('Fetching comments failed!');
            return;
        }
    if (responseData && responseData.length > 0) {
        const commentsListElement = createCommentsList(responseData);
        commentsSectionElement.innerHTML = '';
        commentsSectionElement.appendChild(commentListElement);
    } else {
        commentsSectionElement.firstElementChild.textContent =
        'We could not find any comments. Maybe add one?';
    }
    } catch (error) {
        aleart('Getting comments failed!');
    }
}
```

## 7) 추가 HTTP Method (GET, POST 외)
    -   Default browser Http methods
        -   GET : Fetch some data
            -   Enter a URL, (ajax) method="GET"
            -   Click a link
            -   Form with method="GET"
        -   POST : Store some data
            -   Form with method="POST"
            -   (ajax) method="POST"
    -   Available via Ajax / JS-driven Http requests
        -   fetch()함수와 같이 사용
        -   PUT : Replace / update some data
            -   method: 'PUT'
            -   전체 문서가 대체되는 경우 주로 사용
        -   PATCH : Update some data
            -   method: 'PATCH'
            -   전체 문서의 일부가 업데이트되는 경우 사용
        -   DELETE : Delete some data
            -   method: 'DELETE'

## 모듈 요약
    -   브라우저 측 자바스크립트 내부에서 직접 보내는 백그라운드 요청
    -   페이지 리로드 없이 백그라운드 작업 실행
    -   `Axio` 라이브러리도 많이 사용 됨.(HXMLHttpReqeust)
    -   `POST 요청` 시, JSON형식과 적절한 Header 필요함
    -   에러 핸들링 (서버-사이드, 기술적문제)