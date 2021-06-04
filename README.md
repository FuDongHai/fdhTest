# fdhTest

```java
@Test
    public void repoCase1() {
        ConnectionProvider connectionProvider = ConnectionProvider.create("es", 10);
        HttpClient httpClient = HttpClient.create(connectionProvider)
                .proxy(proxy -> proxy.type(ProxyProvider.Proxy.HTTP).address(new InetSocketAddress("ip", 18888)))
                .option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 20000);

        httpClient.get()
                .uri("http://host/....")
                .responseSingle((r, b) -> b.asString(StandardCharsets.UTF_8).map(s -> {
                    return s;
                }))
                .block();
    }
```
