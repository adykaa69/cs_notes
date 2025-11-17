# What is REST?
- REST (Representational State Transfer) is an architectural style for designing networked applications.
- REST API is a type of API (Application Programming Interface) that allows communication between different systems over the internet.
	- Works by sending requests and receiving responses, typically in JSON format, between the client and server.
## Key concepts
- Resources are identified by **URLs**.
- Clients interact with resources using **standard HTTP methods**.
- Communication is **stateless**, meaning each request contains everything required for the server to process it.

> [!note]
> - **REST**: architectural design / a set of principles guiding APIs
> 	- not a protocol, nor a standard
> - **HTTP**: communication protocol

# HTTP Methods
## Type of Methods
- In HTTP, there are five methods that are commonly used in a REST-based architecture:
	- [[#GET]]
	- [[#POST]]
	- [[#PUT]]
	- [[#PATCH]]
	- [[#DELETE]]
### GET
- The HTTP GET method is used to **read** (or retrieve) a representation of a resource.
	- Success: returns a representation in XML or JSON and #todo **200** (OK).
	- Error: returns **404** (Not Found) or **400** (Bad Request)

> [!example]
> Request:
> ```bash
> GET  /api/v1/users/123
> ```
> Response:
> ```bash
> 200 OK
> {
>   "id": 123,
>   "name": "Harry Potter"
> }
> ```
> - This request fetches data for the user with ID 123.

### POST
- The HTTP POST method is commonly used to create new resources.
- It is often used to create subordinate resources related to a parent resource.
	- Success: returns Location header and **201** (Created)
		- Location Header: URL of the recently created resource

> [!example]
> Request:
> ```bash
> POST /api/v1/users
> Content-Type: application/json
> 
> {
>   "name": "Tom Riddle"
>   "email": "tom.riddle@hogwarts.com"
> }
> ```
> Response:
> ```bash
> HTTP/1.1 201 Created
> Location: https://api.example.com/api/v1/users/124
> Content-Type: application/json
> 
> {
>   "id": 124,
>   "name": "Tom Riddle"
>   "email": "tom.riddle@hogwarts.com"
> }
> ```

### PUT
- HTTP PUT method is used to update a resource on the server.
- When using PUT, the entire resource is sent in the request body, and it replaces the current resource at the specified URL.
	- Returns 200 (OK)
- If the resource doesn’t exist, and it **can** create a new one
	- Returns 201 (Created)
- If the resource doesn't exist, and it **cannot** create a new one
	- Returns 404 (Not Found)

> [!example]
> Request:
> ```bash
> PUT /api/v1/users/124
> Content-Type: application/json
> 
> {
>   "name": "Lord Voldemort",
>   "email": "lord.voldemort@azkaban.com"
> }
> ```
> Response I. - Resource already exists -> Returns 200 OK
> ```bash
> HTTP/1.1 200 OK
> Content-Type: application/json
> 
> {
>   "id": 124,
>   "name": "Lord Voldemort",
>   "email": "lord.voldemort@azkaban.com"
> }
> ```
> Response II. - Resource did NOT exist -> PUT can create it -> Returns 201 Created
> ```bash
> HTTP/1.1 201 Created
> Location: https://api.example.com/api/v1/users/124
> Content-Type: application/json
> 
> {
>   "id": 124,
>   "name": "Lord Voldemort",
>   "email": "lord.voldemort@azkaban.com"
> }
> ```
> Response III. - Resource did NOT exist -> Returns 404 Not Found
> ```bash
> HTTP/1.1 404 Not Found
> Content-Type: application/json
> 
> {
>   "error": "User with ID 124 not found."
> }
> ```

### PATCH
- HTTP PATCH method is used to partially update a resource on the server.
- Unlike PUT, PATCH only requires the fields that need to be updated to be sent in the request body. 
- It modifies specific parts of the resource rather than replacing the entire resource.

> [!example]
> Request:
> ```bash
> PATCH /api/v1/users/124
> Content-Type: application/json
> 
> {
>   "name": "Dark Lord"
> }
> ```
> Response:
> ```bash
> HTTP/1.1 200 OK
> Content-Type: application/json
> 
> {
>   "id": 124,
>   "name": "Dark Lord",
>   "email": "lord.voldemort@azkaban.com"
> }
> ```

### DELETE
- HTTP DELETE method is used to ****delete**** a resource identified by a URI.
- On successful deletion, return HTTP status 200 (OK) along with a response body.

> [!example]
> Request:
> ```bash
> DELETE /api/v1/users/124
> ```
> Response:
> ```bash
> HTTP/1.1 200 OK
> Content-Type: application/json
> 
> {
>   "message": "User with ID 124 deleted successfully."
> }
> ```

## Idempotency

# Status Codes