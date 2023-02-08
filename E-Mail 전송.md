##### toString과 String.valueOf()
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

1. 의존성 추가
```
dependencies {
    // javamail
    implementation 'org.springframework.boot:spring-boot-starter-mail'
}
```

2. Gmail SMTP Server 설정
- 구글 계정만 있으면 무료로 발송할 수 있는 Gmail SMTP Server 활용
- 구글 SMTP 활용하여 application.yml 기본 설정
```
application:
  mail:
    host: smtp.gmail.com
    username: user@gmail.com
    password: passwords
    port: 465
    socketFactory-class: javax.net.ssl.SSLSocketFactory
    supplier: gmail.com
    from-mail: user@gmail.com
```

3. 퍼블리싱
```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>메일 발송</title>
</head>
<body>
    <h1>메일 발송</h1>

    <form th:action="@{/mail}" method="post">
        <input name="address" placeholder="이메일 주소"> <br>
        <input name="title" placeholder="제목"> <br>
        <textarea name="message" placeholder="메일 내용을 입력해주세요." cols="60" rows="20"></textarea>
        <button>발송</button>
    </form>
</body>
</html>
```
4. Controller
```
@Controller
@AllArgsConstructor
public class MailController {
    private final MailService mailService;

    @GetMapping("/mail")
    public String dispMail() {
        return "mail";
    }

    @PostMapping("/mail")
    public void execMail(MailDto mailDto) {
        mailService.mailSend(mailDto);
    }
}
```

5. Dto
```
@Getter
@Setter
@NoArgsConstructor
public class MailDto {
    private String address;
    private String title;
    private String message;
}
```

6-1. Service (텍스트로만 이루어진 메일 발송)
```
@Service
@AllArgsConstructor
public class MailService {
    private JavaMailSender mailSender;
    private static final String FROM_ADDRESS = "aaaa@bbbb.com";

    public void mailSend(MailDto mailDto) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(mailDto.getAddress());          // 받는 사람의 메일주소
        message.setFrom(MailService.FROM_ADDRESS);    // 보내는 사람의 메일주소
        message.setSubject(mailDto.getTitle());       // 메일 제목
        message.setText(mailDto.getMessage());        // 내용

        mailSender.send(message);
    }
}
```
  - JavaMailSender에서 MailSender 인터페이스를 상속, MIME을 지원
  - MIME 이란, Multipuropose Internet Mail Extensions의 약자로 여러 형태의 파일을 텍스트 문자로 변환해서 이메일 시스템을 통해 전달


6-2. Service (이미지, 첨부파일 등을 포함한 메일 발송)
```
@Service
@AllArgsConstructor
public class MailService {
    private JavaMailSender mailSender;
    private static final String FROM_ADDRESS = "YOUR_EMAIL_ADDRESS";

    public void mailSend(MailDto mailDto) {
        try {
            MailHandler mailHandler = new MailHandler(mailSender);
            
            // 받는 사람
           mailHandler.setTo(mailDto.getAddress());
            // 보내는 사람
           mailHandler.setFrom(MailService.FROM_ADDRESS);
            // 제목
           mailHandler.setSubject(mailDto.getTitle());
            // HTML Layout
            String htmlContent = "<p>" + mailDto.getMessage() +"<p> <img src='cid:sample-img'>";
            mailHandler.setText(htmlContent, true);
            // 첨부 파일
           mailHandler.setAttach("newTest.txt", "static/originTest.txt");
            // 이미지 삽입
           mailHandler.setInline("sample-img", "static/sample1.jpg");

            mailHandler.send();
        }
        catch(Exception e){
            e.printStackTrace();
        }
    }
}
```
  - 텍스트 메일과 다른 점은 이메일 메시지 작성/발송 부분 모두 MailHandler를 통해 처리
  - 첨부 파일 및 이미지 파일은 static 경로 아래에 존재
  > e.printStackTrace : 에러의 발생 근원지를 찾아 단계별로 에러를 출력
  > 
  > 예외 발생 당시의 호출 스택(Call Stack)에 있던 메소드의 정보와 예외 결과를 화면에 출력 (예외 상황을 분석하기 위한 용도로 사용)
  > 이외에 e.getMessage(): 에러의 원인을 간단하게 출력 / e.toString() : 에러의 Exception 내용과 원인을 출력 등이 있음

  > e.printStackTrace(); 의 단점
    - printStackTrace를 call 할 경우, system.err로 쓰여져서 제어하기 힘듬
    - java 리플렉션을 사용하여 추적하는 것이라서 많은 오버헤드 발생 가능
    - 서버에서 스택정보를 취합하기 때문에 서버에 부하 발생 가능
    - 출력이 어디로 가는지 하악하기가 어려움

7. MailHandler
```
public class MailHandler {
    private JavaMailSender sender;
    private MimeMessage message;
    private MimeMessageHelper messageHelper;

    // 생성자
    public MailHandler(JavaMailSender jSender) throws
            MessagingException {
        this.sender = jSender;
        message = jSender.createMimeMessage();
        messageHelper = new MimeMessageHelper(message, true, "UTF-8");
    }

    // 보내는 사람 이메일
    public void setFrom(String fromAddress) throws MessagingException {
        messageHelper.setFrom(fromAddress);
    }

    // 받는 사람 이메일
    public void setTo(String email) throws MessagingException {
        messageHelper.setTo(email);
    }

    // 제목
    public void setSubject(String subject) throws MessagingException {
        messageHelper.setSubject(subject);
    }

    // 메일 내용
    public void setText(String text, boolean useHtml) throws MessagingException {
        messageHelper.setText(text, useHtml);
    }

    // 첨부 파일
    public void setAttach(String displayFileName, String pathToAttachment) throws MessagingException, IOException {
        File file = new ClassPathResource(pathToAttachment).getFile();
        FileSystemResource fsr = new FileSystemResource(file);

        messageHelper.addAttachment(displayFileName, fsr);
    }

    // 이미지 삽입
    public void setInline(String contentId, String pathToInline) throws MessagingException, IOException {
        File file = new ClassPathResource(pathToInline).getFile();
        FileSystemResource fsr = new FileSystemResource(file);

        messageHelper.addInline(contentId, fsr);
    }

    // 발송
    public void send() {
        try {
            sender.send(message);
        }catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

  > MimeMessageHelper
  - 스프링에서 제공하는 헬퍼 객체이며, HTML 레이아웃, 이미지 삽입, 첨부파일 등 MIME 메시지를 생성할 수 있음
  - messagaHelper.setText : 메일 내용, 두 번째 파라미터에 HTML 사용여부를 전달
  - messageHelper.addAttachment : 첨부파일, 파일 이름과 파일 경로 작성
  - messageHelper.addInline : 이미지 삽입, 이미지id Attribute명과 파일 경로 작성
  - 기존 (setTo 받는 사람 주소, setFrom 보내는 사람 주소, setSubject 제목, setText 내용 등 기본적인 메소드도 지원)
