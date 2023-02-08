#### toString과 String.valueOf()
- 두 메소드는 Object 값을 String 형으로 변환할 때 주로 사용하는 메소드
- String 형태로 값을 변환해주는 비슷한 점이 있지만, 변경하고자 하는 값이 null이라면 차이가 존재
- null 값에 따른 NullPointerException 발생 유무에 차이가 있음

  - toString() : null 값을 형 변환 시 NullPointerException 발생, Object 값이 String이 아니여도 출력
  - String.valueOf() : 파리미터로 null이 오면 "null"이라는 문자열 출력

valueOf 예시
```
String ePw = "";
        for(int i = 0; i < 6 ; i++) {
            char a = (char) (Math.random() * 42 + 49);
            if (a >= 58 && a <= 64) {
                i--;
            } else {
                ePw += String.valueOf(a);
            }
        }
```

#### JavaMailSender
MailSender 인터페이스를 상속 받은 JavaMailSender를 사용


