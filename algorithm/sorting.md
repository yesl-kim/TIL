# 정렬 알고리즘

> [제로초 알고리즘](https://www.zerocho.com/category/Algorithm)

정렬 알고리즘 종류와 자바스크립트 코드

## 삽입 정렬(Insertion Sort)

- 배열의 첫번째 요소는 정렬을 마쳤다고 가정하고 두번째요소부터 정렬을 시작한다.

- 선택한 요소를 기준으로 왼쪽 요소와 비교해서 왼쪽 요소보다 작으면 위치를 바꾼다.
- 선택한 요소가 왼쪽 요소보다 크거나 배열의 가장 왼쪽(인덱스 0)에 위치하면 정렬을 멈춘다.
- 이를 배열의 마지막 요소까지 검사하며 정렬한다.
- 시간복잡도가 좋지 못하다. O(n^2)

  | 공간복잡도 | 시간복잡도 |
  | :--------: | :--------: |
  |    O(1)    |   O(n^2)   |

### 코드

```js
var insertSort = arr => {
  var i = 1, j, temp;
  for (i; i < arr.length; i++) {
    temp = arr[i]
    for (j = i - 1; j >= 0 && temp < arr[j] j--) {
      arr[j + 1] = arr[j]
    }
    arr[j + 1] = temp
  }
  return arr
}
```

---

## 버블 정렬 (Bubble Sort)

- 배열의 가장 좌측부터 두 수씩 비교하여 왼쪽 숫자가 오른쪽 숫자보다 크면 자리를 바꾼다.
- 정렬을 배열의 가장 우측까지 진행하면 다시 처음부터 반복
- 이 때 처음 이후 정렬부터는 배열의 끝까지 검사하지 않고, 최우측 숫자는 정렬이 완료됐다고 간주하여 검사에서 제외
- 정렬 알고리즘 중 성능이 가장 안좋다. O(n^2)

  | 공간복잡도 | 시간복잡도 |
  | :--------: | :--------: |
  |    O(1)    |   O(n^2)   |

## 버블 정렬 함수 작성하기

```js
function bubbleSort(arr) {
  let max, i, j;
  for (i = 0; i < arr.length - 1; i++) {
    for (j = 0; j < arr.length - 1 - i; j++) {
      if (arr[j] > arr[j + 1]) {
        max = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = max;
      }
    }
  }
  return arr;
}
```

---

## 합병 정렬 (Merge Sort)

- 준비물: 원본 배열을 둘로 나눈 배열(이하 left, right)과 빈 배열(이하 sorted)

- **분할과 정렬의 반복**
- 배열 left와 right의 가장 좌측 요소를 비교하여 작은 수를 sorted에 넣는다.
- 배열을 둘로 나누고, 나눈 배열끼리 비교하여 정렬하는 것을 재귀적으로 실행한다.
- 즉, 배열을 쪼갤 수 있는 가장 단위로 쪼갠 뒤, 정렬된 하나의 새로운 배열을 반환하는 함수를 재귀적으로
- nlogn의 시간 복잡도로 삽입정렬보다는 성능이 우수하지만,
  정렬할 요소가 30개 이하일 때는 삽입정렬과 속도가 비슷하고, 메모리를 더 차지하기 때문에 비효율적이다.

  | 공간복잡도 | 시간복잡도 |
  | :--------: | :--------: |
  |    O(n)    |  O(nlogn)  |

### 코드

```js
function mergeSorting(arr) {
  if (arr.length === 1) return arr;
  const center = Math.ceil(arr.length / 2);
  const left = arr.slice(0, center);
  const right = arr.slice(center);
  return merge(mergeSorting(left), mergeSorting(right));
}

function merge(left, right) {
  const sorted = [];
  while (left.length && right.length) {
    if (left[0] <= right[0]) sorted.push(left.shift());
    else sorted.push(right.shift());
  }
  while (left.length) sorted.push(left.shift());
  while (right.length) sorted.push(right.shift());
  return sorted;
}
```
