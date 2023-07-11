# 브라우저 렌더링 과정

## 요약

**메인 스레드**

1. `Parsing` -> `DOM tree`
2. `Style`
   - 스타일시트 생성: 외부 css 파일, head 태그 안의 style, 인라인 스타일 정보들을 스타일시트로 생성 ( TODO: css, style, 인라인 스타일 모두 바로 브라우저가 인식할 수 없는건가? 스타일시트는 브라우저가 해석할 수 있는 정보인가?)
   - px 단위로 변환: 각 화면, 프레임은 비트맵 이미지로 렌더링되기 때문에 각 요소의 rem, em, %와 같은 상대크기 단위를 비트맵 단위인 픽셀 단위로 변환
   - 스타일 계산: 오버라이딩을 포함하여 최종 스타일 정보를 계산 (computed style)
3. `Layout` -> `Layout tree`
   - DOM tree와 스타일 정보 병합(?)
   - 요소가 어디에 위치해야하는지에 대한 정보
4. `PrePaint` -> `property tree`
   1. 검증: 이전 단계에서 변경이 일어나면 캐시된 정보 모두 무효화 -> 다시 계산
   2. Property tree 생성
      - 레이어 단위로 적용되어야 하는 속성 저장
      - 레이어를 합성하는 단계에서 속성 트리에 저장된 속성 적용 (요소를 다시 순회할 필요x)
      - ex. opacity, clip, effect, trasform 등
5. `Paint` -> `paint tree`
   - '요소를 화면에 어떻게 그려야하는지'에 대한 정보
   - 위치, 색상 정보 등
   - 다수의 Layout object layer가 하나의 paint layer로 병합 및 분리
   - tranform, position 등의 속성을 가진- 조건에 부합하는 레이어가 paint layer로 분리
6. `Layerize` -> `composited layer`
   - Paint layer를 composited layer로 병합 및 분리
   - traslateZ, traslate3d, {position: fixed}, video, iframe, 스크롤 영역이 있는 레이어 분리
   - compoisted layer는 **독립적인 픽셀화**가 가능 -> 이미 픽셀화된 레이어의 합성을 통해 스크롤시 새로운 프레임을 빠르게 렌더링할 수 있음
7. `Commit`
   - 독립된 composited layer를 합성 스레드로 전달

**합성 스레드**

- 스크롤 이벤트는 합성 스레드에서 처리
- 단, 이벤트 핸들러가 등록된 영역은 제외
- 이벤트 핸들러가 등록된 영역은 자바스크립트로 수행되기 때문에 메인스레드에서 처리 -> 이런 영역이 커질수록/전체 문서에 이벤트 핸들러를 등록한 경우 스크롤 성능 저하를 일으킬 수 있다.
- 문서에 이벤트 핸들러를 등록해야하는 경우 {passive: true} 속성을 지정해주면 이벤트를 메인스레드에서 처리하는 동안 합성스레드에서 새 프레임 렌더링을 멈추지 않고 계속 진행한다.

1. `Tiling`
   - 하나의 레이어를 여러 개의 타일로 분할
   - 각 타일은 뷰포트 포함여부에 따라 다른 우선순위로 래스터화
2. `Raster`
   > ~~Raster? 픽셀화. 비트맵 이미지로 만드는 것~~
   - 각 타일별로 래스터화 -> 비트맵 이미지로 GPU 메모리에 저장
   - 합성 스레드의 Pending Tree에서 raster -> (새로운 커밋이 들어오면) Draw quad를? Active Tree로 복제, 전달 -> 하나의 레이어로 합성? -> GPU로 전달 -> 프레임 렌더링
   - 합성 스레드의 Pending Tree와 Active Tree는 서로 비동기로 처리된다.

- [ ] Draw Quad란?
- [ ] raster란?
- [ ] raster 과정이 아직 좀 아리송한듯

## 참고

- [soso blog | 렌더링 성능 개선(1) — 렌더링 과정 이해하기](https://so-so.dev/web/browser-rendering-process/)
