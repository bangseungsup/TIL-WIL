### 인코딩
정보의 형태나 형식을 표준화, 보안, 처리속도 향상, 저장 공간 절약 등을 위해서 다른 형태나 형식으로 변환 처리 또는 처리 방식을 말한다.

### Base64 인코딩
Base64란 Binary Data를 Text로 바꾸는 인코딩의 하나로, Binary Data를 Character set에 영향을 받지 않는 공통 ASCII 영역의 문자로만 이루어진 문자열로 바꾸는 인코딩이다.
Base64를 글자 그대로 직역하면 64진법이라는 뜻이다. 64진법은 컴퓨터한테 특별한데 그 이유는 64가 2의 제곱수 64=2^6이며 2의 제곱수에 기반한 진법 중 화면에 표시되는 ASCII 문자들로 표시할 수 있는 가장 큰 진법이기 때문이다. 
(ASCII에는 제어문자가 다수 포함되어 있기 때문에 화면에 표시되는 ASCII 문자는 128개가 되지 않는다.)

핵심은 **Base64 인코딩은 Binary Data를 Text로 변경하는 인코딩**이다.

변경하는 방식을 간략하게 설명하면 Bianry Data를 6bit씩 자른 뒤 6bit에 해당하는 문자를 아래 Base64 색인표에서 찾아 치환한다. (실제로는 Padding을 더해주는 과정이 추가된다.)

![image](https://user-images.githubusercontent.com/118147296/220522282-fda49c9c-5211-4fa8-a7ca-88a0d21b58c8.png)
![image](https://user-images.githubusercontent.com/118147296/220522317-82d13a40-35a3-422a-9e26-b6b38b10a068.png)

![image](https://user-images.githubusercontent.com/118147296/220522337-a069dcf3-aeb0-4db8-a02a-7f4efa1f577c.png)

### Base64 인코딩을 쓰는 이유
Base64 Encoding을 하게되면 전송해야 될 데이터의 양도 약 33% 정도 늘어난다. 6bit당 2bit의 Overhead가 발생하기 때문이다. 
인코딩 전 대비 33%나 데이터의 크기가 증가하고, 인코딩 및 디코딩에 추가 CPU 연산까지 필요한데 우리는 왜 Base64 인코딩을 하는 이유는 
문자를 전송하기 위해 설계된 Media(Email, HTML)를 이용해 플랫폼 독립적으로 Binary Data(이미지나 오디오)를 전송 할 필요가 있을 때, ASCII로 Encoding하여 전송하게 되면 여러가지 문제가 발생할 수 있다. 

> 문제점
> - ASCII는 7 bits Encoding인데 나머지 1bit를 처리하는 방식이 시스템 별로 상이하다.
> - 일부 제어문자 (e.g. Line ending)의 경우 시스템 별로 다른 코드값을 갖는다.

위와 같은 문제로 ASCII는 시스템간 데이터를 전달하기에 안전하지가 않다. Base64는 ASCII 중 제어문자와 일부 특수문자를 제외한 64개의 안전한 출력 문자만 사용한다.

**Base64는 HTML 또는 Email과 같이 문자를 위한 Media에 Binary Data를 포함해야 될 필요가 있을 때, 포함된 Binary Data가 시스템 독립적으로 동일하게 전송 또는 저장되는걸 보장하기 위해 사용한다.**
