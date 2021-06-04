# repoCase1()，repoCase2()，repoCase3() the proxy IP, port, and URI are all the same，but only repoCase1() throws a 403 exception
## example：

```java
    @Test
    public void repoCase1() {
        ConnectionProvider connectionProvider = ConnectionProvider.create("es", 10);
        HttpClient httpClient = HttpClient.create(connectionProvider)
                .proxy(proxy -> proxy.type(ProxyProvider.Proxy.HTTP).address(new InetSocketAddress("47.243.12.14", 18888)))
                .option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 20000);

        httpClient.get()
                .uri("http://www.baidu.com")
                .responseSingle((r, b) -> b.asString(StandardCharsets.UTF_8).map(s -> {
                    return s;
                }))
                .block();
    }
```

# exception：

```
reactor.core.Exceptions$ReactiveException: io.netty.handler.proxy.HttpProxyHandler$HttpProxyConnectException: http, none, /ip:18888 => host:80, status: 403 Access violation
```

----------------------------------------------------------------------------------------------------------

```java
    @Test
    public void repoCase2() {
        ConnectionProvider connectionProvider = ConnectionProvider.create("es", 10);
        HttpClient httpClient = HttpClient.create(connectionProvider)
                .proxy(proxy -> proxy.type(ProxyProvider.Proxy.HTTP).address(new InetSocketAddress("47.243.12.14", 18888)))
                .option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 20000);

        httpClient.get()
                .uri("https://www.baidu.com")
                .responseSingle((r, b) -> b.asString(StandardCharsets.UTF_8).map(s -> {
                    return s;
                }))
                .block();
    }
```

# I don't know why I would report 403 when using reactor netty to access the address of the HTTP protocol? For your help, thank you

* Reactor version(s) used: 3.4.3
* Other relevant libraries versions (eg. `netty`, ...): netty 4.1.59.Final
* JVM version (`java -version`): 1.8.0_231
* OS and version (eg. `uname -a`):
