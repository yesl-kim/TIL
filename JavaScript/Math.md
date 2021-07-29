## 랜덤함수
### min <= x <= max (무작위 정수)
> Math.floor(Math.random() * (max - min + 1)) + min;

### min <= x < max (무작위 정수)
> Math.floor(Math.random() * (max - min)) + min;

---

* Math.random() 함수는 0~1의 실수를 생성하며 1을 생성하지는 않는다, Math.round가 아닌 Math.floor를 사용하는 것은 더 균일한 랜덤 값 생성을 하기 위한 것이다. Math.round를 사용하게 되면, 0과 1이 나올 수 있는 확률이 절반으로 줄어들게 된다.

출처: https://unikys.tistory.com/281 [All-round programmer]
