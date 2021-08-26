# Basic

> Next.js 공식문서 learn tutorial

## getting started

```
npx create-next-app
```

## About Next.js (advantage)

- next는 code spliting, 클라이언트 측의 네비게이션 기능, prefetching(프로덕션 모드에서)을 사용하여 알아서 어플리케이션의 퍼포먼스를 최적화한다.

  > code spliting (코드 분할)  
  > 클라이언트 사이드 렌더링의 경우 모든 데이터 (모든 페이지의 데이터)를 한 번에 가져오기 때문에 속도가 느려지는데,  
  > 필요한 데이터만 필요할 때 가져오기 위해서 코드를 분할 하는 것.

  > prefetching  
  > 데이터를 미리 받아오는 것  
  > 코드를 분활해서 그때그때, 필요할 때 데이터를 받아오려고 하면 또 속도가 느려질 수 있다.  
  > next는 Link 컴포넌트로 연결된 다음 페이지를 background에서 미리 다운받아놓는다.

## component

넥스트에서 제공하는 컴포넌트

### Link

- **클라이언트 측 네비게이션 기능**을 사용하기 위해 next에서 제공하는 Link 컴포넌트를 사용한다.  
  주의) 외부 페이지로 이동할 땐, a 태그를 사용한다.

  ```js
  import Link from 'next/link'
  ...
  return (
    <Link href='_path_'>
      <a>...</a>
    </Link>
  )
  ```

### Image

- 기본적으로 이미지 최적화를 제공한다.  
  반응형에서의 resizing, 최적화, 모던 웹에서 사용되는 webP같은 형식의 파일을 지원한다.
