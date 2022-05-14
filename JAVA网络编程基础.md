---
title: JAVA网络编程基础
tags:
  - java
  - network
  - socket
abbrlink: 9f68f09f
date: 2022-03-08 16:09:30
---

这篇文章主要用来总结Java在网络编程中的知识点

下面是一个Java客户端与服务端通信的样例程序

```java
//Server
public class Server {
    public static void main(String[] args) throws IOException {
        ServerSocket ss = new ServerSocket(6666);
        System.out.println("Server start listening...");
        for (; ; ) {
            Socket sock = ss.accept();
            System.out.println("Receviced package from " + sock.getRemoteSocketAddress());
            Thread t = new Handler(sock);
            t.start();
        }
    }
}

class Handler extends Thread {
    Socket sock;

    public Handler(Socket sock) {
        this.sock = sock;
    }

    @Override
    public void run() {
        try (InputStream input = this.sock.getInputStream()) {
            try (OutputStream output = this.sock.getOutputStream()) {
                handle(input, output);
            }
        } catch (Exception e) {
            try {
                this.sock.close();
            } catch (IOException ioe) {
            }
            System.out.println("Client disconnected.");
        }
    }

    private void handle(InputStream input, OutputStream output) throws IOException {
        var writer = new BufferedWriter(new OutputStreamWriter(output, StandardCharsets.UTF_8));
        var reader = new BufferedReader(new InputStreamReader(input, StandardCharsets.UTF_8));
        writer.write("hello\n");
        writer.flush();
        for (; ; ) {
            String s = reader.readLine();
            if (s.equals("bye")) {
                writer.write("bye\n");
                writer.flush();
                break;
            }
            writer.write("ok: " + s + "\n");
            writer.flush();
        }
    }
}
```

```java
//client
public class client {
    public static void main(String[] args) throws IOException {
        Socket sock = new Socket("localhost", 6666);
        try (InputStream input = sock.getInputStream()){
            try(OutputStream output = sock.getOutputStream()){
                handle(input, output);
            }
        }
    }
    private static void handle(InputStream input, OutputStream output) throws IOException{
        var writer = new BufferedWriter(new OutputStreamWriter(output, StandardCharsets.UTF_8));
        var reader = new BufferedReader(new InputStreamReader(input, StandardCharsets.UTF_8));
        Scanner scanner = new Scanner(System.in);
        System.out.println("[server]: "+ reader.readLine());
        for(;;){
            System.out.print(">>>");
            String s = scanner.nextLine();
            writer.write(s);
            writer.newLine();
            writer.flush();
            String resp = reader.readLine();
            System.out.println("<<<" + resp);
            if(resp.equals("bye"))  break;
        }
    }
}
```

实现功能为服务端与客户端相互通信，收到`bye`信号后断开连接，如图所示

![java_net_example1_result](https://image.0error.net/img/2022/20220308161211.png)
