# lwcFxJs
함수형 프로그래밍 라이브러리

## 참고
fxjs(https://github.com/marpple/FxJS)

## 적용배경
- 대표적인 함수형 프로그래밍 라이브러리 lodash 혹은 underscore 대비 Promise를 잘 지원함.
- Promise를 잘 지원하기때문에 async/await 문법을 함께 사용가능.
- es6 표준 이터러블 프로토콜을 따르고 있어 이터러블/이터레이터/제너레이터와 잘 조합됨
- 엄격/지연/병렬 동시평가를 간결하게 작성가능.(엄격/지연/병렬 등은 밑의 설명참고)
- 결정적으로 for문이 2 depth 이상깊어진다면, 유지보수가 쉽지 않아진다.(이해가 쉽지않다.)

## 함수형 프로그래밍 간단 정리.

### 함수의 기본 특징.

 - 일급
   - 값으로 다룰 수 있다.
   - 변수에 담을 수 있다.
   - 함수의 인자로 사용될 수 있다.
   - 함수의 결과로 사용될 수 있다.
 
- 일급함수
   - 함수를 값으로 다룰 수 있다.
   - 조합성과 추상화의 도구.
   - 함수가 일급이라는 이야기는 함수를 값으로 다룰 수 있다는 말.
   - 함수를 값으로 다룰수 있다는 이야기는
   - const add5 = a => a + 5; 변수에 함수를 값으로 다뤄서 담을 수 있다는 뜻.
   - 함수의 결과값으로 함수가 사용될 수 있다.
   
- 고차 함수
   - 함수를 값으로 다루는 함수.
   - 함수를 인자로 받아서 실행하는 함수

### 평가
 - 코드가 계산(Evulation) 되어 값을 만드는 것

### 작성준비중

### 그외
- JS 제네레이터 & 이터러블 프로토콜을 이용한 반복처리에 대한 새로운 시점
- 위의 방법을 통한 사이드이펙트가 없는 함수형 프로그래밍 방법
- JS Promise의 이해와 그를 이용하여 지연성 & 동시성 처리방법
- 함수의 안전한 합성
- 명령형 코드 작성 습관 지우기

### 엄격/지연/병렬 예제

- Basic

```javascript
_.go(
    [1, 2, 3, 4, 5, 6],
    map((a) => a * a),
    filter((a) => a % 2),
    take(6),
);
```

![스크린샷 2022-07-08 오후 5 35 40](https://user-images.githubusercontent.com/17538535/177953601-f7f76946-65bb-44df-8ca5-11e9617c2e87.png)

- Lazy Evaluation

```javascript
_.go(
    L.range(Infinity), // 최대로 일어날일
    L.map((i) => track[i]), // 여기에서 해당하는 데이터의 구조를 맞춰주는 일
    L.map(({ cars }) => cars), //여기에서 해당하는 데이터의 구조를 맞춰주는 일
    L.map(_.delay(2000)), // 얼마나 대기를 하겠다.
    //L.takeWhile(({ length: l }) => l === 4), //어떤 조건일떄가지만 이 일을 일어나게하겠다.
    L.takeUntil(({ length: l }) => l < 4), //어떤 조건일떄가지만 이 일을 일어나게하겠다.
    L.flat, // 자료구조를 펼쳐주고
    L.map((car) => `${car} 출발~`), // 데이터에 변형을 주고
    _.each(_.log) //  효과를 일으킨다.
);
```
![스크린샷 2022-07-08 오후 5 36 33](https://user-images.githubusercontent.com/17538535/177953663-091ecb89-6c76-4fb7-97c9-66be52f3e5da.png)


- Concurent

```javascript
const delay500 = (a, name) => {
    console.log('delay500 : ', a);
    return new Promise((resolve) => {
        setTimeout(() => resolve(a), 500);
    });
};

go(
    [1, 2, 3, 4, 5, 6, 7, 8],
    C.map((a) => delay500(a * a, 'map 1')),
    C.filter((a) => delay500(a % 2, 'filter 2')),
    C.map((a) => delay500(a + 1, 'map 3')),
    C.take(2),
    log,
);
```
![스크린샷 2022-07-08 오후 5 46 33](https://user-images.githubusercontent.com/17538535/177954931-7cfa9001-fd52-454f-9b53-384351b293cc.png)

