#HTTP客户端API

1. 2015年HTTP/2协议成为标准，允许服务器“push”数据，可以发送比客户端请求更多的数据。可以优先处理并发送加载网页所需的数据。
2. JAVA9引入HTTP CLIENT API，支持同步和异步，在JAVA11中正式可用，存在于java.net包下，替代HttpURLConnection，并提供WebSocket和HTTP/2的支持。
3. 同步方式
    ```
   HttpClient client = HttpClient.newHttpClient();
   HttpRequest request = HttpRequest.newBuilder(URI.create("")).build();
   HttpResponse.BodyHandler<String> handler = HttpResponse.BodyHandler.ofString();
   HttpResponse<String> response = client.send(request,handler);
   String body = response.body();
   System.out.println(body);
   ```
4. 异步方式
    ```
   HttpClient client = HttpClient.newHttpClient();
   HttpRequest request = HttpRequest.newBuilder(URI.create("")).build();
   HttpResponse.BodyHandler<String> handler = HttpResponse.BodyHandler.ofString();
   CompletableFuture<HttpResponse<String>> sendAsync = client.sendAsync(request,handler);
   sendAsync.thenApply(t -> t.body()).thenAccept(System.out::println);
   ```