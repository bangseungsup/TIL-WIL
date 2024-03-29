### Synchronous 동기
직렬적으로 태스크를 수행하는 방식이다.
- 요청을 보낸 후 응답을 받아야지만 다음 동작이 이루어지는 방식이다. 어떠한 태스크를 처리할 동안 나머지 태스크는 대기한다.
- 실제로 cpu가 느려지는 것은 아니지만, 시스템의 전체적인 효율이 저하될 수도 있다.

![image](https://user-images.githubusercontent.com/118147296/220805812-c7df7cee-d359-4728-be8a-c5b690f5dc8d.png)
이미지 출처: [동기식 처리 모델(synchronous processing model)](https://poiemaweb.com/es6-promise)

### Asynchronous 비동기
병렬적으로 태스크를 수행하는 방식이다.
- 요청을 보낸 후 응답의 수락 여부와 상관없이 다음 태스크가 동작하는 방식이다.
- a 태스크가 실행되는 시간동안 b 태스크를 할 수 있으므로 자원을 효율적으로 사용할 수 있다.
- 비동기 요청 시 응답 후 처리할 '콜백 함수'를 함께 알려준다. 따라서 해당 태스크가 완료되었을 때, '콜백 함수'가 호출된다.

![image](https://user-images.githubusercontent.com/118147296/220805950-fe3979e3-8389-48f1-97ef-6a8d340ab886.png)
이미지 출처: [비동기식 처리 모델(Asynchronous processing model)](https://poiemaweb.com/es6-promise)

##### 비동기적 코드의 실행 결과는 동기적 코드가 전부 실행되고나서 값을 반환한다.
동기는 디자인이 비동기보다 간단하고 직관적일 수 있지만 결과가 주어질 때까지 아무것도 하지 못하고 대기해야만 하는 문제가 있다.

비동기는 동기보다 복잡하지만 결과가 주어지는데 시간이 걸려도 그 시간동안 다른 작업을 할 수 있어서 보다 효율적일 수 있다.

> 하지만 비동기 처리를 위해 콜백 패턴을 사용하면 처리 순서를 보장하기 위해 여러 개의 콜백 함수가 중첩되어 복잡도가 높아지는 **콜백 헬(Callback Hell)** 이 발생하는 단점 발생할 수 있습니다. 콜백 헬은 가독성을 나쁘게 하며 실수를 유발하는 원인이 된다.

```
step1(function(value1) {
  step2(value1, function(value2) {
    step3(value2, function(value3) {
      step4(value3, function(value4) {
        step5(value4, function(value5) {
            // value5를 사용하는 처리
        });
      });
    });
  });
});
```
