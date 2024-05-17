---
title: JSP/Servlet으로 Gmail을 이용하여 메일 보내기
date: 2012-02-12T04:56:27+09:00
categories:
  - Java/JSP
tags:
  - gmail
  - Java/JSP
  - JSP
  - Servlet
  - 메일보내기
  - 서블릿
  - 자바
  - 지메일
---
사용자가 적은 내용을 내 메일계정으로 보내도록 만들려고 여기저기 찾아본 끝에 결국 완료했다.

필요한 라이브러리는 Javabeans Activation Framework (JAF)와 JavaMail 이 두가지이다.

  * JAF 라이브러리 다운로드 : <http://www.oracle.com/technetwork/java/jaf11-139815.html>
  * JavaMail 라이브러리 다운로드 : <http://www.oracle.com/technetwork/java/index-138643.html>

두 라이브러리를 다운 받아 적당한 폴더에 넣는다. 내 경우에는 이클립스에서 `프로젝트폴더/WebContent/WEB-INF/lib` 에 압축을 푼 JAR 파일들을 복사해넣었다.

두 라이브러리를 설치했다면 코드를 작성한다. 내가 작성한 코드는 JSP에서 form을 이용해 이름, 이메일주소, 제목, 내용을 입력받고 이 내용을 서블릿으로 보내고 서블릿에서 파라미터로 받은 내용을 메일로 보낸 다음, 처리 결과에 따라 적절한 결과 페이지로 보내게 되어있는 구조이다.

JSP의 form 입력양식 페이지는 어려울게 없다. 문제는 서블릿 부분이었다.

인터넷을 찾아보니 소스가 많이 있긴한데 대부분은 로컬 메일서버를 이용하는 방법들이었고 내 서버 역시 SendMail이 있긴하지만 굳이 그걸 쓸 필요도 없고 지메일쪽의 SMTP를 쓰는게 더 나을 것 같아서 방법을 찾아보았다. 지메일쪽은 SSL을 이용한 인증을 하고 있어서 이것저것 옵션을 붙여야할 것들이 많았다.

`SendMail.java` 서블릿의 코드는 다음과 같다.

```java
package mailer;

import java.io.IOException;
import java.util.Properties;

import javax.mail.Authenticator;
import javax.mail.Message;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class SendMail
 */
public class SendMail extends HttpServlet {
  private static final long serialVersionUID = 1L;

    /**
     * @see HttpServlet#HttpServlet()
     */
    public SendMail() {
        super();
        // TODO Auto-generated constructor stub
    }

  /**
   * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
   */
  protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // TODO Auto-generated method stub
  }

  /**
   * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
   */
  protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // TODO Auto-generated method stub

    request.setCharacterEncoding("UTF-8");

    response.setContentType("text/html;charset=UTF-8");
    response.setCharacterEncoding("UTF-8");

    String m_name =		request.getParameter("name");
    String m_email =	request.getParameter("email");
    String m_title =	request.getParameter("title");
    String m_text =		request.getParameter("text");

    try {
      String mail_from =	m_name + "<" + m_email + ">";
      String mail_to =	"불법스팸대응센터<118@kisa.or.kr>";
      String title =		"테스트 이메일입니다. :: " + m_title;
      String contents =	"보낸 사람 :: " + m_name + "<" + m_email + "><br><br>" + m_title + "<br><br>" + m_text;

      mail_from = new String(mail_from.getBytes("UTF-8"), "UTF-8");
      mail_to = new String(mail_to.getBytes("UTF-8"), "UTF-8");

      Properties props = new Properties();
      props.put("mail.transport.protocol", "smtp");
      props.put("mail.smtp.host", "smtp.gmail.com");
      props.put("mail.smtp.port", "465");
      props.put("mail.smtp.starttls.enable", "true");
      props.put("mail.smtp.socketFactory.port", "465");
      props.put("mail.smtp.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
      props.put("mail.smtp.socketFactory.fallback", "false");
      props.put("mail.smtp.auth", "true");

      Authenticator auth = new SMTPAuthenticator();

      Session sess = Session.getDefaultInstance(props, auth);

      MimeMessage msg = new MimeMessage(sess);

      msg.setFrom(new InternetAddress(mail_from));
      msg.setRecipient(Message.RecipientType.TO, new InternetAddress(mail_to));
      msg.setSubject(title, "UTF-8");
      msg.setContent(contents, "text/html; charset=UTF-8");
      msg.setHeader("Content-type", "text/html; charset=UTF-8");

      Transport.send(msg);

      response.sendRedirect("request_complete.jsp");
    } catch (Exception e) {
      response.sendRedirect("request_failed.jsp");
    } finally {

    }
  }

}
```

