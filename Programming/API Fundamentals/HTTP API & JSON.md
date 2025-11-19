[Baeldung - Exploring the New HTTP Client in Java](https://www.baeldung.com/java-9-http-client)
[JSON Placeholder]("https://jsonplaceholder.typicode.com/users/1")
- `java.net.http.HttpClient` API was introduced in Java 11
- Replaced the old `HttpURLConnection`
-  `HttpClient` > `HttpURLConnection`
	- easier to work with
	- supporting synchronous and asynchronous calls

# Basic Classes
- **[[#HttpClient]]**: the main client object used to send HTTP requests.
- **[[#HttpRequest]]**: represents the request (method, URI, headers, body).
- **[[#HttpResponse]]**: represents the response (status code, headers, body).
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

#### Accept vs Content-Type
- Accept: Response format I can understand
	- ```java
	  .header("Accept", "application/json")
	  // I want JSON in response 
	  ```
- Content-Type: Request format I'm sending
	- ```java
	  .header("Content-Type", "application/json")
	  // I am sending JSON in body
	  ```

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
	-    ->  `.POST(HttpRequest.BodyPublishers.ofFile(Path.of("file.txt")))`

### URI
> [!info]
> ```java
> URI uri = URI.create("https://api.example.com/users");
> ```
> - Simple, literal URIs, no checked exceptions
> 
> ```java
> try {
>     URI uri = new URI("https", "api.example.com", "/users", null);
> } catch (URISyntaxException e) {
>     e.printStackTrace();
> }
> ```
> - Needs try-catch
> - More flexible, safer if you expect invalid input or need to construct from parts

## HttpResponse
[Interface HttpResponse\<T>](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/HttpResponse.html)
- `HttpResponse<T>` contains several key parts:
	- `statusCode()` - returns HTTP status code
	- `headers(`) - returns an **HttpHeaders** object containing all response headers
	- `body() `- returns body of response, tpye depends on BodyHandler (String, byte[], file)
	- `version()` - returns HTTP version used (HTTP/1.1 or HTTP/2)
	- `uri()` - returns the URI of the request that produced this response

# Sending a Request
## Synchronous Request
> [!example]
> ```java
> import java.net.URI;
> import java.net.http.HttpClient;
> import java.net.http.HttpRequest;
> import java.net.http.HttpResponse;
> import java.time.Duration;
> 
> public class HttpExample {
>     public static void main(String[] args) {
>         try {
>             // 1. Create the client
>             HttpClient client = HttpClient.newHttpClient();
> 
>             // 2. Create the request
>             HttpRequest request = HttpRequest.newBuilder()
>                 .uri(URI.create("https://jsonplaceholder.typicode.com/users/1"))
>                 .timeout(Duration.ofSeconds(10))
>                 .header("Accept", "application/json")
>                 .GET()
>                 .build();
> 
>             // 3. Send the request synchronously
>             HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
> 
>             // 4. Print the response details
>             System.out.println("Status code: " + response.statusCode());
>             System.out.println("Headers: " + response.headers().map());
>             System.out.println("Body: " + response.body());
> 
>         } catch (Exception e) {
>             e.printStackTrace();
>         }
>     }
> }
> ```
## Asynchronous Request
- Request can be send **non-blocking**:

> [!example]
> ```java
> client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
>       .thenApply(HttpResponse::body)
>       .thenAccept(System.out::println)
>       .join();  // Wait for completion
> ```
> - `sendAsync` returns a `CompletableFuture<HttpResponse<T>>`
> - Non-blocking → your program can continue doing other things while waiting
> - `.join()` waits for the response (optional)

# JSON Parsing

## Gson
- Google JSON Library

### Parsing JSON String
> [!example]
> ```json
> {
>   "id": 1,
>   "name": "Leanne Graham",
>   "username": "Bret",
>   "email": "Sincere@april.biz"
> }
> ```
> ```java
> import com.google.gson.JsonObject;
> import com.google.gson.JsonElement;
> import com.google.gson.JsonParser;
> 
> // Assume responseBody is the String from response.body()
> String responseBody = """
> {
>   "id": 1,
>   "name": "Leanne Graham",
>   "username": "Bret",
>   "email": "Sincere@april.biz"
> }
> """;
> 
> // Parse JSON string
> JsonElement element = JsonParser.parseString(responseBody);
> JsonObject obj = element.getAsJsonObject();
> 
> // Access fields
> String name = obj.get("name").getAsString();
> String email = obj.get("email").getAsString();
> ```

### Parsing nested JSON
> [!example]
> ```json
> {
>   "id": 1,
>   "name": "Leanne Graham",
>   "address": {
>     "street": "Kulas Light",
>     "city": "Gwenborough"
>   }
> }
> ```
> ```java
> String responseBody = """
> {
>   "id": 1,
>   "name": "Leanne Graham",
>   "address": {
>     "street": "Kulas Light",
>     "city": "Gwenborough",
>   }
> }
> """;
> 
> // Parse JSON string
> JsonElement element = JsonParser.parseString(responseBody);
> JsonObject root = element.getAsJsonObject();
> 
> // Top-level field
> String name = root.get("name").getAsString();
> 
> // Nested object
> JsonObject address = root.getAsJsonObject("address");
> 
> String street = address.get("street").getAsString();
> String city = address.get("city").getAsString();
> ```

### Parsing Arrays

> [!example]
> ```json
> {
>   "users": [
>     {"name": "Alice", "email": "alice@example.com"},
>     {"name": "Bob", "email": "bob@example.com"}
>   ]
> }
> ```
> ```java
> String responseBody = """
> {
>   "users": [
> 	{"name": "Alice", "email": "alice@example.com"},
> 	{"name": "Bob", "email": "bob@example.com"}
>   ]
> }
> """;
> 
> // Parse JSON
> JsonElement element = JsonParser.parseString(responseBody);
> JsonObject root = element.getAsJsonObject();
> 
> // Access array
> JsonArray users = root.getAsJsonArray("users");
> 
> // Iterate over array
> for (JsonElement userElement : users) {
> 	JsonObject user = userElement.getAsJsonObject();
> 
> 	String name = user.get("name").getAsString();
> 	String email = user.get("email").getAsString();
> }
> ```

## Jackson
- FasterXML Jackson library

### Parsing JSON String
> [!example]
> ```json
> {
>   "id": 1,
>   "name": "Leanne Graham",
>   "username": "Bret",
>   "email": "Sincere@april.biz"
> }
> ```
> ```java
> import com.fasterxml.jackson.databind.JsonNode;
> import com.fasterxml.jackson.databind.ObjectMapper;
> 
> String responseBody = """
> {
>   "id": 1,
>   "name": "Leanne Graham",
>   "username": "Bret",
>   "email": "Sincere@april.biz"
> }
> """;
> 
> ObjectMapper mapper = new ObjectMapper();
> JsonNode root = mapper.readTree(responseBody);
> 
> String name = root.get("name").asText();
> String email = root.get("email").asText();
> 
> System.out.println("Name: " + name);
> System.out.println("Email: " + email);
> ```

### Parsing Nested JSON
> [!exmple]
> ```json
> {
>   "id": 1,
>   "name": "Leanne Graham",
>   "address": {
>     "street": "Kulas Light",
>     "city": "Gwenborough"
>   }
> }
> ```
> ```java
> String responseBody = """
> {
>   "id": 1,
>   "name": "Leanne Graham",
>   "address": {
>     "street": "Kulas Light",
>     "city": "Gwenborough"
>   }
> }
> """;
> 
> ObjectMapper mapper = new ObjectMapper();
> JsonNode root = mapper.readTree(responseBody);
> 
> String name = root.get("name").asText();
> JsonNode address = root.get("address");
> 
> String street = address.get("street").asText();
> String city = address.get("city").asText();
> ```

### Parsing Arrays
> [!example]
> ```json
> {
>   "users": [
>     {"name": "Alice", "email": "alice@example.com"},
>     {"name": "Bob", "email": "bob@example.com"}
>   ]
> }
> ```
> ```java
> String responseBody = """
> {
>   "users": [
>     {"name": "Alice", "email": "alice@example.com"},
>     {"name": "Bob", "email": "bob@example.com"}
>   ]
> }
> """;
> 
> ObjectMapper mapper = new ObjectMapper();
> JsonNode root = mapper.readTree(responseBody);
> JsonNode users = root.get("users");
> 
> for (JsonNode user : users) {
>     String name = user.get("name").asText();
>     String email = user.get("email").asText();
>     System.out.println("Name: " + name + ", Email: " + email);
> }
> ```
## Jackson vs Gson
### Annotations
**Gson**
- `@SerializedName` → rename fields for JSON mapping.

**Jackson**
- `@JsonProperty` → rename fields.
- `@JsonIgnore` → ignore fields.
- `@JsonCreator` → control constructor-based deserialization.
- `@JsonInclude` → include/exclude null or default values.

### Parsing Approach
**Gson**
- Uses `JsonElement`, `JsonObject`, and `JsonArray` for tree-based parsing.    
- Can also map JSON directly to Java classes using `Gson.fromJson(json, Class.class)`.
- Slightly slower for large JSON datasets.
        
**Jackson**
- Uses `JsonNode` for tree-based parsing.
- Maps JSON to Java classes using `ObjectMapper.readValue(json, Class.class)`.    
- Extremely fast and optimized for large data.

### Jackson preference
- Jackson’s annotation system is **more extensive and flexible**, which is why it’s often preferred in enterprise applications.
- **Jackson**: Default JSON library in Spring Boot, fully integrated with `@RestController`, `@RequestBody`, `@ResponseBody`.
- Jackson is generally **faster** than Gson for large JSON structures.


# Pagination
- APIs often return data in **chunks (pages)** instead of all at once, for efficiency.

> [!example]
> ```json
> {
>   "page": 1,
>   "per_page": 2,
>   "total": 6,
>   "total_pages": 3,
>   "data": [
>     {"id":1,"name":"Alice"},
>     {"id":2,"name":"Bob"}
>   ]
> }
> ```
> - `page` - current page number
> - `per_page` - number of results per page
> - `total` - total number of records
> - `total_pages` -  how many pages in total
> - `data` - array of items

## Pagination Handling
You **loop through all pages**, sending a GET request for each page and collecting results.

**Step-by-Step**
1. **Send the first request** to get `total_pages`.
2. **Loop from 1 to total_pages**, sending requests for each page.
3. **Parse the response body** (JSON) and extract `data`.
4. **Process each item**.

> [!example]
> Paginated GET with HttpClient + Gson
> ```java
> import java.net.URI;
> import java.net.http.*;
> import com.google.gson.*;
> 
> public class PaginationExample {
>     private static final String BASE_URL = "https://jsonmock.hackerrank.com/api/medical_records?page=";
> 
>     public static void main(String[] args) throws Exception {
>         HttpClient client = HttpClient.newHttpClient();
> 
>         // Step 1: Get first page to know total_pages
>         HttpRequest firstRequest = HttpRequest.newBuilder()
>                 .uri(URI.create(BASE_URL + "1"))
>                 .GET()
>                 .build();
> 
>         HttpResponse<String> firstResponse = client.send(firstRequest, HttpResponse.BodyHandlers.ofString());
> 
>         JsonObject firstJson = JsonParser.parseString(firstResponse.body()).getAsJsonObject();
>         int totalPages = firstJson.get("total_pages").getAsInt();
> 
>         System.out.println("Total pages: " + totalPages);
> 
>         // Step 2: Loop through all pages
>         for (int page = 1; page <= totalPages; page++) {
>             HttpRequest request = HttpRequest.newBuilder()
>                     .uri(URI.create(BASE_URL + page))
>                     .GET()
>                     .build();
> 
>             HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
>             JsonObject json = JsonParser.parseString(response.body()).getAsJsonObject();
>             JsonArray data = json.getAsJsonArray("data");
> 
>             // Step 3: Iterate over items
>             for (JsonElement element : data) {
>                 JsonObject record = element.getAsJsonObject();
>                 String name = record.get("userName").getAsString();
>                 System.out.println("User: " + name);
>             }
>         }
>     }
> }
> 
> ```

# URI Basics
- **URI (Uniform Resource Identifier)** identifies a resource on the web.

## URI parts
URI can be split into several parts:
`scheme://host:port/path?query#fragment`

- **scheme** → protocol (http, https, ftp)
- **host** → domain or IP (api.example.com)
- **port** → optional (default 80 for HTTP, 443 for HTTPS)
- **path** → resource location (`/users/42`)
- **query** → optional parameters (`?page=2&sort=asc`)
- **fragment** → optional anchor (`#section1`)

> [!example]
> `https://api.example.com/users/42?verbose=true#details`
> - scheme: `https`
> - host: `api.example.com`
> - path: `/users/42`
> - query: `verbose=true`
> - fragment: `details`

## Symbols in URIs
1. Slash `/`
	- Separates **path segments**.
	- 