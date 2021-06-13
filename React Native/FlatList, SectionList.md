# FlatList

- 리액트에서 map을 통해 구현하던 리스트들을 리액트 네이티브에서는 FlatList로 대체할 수 있다.
- `ScrollView`와 달리 화면에 보여지는 ui만 먼저 렌더하므로 성능 최적화를 위해 사용된다. (스크롤 로딩 지원)
- 페이지네이션을 구현할 때 필수로 요구된다.
- 다중 칼럼 지원
- 헤더, 푸터 지원

## props

**pagenation**

- `onEndReached`  
  페이지가 해당 지점에 도달했을 때 호출되는 이벤트 핸들러
- `onEndReachedThreshold`  
  `onEndReached`가 실행될 지점을 지정하는 props  
  전체 페이지의 최상단을 0, 최하단을 1이라고 했을 때의 숫자 타입의 값

other props

- `ListEmptyComponent`  
  FlatList의 내용이 비어있을 때 render하는 컴포넌트
- `ListHeaderComponent`  
  list의 header에 해당하는 컴포넌트  
  FlatList 안에 따로 Header 요소를 넣는 것과 props를 사용하는 것의 차이는 스크롤했을 때의 header 컴포넌트의 위치이다.  
  ListHeaderComponent는 스크롤했을 때 기본적으로 고정된 위치값을 갖는다.
- `ListFooterComponent`  
  list의 가장 하단에 위치하는 컴포넌트  
  페이지네이션이 렌더되기 전 로딩 아이콘 혹은 로딩 컴포넌트를 넣는 것이 대표적이다.
