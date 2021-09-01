# HTTP

> [JaeYeopHan / Interview_Question_for_Beginner | HTTP의 GET과 POST의 비교](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/Network#http%EC%9D%98-get%EA%B3%BC-post-%EB%B9%84%EA%B5%90)  
> [MDN | HTTP 메소드](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)  
> [제로초 블로그 | HTTP 메소드](https://www.zerocho.com/category/HTTP/post/5b3723477b58fc001b8f6385)

- [ ] HTTP GET 캐싱
- [ ] POST 데이터 길이 제한, 있다? 없다?

---

## HTTP 메소드

HTTP 메소드는 **리소스에 대해 수행하길 원하는 행동**을 나타낸다 (-> 동사로 표현됨)  
REST하게, '리소스가 무엇인지, 그리고 어떻게 하길 원하는지'가 잘 드러나도록 하기 위해 설계원칙에 맞게, 그리고 적절한 용도에 따라 메소드를 사용하는 것이 바람직하다.

### GET vs POST

GET 메소드는 데이터를 조회할 때, POST는 데이터를 변경할 때 사용하도록 설계되었다. 이 둘은 각자 다른 특징이 있기 때문에 특히 구분해서 사용해야 한다.

#### GET

- 요청을 보낼 때 필요한 데이터가 body가 아닌 header url에 담겨 보내진다.  
  즉, 서버로 보내는 데이터가 url의 쿼리스트링으로 드러난다.  
  때문에 보낼 수 있는 데이터의 크기가 제한적이고, 보안에 취약하다

- GET 방식의 요청은 브라우저에서 캐싱될 수 있다.  
  불필요한 서버 요청을 줄이기 위해 이전에 했던 요청과 같은 요청을 할 경우 캐싱된 리소스를 재사용한다. (-> 성능 향상!)  
  _POST로 보내야 할 요청을, 크기가 작고 보안에 문제되지 않는다는 이유로 GET 방식으로 보낼 경우, 캐싱된 데이터로 응답을 받을 수 있다._
  > HTTP 캐싱은 선택적이지만, GET 요청에 대해서만 캐싱할 수 있다.  
  > 다른 메소드들은 캐싱 대상에서 제외된다.

#### POST

- 요청을 보낼 때 필요한 데이터가 body에 담긴다.  
  때문에 비교적 GET보다 큰 데이터를 보낼 수 있다.  
  하지만 그렇다고 <u>보안에 강한 것은 아니다.</u> url에 드러나지만 않을 뿐 개발자 도구와 같은 툴을 사용하여 도청은 가능하다.  
  보안에 민감한 데이터라면 암호화를 해야한다.

  > body에 담아 보내는 데이터에는 길이 제한이 없을까?  
  > 버전 1.1에는 없고, 1.0 버전에는 있을 수 있다.

### 그 외 메소드: PUT, PATCH, DELETE, OPTIONS

- **PUT** 대체, 전체 수정
- **PATCH** 부분 수정
- **DELETE** 삭제
- **OPTIONS**  
  서버에서 지원하는 HTTP 메소드를 확인하기 위해 사용  
  실제 요청을 보내기 전의 테스트 용도
