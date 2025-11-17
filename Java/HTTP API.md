- `java.net.http API was introduced in Java 11
- Replaced the old `HttpURLConnection`
-  `HttpClient` > `HttpURLConnection`
	- easier to work with
	- supporting synchronous and asynchronous calls

## Basic Classes
- **HttpClient**: the main client object used to send HTTP requests.
- **HttpRequest**: represents the request (method, URI, headers, body).
- **HttpResponse**: represents the response (status code, headers, body).
	- **BodyHandlers**: specify how to handle the response body (as string, byte array, file, etc.).

## HttpClient
> [!example]
> Basic HttpClient
> ```java
> import java.net.http.HttpClient;
> 
> HttpClient client = HttpClient.newHttpClient();
> ```
> Optional configuration
> ```java
> HttpClient client = HttpClient.newBuilder()
>     .version(HttpClient.Version.HTTP_2)
>     .followRedirects(HttpClient.Redirect.NORMAL)
>     .connectTimeout(Duration.ofSeconds(10))
>     .proxy(ProxySelector.of(new InetSocketAddress("proxy.example.com", 8080)))
>     .authenticator(Authenticator.getDefault())
>     .build();
> ```

- Version
	- `.version(HttpClient.Version.HTTP_2)`
	- Specifies the **HTTP protocol version** to use. Options:
		- - `HTTP_1_1` — older, widely supported
		- `HTTP_2` — newer, allows multiplexing multiple requests over one connection (faster)