중요한건 인증에 대한 부분인데 Session 객체에 이 인증에 대한 객체를 넣어줘야한다. 이 인증 객체는 내가 참조한 다른 글이나 구글링을 해서 찾은 외국문서들에도 모두다 하나의 클래스로 구현하고 있었다. 굳이 왜 그래야하는지는 잘 모르겠지만 어쨌튼 나도 따로 클래스 하나로 구현했다.

`SMTPAuthenticator.java`의 코드이다.

```java
/**
 *
 */
package mailer;

import javax.mail.PasswordAuthentication;
import javax.mail.Authenticator;

/**
 * @author viper9
 *
 */
public class SMTPAuthenticator extends Authenticator {
  public SMTPAuthenticator() {
    super();
  }

  public PasswordAuthentication getPasswordAuthentication() {
    String username = "지메일주소";
    String password = "지메일암호";
    return new PasswordAuthentication(username, password);
  }
}
```

Authenticator를 상속받는 이러한 인증객체를 따로 구현한 다음 위 서블릿 클래스와 같은 디렉토리에 넣는다.  
이 서블릿 클래스에 파라미터로 name, email, title, text 파라미터를 주면 설정된 메일 주소로 메일을 보내게 된다. 받는 사람도 따로 받고 싶다면 역시 파라미터로 받으면 그만이다.

난 서버의 거의 모든 부분에서 UTF-8 인코딩만 쓰고 있고 MySQL도 UTF-8을 기준으로 구성되어있으며 자바 작업을 할 때도 UTF-8을 선호하는 편이라 입력되는 파라미터들을 모두 UTF-8로 처리해주며 작업했다.

작업하며 어려웠던건 인증객체를 생성할 때 사용하는 SMTPAuthenticator 클래스를 만들 때였는데 이게 이클립스의 자동완성을 믿고 하다가 스펠링이 몇개 틀려서 한참 애먹었다. 내가 참조하던 그 문서도 아마 이 스펠링이 틀렸던 것으로 기억한다.Authentication와 Authenticator 이 스펠링이 틀리면 자바가 계속 암호가 틀린거 아니냐고 에러를 출력한다. 굉장히 짜증나던 부분.

여튼 한참 디버깅 끝에 오류를 잡고 나서 작동해보니 메일은 잘 오는데 몇가지 문제점이 있었다.

  1. 메일을 보내는 속도가 빠르지 않다. 외국서버라 그런지 메일을 보내고 나서 2~3초 후에 결과가 수신되는 것 같다. form의 submit 버튼을 사용자가 연속으로 누를 가능성이 있어서 이 부분을 조치를 취해야할 것 같다.
  2. 메일이 바로바로 오지 않는다. 바로바로 올때도 있는데 안 그럴 때도 꽤 많다. 메일을 보내고 나서 몇초 후에 도착하는 일이 꽤 많았다.
  3. 이걸 이용해서 메일을 받아보면 보낸 사람이 SMTPAuthenticator 클래스에서 설정한 그 지메일주소로 설정된다. 내가 따로 지정해주고 싶은데 아무리 해도 되질 않았다. Properties에서 설정이 가능할 것 같기도한데 찾지 못했다. (혹시 아시는 분은 답글 좀 부탁드립니다.)

위 3번 문제 때문에 이걸 이용해서 메일을 보내면 보낸 사람 정보를 알수 없어서 아예 메일 본문에다가 보낸 사람의 이름과 메일주소까지 넣도록 서블릿 클래스에 추가해야했다.  
~~메일러의 메일주소를 따로 부여하기 위해 noreply@dongbumkim.com으로 메일 주소를 하나 만들어줬다.~~ (지메일은 만들고 한번 로그인해야 비로고 SMTP 서버를 사용가능하다.)

이로써 사용자요청사항을 받는 페이지는 적절히 구현되었다. 이걸 정리해서 아예 메일 송신 전용 클래스로 만들어놔야겠다.

**2014년 5월 8일 추가.**

이 소스를 이용해 테스트를 하는 사람이 너무 많아서 나에게 너무 많은 스팸메일이 왔다. 소스 안에 메일 주소를 불법스팸대응신고센터 이메일 주소로 변경해두었으니 이 소스를 사용하는 사람은 받는 사람의 메일 주소를 확인하고 수정해서 쓰면 되겠다.
