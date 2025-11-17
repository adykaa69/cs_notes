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
```java
import java.net.http.HttpClient;

HttpClient client = HttpClient.newHttpClient();
```