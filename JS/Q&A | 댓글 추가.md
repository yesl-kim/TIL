### 문제
```html
<section class="contents">
    ...
    <div class="comments js-comments">
        <p>
            <span class="userName">objental</span>
            <a href="#">@5write</a> 사이트 주방 또는 데코 카테고리에서 상품 확인 가능하세요😊
        </p>
        <p>
            <span class="userName">jerrysmarket.official</span>
            와 너무 예뻐요
        </p>
    </div>
</section>
<form class="commentForm align-item-center space-between" name="commentForm">
    <button type="button">
        <img src="image/emoticon.svg" alt="이모티콘">
    </button>
    <input type="text" placeholder="댓글 달기..." class="inputComment" name="inputComment">
    <input type="submit" class="submitComment" name="submitComment" value="게시">
</form>
```
```js
const commentForm = document.commentForm,
    submitBtn = commentForm.submitComment,
    myId = 'yesl.k';

function addComment(e) {
    e.preventDefault();

    const inputComment = commentForm.inputComment.value,
        commentBox = document.querySelector('.js-comments'),
        p = document.createElement('p'),
        span = document.createElement('span'),
        comment = document.createTextNode(` ${inputComment}`);

    span.className = 'userName';
    span.innerText = myId;
    p.appendChild(span);
    p.appendChild(comment);
    commentBox.appendChild(p);
}

submitBtn.addEventListener('click', addComment);
```

- `span.userName`과 `p`의 텍스트노드와의 여백이 같지 않다.
- 인라인 요소는 html 문서에서 가독성을 위한 줄바꿈도 띄어쓰기처럼 인식한다.
- 댓글 `input.value` 앞에 공백을 추가한 뒤 p태그에 추가하면 다른 댓글과 여백이 같아진다.

Q. 공백을 안넣어도 되는 방법은 없을까?
- html 구조를 바꾸는 게 좋을까?
- `.userName`, 댓글 모두 `span`으로 묶은 뒤 마진으로 여백처리를 해주는 게 좋을까?