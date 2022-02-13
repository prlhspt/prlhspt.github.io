---
title: SSH Tunneling 으로 Remote DB에 접속하기
date: 2022-02-13 14:23:00 +0900
category: java
---


<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [소개](#소개)
- [사용법](#사용법)

<!-- /code_chunk_output -->

# 소개

클라우드 하둡을 사용하면서, edgenode의 ssh 포트만 개방되어 있는데, 테스트를 위해 datenode에 db에 접속을 시도해야 하는 일이 생겼습니다.
이번 일을 해결하기 위해 jsch의 SSH Tunneling 기능을 사용해서 접속 후 테스트를 진행하였는데, SSH Tunneling을 어떻게 사용했는지 공유합니다.

# 사용법

우선 터널링을 진행하는 유틸 클래스를 만들어 주었습니다.

```java
public class SSHTunneling {
    static int lport;
    static String rhost;
    static int rport;

    public void sshConnect(){
        String user = USER;
        String password = PASWORD;
        String host = HOST;
        int port = PORT;
        try
        {
            JSch jsch = new JSch();
            Session session = jsch.getSession(user, host, port);

            // 로컬에 어떤 포트로 개방 할 지
            lport = LPORT;

            // 원격지에 어떤 호스트를 로컬로 터널링 할 지
            rhost = RHOST;

            // 원격지에 어떤 포트를 로컬로 터널링 할 지
            rport = RPORT;
            session.setPassword(password);
            session.setConfig("StrictHostKeyChecking", "no");
            System.out.println("Establishing Connection...");
            session.connect();
            int assinged_port=session.setPortForwardingL(lport, rhost, rport);
            System.out.println("localhost:"+assinged_port+" -> "+rhost+":"+rport);
        }
        catch(Exception e){
            System.err.print(e);
            }
    }
}
```

그 후 로직을 실행할 main에서 터널링 클래스를 호출하여 터널링을 진행하였습니다.

```java
    public static void main(String[] args) {

        SSHTunneling sshTunneling = new SSHTunneling();
        sshTunneling.sshConnect();

  ...

```

그 후 설정했던 localhost:lport 번으로 DB 접속을 연결하였습니다.

```java
public class DBConst {
    public static final String constSTRHiveJdbcPublicURI = "jdbc:hive2://localhost:LPORT";
...

```
