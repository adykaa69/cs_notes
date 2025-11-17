[Baeldung - Exploring the New HTTP Client in Java](https://www.baeldung.com/java-9-http-client)
- `java.net.http.HttpClient` API was introduced in Java 11
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
[Class HttpClient](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/HttpClient.html)
> [!example]
> **Basic HttpClient**
> ```java
> import java.net.http.HttpClient;
> 
> HttpClient client = HttpClient.newHttpClient();
> ```
> **Optional configuration**
> ```java
> HttpClient client = HttpClient.newBuilder()
>     .version(HttpClient.Version.HTTP_2)
>     .followRedirects(HttpClient.Redirect.NORMAL)
>     .connectTimeout(Duration.ofSeconds(10))
>     .proxy(ProxySelector.of(new InetSocketAddress("proxy.example.com", 8080)))
>     .authenticator(Authenticator.getDefault())
>     .build();
> ```
>> [!example]- **Version**
>> - `.version(HttpClient.Version.HTTP_2)`
>> - Specifies the **HTTP protocol version** to use.
>> - Options:
>> 	- `HTTP_1_1` — older, widely supported
>> 	- `HTTP_2` — newer, allows multiplexing multiple requests over one connection (faster)
>> - If server doesn’t support HTTP/2, Java **falls back to HTTP/1.1** automatically.
>
>>[!example]- **Redirects Handling**
>> - `.followRedirects(HttpClient.Redirect.NORMAL)`
>> - Controls how the client handles **3xx redirects**.
>> - Options:
>> 	- `NEVER` — do not follow redirects (default)
>> 	- `NORMAL` — automatically follow **GET** and **HEAD** redirects
>> 	- `ALWAYS` — follow all redirects, including POST
>
>> [!example]- **Connection Timeout**
>> - `.connectTimeout(Duration.ofSeconds(10))`
>> - Sets the **maximum time the client will wait** to establish a connection.
>> - Avoids hanging requests.
>
>> [!example]- **Proxy Settings**
>> - `.proxy(ProxySelector.of(new InetSocketAddress("proxy.example.com", 8080)))`
>> - Sets a **proxy server** for your HTTP requests.
>> 	- Useful in corporate networks, VPNs, or for logging requests.
>> - `ProxySelector.of()` takes an `InetSocketAddress`.
>> 	- Can also use `ProxySelector.getDefault()` to use system proxy.
>>> [!info] Local Proxy 
>>> ```java
>>> HttpClient client = HttpClient.newBuilder()
>>>     .proxy(ProxySelector.of(new InetSocketAddress("127.0.0.1", 8888)))
>>>     .build();
>>> ```
>>> This sends all requests through a local proxy
>
>> [!example]- **Authenticator**
>> - `.authenticator(Authenticator.getDefault())`
>> - Sets **credentials for authentication** (Basic, Digest, or NTLM) if the server requires it.
>> - Can also create a **custom** `Authenticator`
>>> [!info] Custom Authenticator
>>> ```java
>>> Authenticator auth = new Authenticator() {
>>>     @Override
>>>     protected PasswordAuthentication getPasswordAuthentication() {
>>>         return new PasswordAuthentication("username","password".toCharArray());
>>>     }
>>> };
>>> 
>>> HttpClient client = HttpClient.newBuilder()
>>>     .authenticator(auth)
>>>     .build();
>>> ```
>>> The client will automatically send credentials **when challenged by the server**.

## HttpRequest
[Class HttpRequest](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/HttpRequest.html)
### Methods
> [!example]
> ```java
> import java.net.http.HttpRequest;
> import java.net.URI;
> 
> HttpRequest request = HttpRequest.newBuilder()
>     .uri(URI.create("https://api.example.com/users"))
>     .GET()  // HTTP method
>     .build();
> ```
#### GET
> [!example]
> ```java
> HttpRequest getRequest = HttpRequest.newBuilder()
>     .uri(URI.create("https://api.example.com/users/42"))
>     .GET() // No Body allowed
>     .header("Accept", "application/json")
>     .build();
> ```

#### POST
> [!example]
> ```java
> String json = "{\"name\":\"John Doe\"}";
> 
> HttpRequest postRequest = HttpRequest.newBuilder()
>     .uri(URI.create("https://api.example.com/users"))
>     .POST(HttpRequest.BodyPublishers.ofString(json))
>     .header("Content-Type", "application/json")
>     .build();
> ```
> - `POST(BodyPublisher)` sends a body (string, byte array, or file)
> - `BodyPublishers.ofString(json)` — simplest way for JSON payload

#### PUT
> [!example]
> ```java
> String json = "{\"name\":\"John Doe\"}";
> 
> HttpRequest putRequest = HttpRequest.newBuilder()
>     .uri(URI.create("https://api.example.com/users/42"))
>     .PUT(HttpRequest.BodyPublishers.ofString(json))
>     .header("Content-Type", "application/json")
>     .build();
> ```
> - Similar to POST

#### PATCH
> [!example]
> ```java
> HttpRequest patchRequest = HttpRequest.newBuilder()
>     .uri(URI.create("https://api.example.com/users/42"))
>     .method("PATCH", HttpRequest.BodyPublishers.ofString("{\"weight\":90}"))
>     .header("Content-Type", "application/json")
>     .build();
> ```
> 
>> [!info]
>>- Java `HttpClient` does **not have a built-in PATCH method**
>> - Use `method("PATCH", BodyPublishers)`

#### DELETE
> [!example]
> ```java
> HttpRequest deleteRequest = HttpRequest.newBuilder()
>     .uri(URI.create("https://api.example.com/users/42"))
>     .DELETE() // Usually No Body
>     .build();
> ```

### Headers
> [!info] 
> ```java
> HttpRequest request = HttpRequest.newBuilder()
>     .uri(URI.create("https://api.example.com/users"))
>     .header("Content-Type", "application/json")
>     .header("Authorization", "Bearer token123")
>     .GET()
>     .build();
> ```
> Use `.header()` to add a single header
> ```java
> .headers("Accept", "application/json", "User-Agent", "JavaHttpClient/1.0")
> ```
> Use `.headers(String...)` to add multiple in one call

### Timeout Per Request
> [!info]
> ```java
> HttpRequest request = HttpRequest.newBuilder()
>     .uri(URI.create("https://api.example.com/users"))
>     .timeout(Duration.ofSeconds(5)) // request timeout
>     .GET()
>     .build();
> ```
> - Overrides the `HttpClient` global timeout (if set)   
> - If exceeded → `HttpTimeoutException` is thrown

### HTTP Version Override
> [!info]
> ```java
> HttpRequest request = HttpRequest.newBuilder()
>     .uri(URI.create("https://api.example.com/users"))
>     .version(HttpClient.Version.HTTP_2)
>     .GET()
>     .build();
> ```
> - If omitted, uses client default (`HTTP_2` or `HTTP_1_1`)
> - Useful if you want to test HTTP/1.1 explicitly

### Body Publishers
- **No body**: `.GET()` or `.noBody()`
- **String**: `BodyPublishers.ofString("...")`
- **Byte array**: `BodyPublishers.ofByteArray(byte[])`
- **File**: `BodyPublishers.ofFile(Path path)`
	-     `.POST(HttpRequest.BodyPublishers.ofFile(Path.of("file.txt")))`



