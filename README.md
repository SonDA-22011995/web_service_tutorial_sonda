- [REST API](#rest-api)
  - [WHAT IS THE REST API](#what-is-the-rest-api)
  - [URI](#uri)
  - [URL - Uniform Resource Locator](#url---uniform-resource-locator)
    - [URL encoding](#url-encoding)
  - [HTTP Request](#http-request)
    - [What is an HTTP Request?](#what-is-an-http-request)
    - [Client–Server Model](#clientserver-model)
    - [Structure of an HTTP Request](#structure-of-an-http-request)
    - [Example](#example)
    - [Request Line](#request-line)
    - [HTTP Methods](#http-methods)
    - [HTTP Headers](#http-headers)
    - [Request Body](#request-body)
    - [Query Parameters](#query-parameters)
    - [HTTP Request vs HTTP Response](#http-request-vs-http-response)
    - [Responses HTTP codes](#responses-http-codes)
      - [1xx — Informational](#1xx--informational)
      - [2xx — Success](#2xx--success)
      - [3xx — Redirection](#3xx--redirection)
      - [4xx — Client Errors](#4xx--client-errors)
      - [5xx — Server Errors](#5xx--server-errors)
    - [HTTP IDEMPOTENCY, SAFETY, AND CACHING](#http-idempotency-safety-and-caching)
      - [WHAT IS IDEMPOTENCY](#what-is-idempotency)
      - [WHAT IS SAFETY](#what-is-safety)
      - [WHAT IS CACHING](#what-is-caching)
  - [The Six Constraints](#the-six-constraints)
    - [Uniform Interface](#uniform-interface)
      - [Resource Identification (via URI)](#resource-identification-via-uri)
      - [Manipulation of Resources through Representations](#manipulation-of-resources-through-representations)
      - [Self-Descriptive Messages](#self-descriptive-messages)
      - [Hypermedia as the Engine of Application State (HATEOAS)](#hypermedia-as-the-engine-of-application-state-hateoas)
    - [Stateless](#stateless)
    - [Cacheable](#cacheable)
    - [Client-Server](#client-server)
    - [Layered System](#layered-system)
  - [Code on Demand](#code-on-demand)
  - [API Maturity Models](#api-maturity-models)
  - [RESTful web API design](#restful-web-api-design)
    - [API Endpoints](#api-endpoints)
    - [Implement asynchronous methods](#implement-asynchronous-methods)
    - [Implement API version](#implement-api-version)
      - [URI versioning](#uri-versioning)
      - [Query string versioning](#query-string-versioning)
      - [Header versioning](#header-versioning)
      - [Media type versioning](#media-type-versioning)
      - [REST API Versioning – Advantages \& Disadvantages](#rest-api-versioning--advantages--disadvantages)
    - [Implement data pagination and filtering](#implement-data-pagination-and-filtering)
      - [Pagination](#pagination)
      - [Filtering](#filtering)
      - [Sorting](#sorting)
      - [Field selection for client-defined projections](#field-selection-for-client-defined-projections)
    - [Implement HATEOAS (Hypertext as the Engine of Application State)](#implement-hateoas-hypertext-as-the-engine-of-application-state)
    - [Support partial responses](#support-partial-responses)
    - [Multitenant web APIs](#multitenant-web-apis)
      - [Subdomain or Domain-Based Isolation (DNS-Level Tenancy)](#subdomain-or-domain-based-isolation-dns-level-tenancy)
      - [Pass tenant-specific HTTP headers](#pass-tenant-specific-http-headers)
      - [Pass tenant-specific information through the URI path](#pass-tenant-specific-information-through-the-uri-path)
    - [CACHING](#caching)
  - [Security](#security)
    - [JSON Web Token](#json-web-token)
      - [When should you use JSON Web Tokens?](#when-should-you-use-json-web-tokens)
      - [What is the JSON Web Token structure?](#what-is-the-json-web-token-structure)
      - [Header](#header)
      - [Payload](#payload)
      - [Signature](#signature)
      - [Example](#example-1)
        - [ALGORITHM - HS256](#algorithm---hs256)
        - [ALGORITHM - RS256](#algorithm---rs256)
    - [Transport Layer Security](#transport-layer-security)
  - [Documentation](#documentation)
    - [OpenAPI Specification](#openapi-specification)
  - [Trade-Offs](#trade-offs)
    - [Advantages](#advantages)
    - [Disadvantages](#disadvantages)

# REST API

## WHAT IS THE REST API

- Technically, **REST** is an acronym for **“REpresentational State Transfer”** which is simply an architectural style originally written about by **Roy Fielding** in his doctoral dissertation

- Makes an HTTP request to a URL…
  - Using one of the standard HTTP methods (GET, PUT, POST, PATCH, DELETE, etc.)…
  - With some content (usually JSON) in the body…
- And waits for a response, which:
  - Indicates status via an HTTP response code
  - And usually has more JSON in the body.

## URI

- The purpose of a Uniform Resource Identifier (URI) is to provide an identifier to distinguish the given resource from other resources. A URI distinguishes but doesn’t guarantee an identity of a resource, since the underlying resource can be changed.A URI is a string of characters that identifies a resource
- URI is a broad term that encapsulates two additional terms:
  - URL - Uniform Resource Locator
  - URN - Uniform Resource Name

## URL - Uniform Resource Locator

`http://www.example.com:80/blog/2025/search?tag=api&sort=date#latest`

| URL value         | URL term                | Explanation                                                      |
| ----------------- | ----------------------- | ---------------------------------------------------------------- |
| http              | Scheme                  | The application layer protocol used to access the resource       |
| ://               | Scheme separator        | Separates the scheme from the rest of the URL                    |
| www               | Subdomain               | Identifies a subdivision of the domain                           |
| example           | Domain                  | The main domain name identifying the server                      |
| .                 | Domain separator        | Separates parts of the domain name                               |
| com               | Top-level domain (TLD)  | Represents the highest level of the domain hierarchy             |
| :                 | Port delimiter          | Separates the domain name from the application port number       |
| 80                | Application port        | The port number on which the application handles requests        |
| /blog/2025/search | Path                    | The location of the resource on the server                       |
| ?                 | Query string separator  | Separates the path from the query string                         |
| tag=api&sort=date | Query string parameters | Key-value pairs used to pass additional data to the server       |
| #                 | Fragment identifier     | Marks the beginning of a fragment identifier within the resource |
| latest            | Fragment                | The section or element within the resource                       |

### URL encoding

- **URL Encoding** (also called percent-encoding) is the process of converting unsafe or reserved characters in a URL into a safe format

```
character → UTF-8 bytes → hexadecimal → %HH
```

- Example:

  - `Space → ASCII 32 → Hex 20 → %20`

  - `& → ASCII 38 → Hex 26 → %26`

- Why **URL Encoding** is required:

  - URLs are only guaranteed to work safely with ASCII characters.
  - Characters like spaces, symbols, or Unicode (e.g. Vietnamese characters) must be encoded to avoid ambiguity.

- Characters that must be encoded
  - Reserved characters: These characters have special meanings in URLs and must be encoded when used as data `! * ' ( ) ; : @ & = + $ , / ? # [ ]`
  - Space `space → %20   (or + in form encoding)`
  - Unicode characters `điện thoại → %C4%91i%E1%BB%87n%20tho%E1%BA%A1i`
- How to do URL Encoding (in practice)
  - JavaScript: `encodeURIComponent("điện thoại & phụ kiện")` -> `%C4%91i%E1%BB%87n%20tho%E1%BA%A1i%20%26%20ph%E1%BB%A5%20ki%E1%BB%87n`
  - Python

Encode query parameters

```
from urllib.parse import urlencode

params = {"q": "điện thoại & phụ kiện"}
query_string = urlencode(params)

```

Encode a single string

```
from urllib.parse import quote

quote("điện thoại & phụ kiện")

```

## HTTP Request

### What is an HTTP Request?

An **HTTP request** is a message sent by a **client** (browser, mobile app, API client) to a **server** to ask for a resource or to perform an action.

Examples:

- A browser requests a web page
- A frontend app requests data from a backend API
- A client sends data to create or update a record

HTTP requests follow the **HTTP (HyperText Transfer Protocol)** standard.

---

### Client–Server Model

HTTP works based on a **client–server architecture**:

1. The **client** sends an HTTP request
2. The **server** processes the request
3. The **server** returns an HTTP response

The server does **nothing** until it receives a request.

---

### Structure of an HTTP Request

An HTTP request consists of **four main parts**:

1. Request Line
2. Headers
3. Blank Line
4. Body (optional)

### Example

```
POST /users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Authorization: Bearer token123

{
    "name": "Alice",
    "email": "alice@example.com
"
}
```

### Request Line

- The request line contains: `METHOD /path HTTP/version`

```
GET /users/1 HTTP/1.1
```

- Components:

  - **HTTP Method**– what action the client wants to perform
  - **Path (URL)** – the resource being accessed
  - **HTTP Version** – protocol version (HTTP/1.1, HTTP/2)

### HTTP Methods

- HTTP methods describe the intention of the request.

  | Method | Purpose | Description                          |
  | ------ | ------- | ------------------------------------ |
  | GET    | Read    | Retrieve data (no data modification) |
  | POST   | Create  | Create a new resource                |
  | PUT    | Update  | Replace an existing resource         |
  | PATCH  | Update  | Partially update a resource          |
  | DELETE | Delete  | Remove a resource                    |

### HTTP Headers

- Headers provide metadata about the request.

- Common request headers:

| Header        | Description                |
| ------------- | -------------------------- |
| Host          | Target server              |
| Content-Type  | Format of request body     |
| Authorization | Authentication credentials |
| User-Agent    | Client information         |
| Accept        | Expected response format   |

- Example:

```
Content-Type: application/json
```

### Request Body

- The request body contains data sent to the server.

- Used mainly with POST, PUT, PATCH

- Not typically used with GET

- Common body formats: JSON, Form data, XML

- Example (JSON):

```
{
    "username": "john",
    "password": "secret"
}
```

### Query Parameters

- Query parameters are part of the URL and are used to pass filtering or optional data.
- Start with `?`
- Separated by `&`

Example:

```
GET /users?page=2&limit=10
```

### HTTP Request vs HTTP Response

| HTTP Request              | HTTP Response               |
| ------------------------- | --------------------------- |
| Sent by client            | Sent by server              |
| Asks for an action        | Returns result              |
| Contains method & headers | Contains status code & data |

### Responses HTTP codes

#### 1xx — Informational

| Code    | Meaning             |
| ------- | ------------------- |
| **100** | Continue            |
| **101** | Switching Protocols |
| **102** | Processing          |

#### 2xx — Success

| Code    | Meaning                       |
| ------- | ----------------------------- |
| **200** | OK                            |
| **201** | Created                       |
| **202** | Accepted                      |
| **203** | Non-Authoritative Information |
| **204** | No Content                    |
| **205** | Reset Content                 |
| **206** | Partial Content               |

#### 3xx — Redirection

| Code    | Meaning                    |
| ------- | -------------------------- |
| **300** | Multiple Choices           |
| **301** | Moved Permanently          |
| **302** | Found (Temporary Redirect) |
| **303** | See Other                  |
| **304** | Not Modified               |
| **307** | Temporary Redirect         |
| **308** | Permanent Redirect         |

#### 4xx — Client Errors

| Code    | Meaning                |
| ------- | ---------------------- |
| **400** | Bad Request            |
| **401** | Unauthorized           |
| **402** | Payment Required       |
| **403** | Forbidden              |
| **404** | Not Found              |
| **405** | Method Not Allowed     |
| **406** | Not Acceptable         |
| **408** | Request Timeout        |
| **409** | Conflict               |
| **410** | Gone                   |
| **411** | Length Required        |
| **413** | Payload Too Large      |
| **414** | URI Too Long           |
| **415** | Unsupported Media Type |
| **422** | Unprocessable Entity   |
| **429** | Too Many Requests      |

#### 5xx — Server Errors

| Code    | Meaning                    |
| ------- | -------------------------- |
| **500** | Internal Server Error      |
| **501** | Not Implemented            |
| **502** | Bad Gateway                |
| **503** | Service Unavailable        |
| **504** | Gateway Timeout            |
| **505** | HTTP Version Not Supported |
| **507** | Insufficient Storage       |
| **508** | Loop Detected              |

### HTTP IDEMPOTENCY, SAFETY, AND CACHING

#### WHAT IS IDEMPOTENCY

- An idempotent HTTP method has the same effect on the server when called once or multiple times.
- For example
  - No matter how many times you retrieve a resource with the `GET` method, it’ll return the same data.
  - The same applies to the DELETE method. When performing an initial DELETE request, the server will respond with an HTTP 200 (OK) status code, and any consecutive DELETE
    request to the same resource will return an HTTP 404 (Not Found) status code

#### WHAT IS SAFETY

- An operation is **“safe”** if it doesn’t modify the server’s state.
- For example
  - the `POST` method is not safe because it will create a new resource.
  - But a safe method doesn’t necessarily have to be read-only. Reading a resource may still cause side effects on the server, such as logging or rate-limiting of the requests.
  - The method is considered safe as long as the client does not initiate those side effects.

#### WHAT IS CACHING

- The term caching describes conditions in which an existing response can be used to satisfy subsequent
  requests
- Caching is used to reduce the response time and network bandwidth consumption.
- Caching happens through the use of a cache (local or remote store) that controls the response storage, retrieval, and deletion.

| HTTP Method | Idempotent | Safe | Cacheable |
| ----------- | ---------- | ---- | --------- |
| GET         | Yes        | Yes  | Yes       |
| HEAD        | Yes        | Yes  | Yes       |
| POST        | No         | No   | No        |
| PUT         | Yes        | No   | No        |
| DELETE      | Yes        | No   | No        |
| CONNECT     | No         | No   | No        |
| OPTIONS     | Yes        | Yes  | No        |
| TRACE       | Yes        | Yes  | No        |
| PATCH       | No         | No   | No        |

## The Six Constraints

- The REST architectural style describes six constraints. These constraints, applied to the architecture, were originally communicated by Roy Fielding in his doctoral dissertation and defines the basis of RESTful-style

### Uniform Interface

- The uniform interface constraint defines the interface between clients and servers

#### Resource Identification (via URI)

- Each resource is uniquely identified, typically through the URI
- URI represents nouns (resources), not actions.

- Good example

```
GET /users/123
GET /orders/456
```

- Bad example (RPC style)

```
GET /getUserById?id=123
POST /fetchOrder
```

#### Manipulation of Resources through Representations

- Clients don’t modify resources directly, but through representations (JSON, XML, etc.).

- Example:
  - The JSON body is the representation
  - Server updates the resource based on that representation

```
PUT /users/123
Content-Type: application/json

{
  "name": "Alice",
  "email": "alice@example.com"
}
```

#### Self-Descriptive Messages

- Each message includes all the information necessary for understanding and processing it
- Example request:
  - What format the data is `Content-Type`
  - How to authenticate `Authorization`
  - What action to take `POST /orders`

```
POST /orders
Content-Type: application/json
Authorization: Bearer token
```

#### Hypermedia as the Engine of Application State (HATEOAS)

- Clients should be able to navigate through REST APIs using hyperlinks included in the API’s response. Ideally, clients need to know only the initial entry point and can then explore available interactions through hypertext/hypermedia links
- Example response:
  - Client logic: Doesn’t hardcode URLs. Follows links provided by the server

```
{
  "id": 123,
  "status": "pending",
  "links": {
    "self": "/orders/123",
    "cancel": "/orders/123/cancel",
    "pay": "/orders/123/pay"
  }
}
```

### Stateless

- Each client request contains all the information needed to understand and complete the request by the server (independently of any previous requests).
- The server doesn’t store any session state.
- If the application requires state persistence, that state is maintained on the client
- Example (GOOD): Token is sent every time. Server doesn’t care about previous calls
  - Token is sent every time
  - Server doesn’t care about previous calls

Request 1

```
GET /users/123
Authorization: Bearer eyJhbGciOi...
```

Request 2

```
GET /users/123/orders
Authorization: Bearer eyJhbGciOi...

```

### Cacheable

- Responses should explicitly indicate whether they are cacheable.
- Caching improves network efficiency, reduces latency, and provides better user-perceived performance by allowing clients and intermediaries to reuse responses when appropriate
- Examples:

**Cacheable response**

```
HTTP/1.1 200 OK
Cache-Control: public, max-age=3600
Content-Type: application/json

{
  "id": 123,
  "title": "REST API Guide"
}
```

### Client-Server

- Separates responsibilities between the client and the server, where the client makes the request, and the server produces the response. This constraint leads to improved portability across platforms because clients and servers can evolve independently

### Layered System

- Component behavior can be encapsulated within a hierarchy of layers, where the interaction of the layer is limited to the layers with which it’s interacting. As a result of this constraint, content can move, for example, through gateways, proxies, and reverse proxies, with each acting as a layer.
- Example: Fetching a user profile

**Client request**

```
GET https://api.example.com/users/42
```

**What actually happens (hidden from the client)**

```
Client
  ↓
CDN (Cache Layer)
  ↓
API Gateway (Auth, Rate Limit)
  ↓
Load Balancer
  ↓
User Service (REST API)
  ↓
Database
```

- Client sends the request: It only knows the URL: https://api.example.com/users/42
- CDN checks cache: If the response is cached → returns it immediately. The API server is never called
- API Gateway: Verifies JWT token. Applies rate limiting
- Load Balancer: Chooses one backend server
- User Service: Reads data from the database. Returns JSON

## Code on Demand

- Client functionality can be extended by allowing the client to download and execute scripts from the server. This enables new behaviors within the client without requiring redeployment.
- Example

**Server response**

```
{
  "data": {
    "price": 100,
    "tax": 10
  },
  "script": "return data.price + data.tax;"
}
```

**Client behavior**

```
const result = new Function("data", response.script)(response.data);
```

## API Maturity Models

- Maturity models are used to measure and evaluate the maturity of processes or technologies
- In software, maturity models assess how well the software is designed and implemented

- Level 0:
  - An API at this level uses a single URI and single HTTPmethod. An example is the SOAP API, which typically uses one URI (endpoint) and the HTTP POST method to transfer a SOAP-based payload.
- Level 1
  - An API at this level uses separate URIs to identify system resources and a single HTTP method (typically POST).
- Level 2
  - An API at this level uses separate URIs to identify system resources and uses several HTTP methods on each resource, allowing it to perform Create, Read, Update, and Delete (CRUD) operations.
- Level 3
  - An API at this level inherits all properties from level 2, but it’s enriched by hypermedia controls. The controls consist of self-descriptive links that provide information about possible actions in the given state of the API resource. HATEOAS allows API consumers to navigate through APIs, and promises to reduce developers’ reliance on API documentation.

## RESTful web API design

### API Endpoints

- Avoid using verbs in URIs to represent operations.

**GOOD**

```
https://api.contoso.com/orders // Good
```

**BAD**

```
https://api.contoso.com/create-order // Avoid
```

- Entities are often grouped together into collections like `customers` or `orders`. A collection is a separate resource from the `items` inside the collection, so it should have its own URI

**URI might represent the collection of orders**

```
https://api.contoso.com/orders
```

**The URI of each item**

```
https://api.contoso.com/orders/<string:order_id>
```

- Use nouns for resource names. The HTTP `GET`, `POST`, `PUT`, `PATCH`, and `DELETE` methods already imply the verbal action

**GOOD**

```
/orders
```

**BAD**

```
/create-order
```

- Use plural nouns to name collection URIs.

**BAD**

```
GET /customer
GET /order
```

**Good**

```
GET /customers
GET /orders
```

- Consider the relationships between different types of resources and how you might expose these associations

```
/customers/5/orders # might represent all of the orders for customer 5
```

- Keep relationships simple and flexible

**BAD**: This level of complexity can be difficult to maintain and is inflexible if the relationships between resources change in the future

```
/customers/1/orders/99/products
```

**GOOD**

```
/customers/1/orders # find all the orders for customer 1
/orders/99/products # find the products in this order
```

- Avoid a large number of small resources.

**BAD**

```
GET /orders/1
GET /orders/1/items
GET /items/3
GET /items/4
GET /items/5
```

**GOOD**

```
GET /orders/1?include=items
```

- Avoid creating APIs that mirror the internal structure of a database.

**BAD**

```
GET /tbl_customer
GET /customer_order_mapping
```

**GOOD**

```
GET /customers
GET /orders
```

### Implement asynchronous methods

- An asynchronous method should return HTTP status code `202` (Accepted) to indicate that the request was accepted for processing but is incomplete.

- If the client sends a GET request to this endpoint, the response should contain the current status of the request. Optionally, it can include an estimated time to completion or a link to cancel the operation.

```
HTTP/1.1 200 OK
Content-Type: application/json

{
    "status":"In progress",
    "link": { "rel":"cancel", "method":"delete", "href":"/api/status/12345" }
}
```

- If the asynchronous operation creates a new resource, the status endpoint should return status code `303` (See Other) after the operation completes. In the `303` response, include a Location header that gives the URI of the new resource

```
HTTP/1.1 303 See Other
Location: /api/orders/12345
```

### Implement API version

- A web API that implements versioning can indicate the features and resources that it exposes, and a client application can submit requests that are directed to a specific version of a feature or resource

#### URI versioning

```
GET /api/v1/customers/10
GET /api/v2/customers/10
```

#### Query string versioning

```
GET /customers/10?version=1
GET /customers/10?version=2
```

#### Header versioning

```
GET /customers/10
API-Version: 1
```

#### Media type versioning

```
GET /customers/10
Accept: application/vnd.myapi.v1+json
```

#### REST API Versioning – Advantages & Disadvantages

| Versioning Strategy       | How it looks                          | Advantages                                                                                | Disadvantages                                                                        | When to use                            |
| ------------------------- | ------------------------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | -------------------------------------- |
| **URI Versioning**        | `/api/v1/customers/10`                | • Very clear and readable<br>• Easy to implement and test<br>• Widely used in public APIs | • URI changes per version<br>• Not pure REST<br>• HATEOAS links must include version | Public APIs, beginner-friendly systems |
| **Query Parameter**       | `/customers/10?version=2`             | • Same resource URI<br>• Quick to add to existing APIs                                    | • Poor caching support<br>• Easy to forget or misuse<br>• Less RESTful               | Temporary or internal APIs             |
| **Header Versioning**     | `API-Version: 2`                      | • Clean and stable URIs<br>• More REST-friendly<br>• Good for microservices               | • Harder to test in browser<br>• Requires good documentation                         | Internal systems, microservices        |
| **Media Type Versioning** | `Accept: application/vnd.api.v2+json` | • Most REST-correct<br>• Best for HATEOAS<br>• Clear contract via content negotiation     | • Complex to implement<br>• Hard to understand<br>• Custom media types               | Advanced REST, long-term APIs          |
| **No Versioning**         | `/customers/10`                       | • Simplest design<br>• Cleanest URIs<br>• No version overhead                             | • Cannot support breaking changes<br>• Risky for public APIs                         | Internal APIs with strict discipline   |

### Implement data pagination and filtering

#### Pagination

- Pagination divides large datasets into smaller, manageable chunks.
- Use query parameters like
  - `limit` to specify the number of items to return
  - `offset` to specify the starting point.
- Make sure to also provide meaningful defaults for `limit` and `offset`, such as `limit=25` and `offset=0`

```
GET /orders?limit=25&offset=50
```

#### Filtering

- Allows clients to refine the dataset by applying conditions. The API can allow the client to pass the filter in the query string of the URI

```
# minCost: Filters orders that have a minimum cost of 100.
# status: Filters orders that have a specific status.

GET /orders?minCost=100&status=shipped
```

#### Sorting

- Allows clients to sort data by using a sort parameter like `sort=price`

#### Field selection for client-defined projections

- Enable clients to specify only the fields that they need by using a fields parameter like `fields=id,name`

### Implement HATEOAS (Hypertext as the Engine of Application State)

- Each HTTP GET request should return the information necessary to find the resources related directly to the requested object through hyperlinks included in the response.
- The system is effectively a finite state machine, and the response to each request contains the information necessary to move from one state to another

```
{
  "orderID":3,
  "productID":2,
  "quantity":4,
  "orderValue":16.60,
  "links":[
    {
      "rel":"customer",
      "href":"https://api.contoso.com/customers/3",
      "action":"GET",
      "types":["text/xml","application/json"]
    },
    {
      "rel":"customer",
      "href":"https://api.contoso.com/customers/3",
      "action":"PUT",
      "types":["application/x-www-form-urlencoded"]
    },
    {
      "rel":"customer",
      "href":"https://api.contoso.com/customers/3",
      "action":"DELETE",
      "types":[]
    },
    {
      "rel":"self",
      "href":"https://api.contoso.com/orders/3",
      "action":"GET",
      "types":["text/xml","application/json"]
    },
    {
      "rel":"self",
      "href":"https://api.contoso.com/orders/3",
      "action":"PUT",
      "types":["application/x-www-form-urlencoded"]
    },
    {
      "rel":"self",
      "href":"https://api.contoso.com/orders/3",
      "action":"DELETE",
      "types":[]
    }]
}
```

### Support partial responses

- Some resources contain large binary fields, such as files or images. To overcome problems caused by unreliable and intermittent connections and to improve response times, consider supporting the partial retrieval of large binary resources
- To support partial responses, the web API should support the Accept-Ranges header for `GET` requests for large resources. This header indicates that the `GET` operation supports partial requests. The client application can submit `GET` requests that return a subset of a resource, specified as a range of bytes.
- A HEAD request is similar to a GET request, except that it only returns the HTTP headers that describe the resource, with an empty message body. A client application can issue a `HEAD` request to determine whether to fetch a resource by using partial `GET` requests.

Request

```
HEAD https://api.contoso.com/products/10?fields=productImage
```

Response

```
HTTP/1.1 200 OK

Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 4580
```

Request

```
GET https://api.contoso.com/products/10?fields=productImage
Range: bytes=0-2499
```

Response

```
HTTP/1.1 206 Partial Content

Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 2500
Content-Range: bytes 0-2499/4580

[...]
```

```
from flask import Flask, request, Response, abort
import os

app = Flask(__name__)

FILE_PATH = "sample.mp4"

@app.route("/video")
def get_video():
    file_size = os.path.getsize(FILE_PATH)
    range_header = request.headers.get("Range")

    if not range_header:
        # Trả toàn bộ file
        with open(FILE_PATH, "rb") as f:
            data = f.read()
        return Response(
            data,
            status=200,
            mimetype="video/mp4",
            headers={
                "Content-Length": str(file_size),
                "Accept-Ranges": "bytes",
            },
        )

    # Ví dụ: Range: bytes=0-1023
    bytes_range = range_header.replace("bytes=", "")
    start, end = bytes_range.split("-")

    start = int(start)
    end = int(end) if end else file_size - 1

    length = end - start + 1

    with open(FILE_PATH, "rb") as f:
        f.seek(start)
        chunk = f.read(length)

    return Response(
        chunk,
        status=206,
        mimetype="video/mp4",
        headers={
            "Content-Range": f"bytes {start}-{end}/{file_size}",
            "Accept-Ranges": "bytes",
            "Content-Length": str(length),
        },
    )


if __name__ == "__main__":
    app.run(debug=True)
```

```
async function downloadFile() {
  const chunkSize = 1024 * 1024; // 1MB
  let start = 0;
  let totalSize = null;
  const chunks = [];

  while (true) {
    const end = start + chunkSize - 1;

    const response = await fetch("/download", {
      headers: {
        "Range": `bytes=${start}-${end}`
      }
    });

    if (response.status !== 206) {
      throw new Error("Server does not support partial content");
    }

    const contentRange = response.headers.get("Content-Range");
    // bytes 0-1048575/73400320

    if (!totalSize) {
      totalSize = parseInt(contentRange.split("/")[1], 10);
    }

    const blob = await response.blob();
    chunks.push(blob);

    start += blob.size;

    // 👉 ĐIỀU KIỆN TẢI XONG
    if (start >= totalSize) {
      break;
    }
  }

  // Gộp các chunk lại thành 1 file
  const finalBlob = new Blob(chunks);
  return finalBlob;
}
```

### Multitenant web APIs

- A multitenant web API solution is shared by multiple tenants, such as distinct organizations that have their own groups of users.
- Multitenancy directly impacts:
  - API design
  - Resource access
  - Authentication & authorization
  - Routing and scalability
  - Caching and security
- Designing for multitenancy from the beginning helps avoid expensive refactoring later.
- Common tenant identification mechanisms:
  - Subdomain / domain
  - HTTP headers
  - JWT claims
  - URI path

#### Subdomain or Domain-Based Isolation (DNS-Level Tenancy)

- What is a Wildcard Domain?

  - A wildcard domain is a DNS configuration that allows any subdomain of a domain to point to the same server, without creating each subdomain manually.

- What is a **CNAME** in DNS?

  - A **CNAME** (Canonical Name) record in DNS is used to make one domain name act as an alias of another domain name.

  - Instead of pointing a domain directly to an IP address, a CNAME points to another domain name, and DNS will then resolve that name to its IP.

Instead of defining:

```
api.example.com
admin.example.com
user1.example.com
```

You define one DNS rule:

```
*.example.com
```

- Subdomain or domain-based isolation routes requests using tenant-specific domains.
  Wildcard subdomains provide a simple and scalable way to support multiple tenants with minimal DNS configuration, while custom domains allow tenants to use their own branded domains for greater flexibility and control.

- This approach depends on correct DNS configuration (`A`and `CNAME` records) to route traffic to the appropriate infrastructure. Preserving the original hostname between reverse proxies and backend services is important to prevent incorrect redirects and to avoid exposing internal URLs.

- DNS-based routing also plays a key role in data residency, regional routing, and regulatory compliance by ensuring requests are resolved to the correct geographic infrastructure.

Example:

```
GET https://adventureworks.api.contoso.com/orders/3
```

#### Pass tenant-specific HTTP headers

- Tenant information can be passed through

  - Custom HTTP headers like `X-Tenant-ID` or `X-Organization-ID`
  - Through host-based headers like `Host` or `X-Forwarded-Host`
  - Can be extracted from `JSON Web Token (JWT)` claims

- The choice depends on the routing capabilities of your API gateway or reverse proxy, with header-based solutions requiring a Layer 7 (L7) gateway to inspect each request.

- Advantages

  - It enables centralized authentication, which simplifies security management across multitenant APIs
  - By using SDKs or API clients, tenant context is dynamically managed at runtime, which reduces client-side configuration complexit
  - tenant context in headers results in a cleaner, more RESTful API design by avoiding tenant-specific data in the URI.

- Disadvantages

  - This requirement adds processing overhead, which increases compute costs when traffic scales
  - It complicates caching, particularly when cache layers rely solely on URI-based keys and don't account for headers

```
GET https://api.contoso.com/orders/3
X-Tenant-ID: adventureworks
```

```
GET https://api.contoso.com/orders/3
Host: adventureworks
```

```
GET https://api.contoso.com/orders/3
Authorization: Bearer <JWT-token including a tenant-id: adventureworks claim>
```

#### Pass tenant-specific information through the URI path

- This approach appends tenant identifiers within the resource hierarchy and relies on the API gateway or reverse proxy to determine the appropriate tenant based on the path segment. Path-based isolation is effective, but it compromises the web API's RESTful design and introduces more complex routing logic. It often requires pattern matching or regular expressions to parse and canonicalize the URI path.

```
GET https://api.contoso.com/tenants/adventureworks/orders/3
```

### CACHING

```
from flask import Flask, jsonify
from flask_caching import Cache
import time

app = Flask(__name__)

# Cấu hình cache (SimpleCache dùng cho demo)
app.config["CACHE_TYPE"] = "SimpleCache"
app.config["CACHE_DEFAULT_TIMEOUT"] = 60  # seconds

cache = Cache(app)


@app.get("/users/<int:user_id>")
@cache.cached(timeout=30)  # cache response 30s
def get_user(user_id):
    # Giả lập truy vấn DB chậm
    time.sleep(3)

    return jsonify({
        "id": user_id,
        "name": "John",
        "cached_at": time.time()
    })


if __name__ == "__main__":
    app.run(debug=True)

```

## Security

### JSON Web Token

#### When should you use JSON Web Tokens?

- Authorization: This is the most common scenario for using JWT
- Information Exchange: JSON Web Tokens are a good way of securely transmitting information between parties

#### What is the JSON Web Token structure?

- JSON Web Tokens consist of three parts separated by dots (.), which are:
  - Header: Contains metadata about the token, such as the algorithm used for signing.
  - Payload: Stores the claims, i.e., data being transmitted.
  - Signature: Ensures the token's integrity and authenticity
- Therefore, a JWT typically looks like the following:

```
xxxxx.yyyyy.zzzzz
```

#### Header

- The header contains metadata about the token, including the signing algorithm and token type here metadata means data about data.
  - alg: Algorithm used for signing (e.g., HS256, RS256).
  - typ: Token type, always "JWT".

```
{
    "alg": "HS256",
    "typ": "JWT"
}
```

#### Payload

- The payload contains the information about the user also called as a claim and some additional information including the timestamp at which it was issued and the expiry time of the token.
- Common claim types
  - iss (Issuer): Identifies who issued the token.
  - sub (Subject): Represents the user or entity the token is about.
  - aud (Audience): Specifies the intended recipient.
  - exp (Expiration): Defines when the token expires.
  - iat (Issued At): Timestamp when the token was created.
  - nbf (Not Before): Specifies when the token becomes valid.

```
{
    "userId": 123,
    "role": "admin",
    "exp": 1672531199
}
```

#### Signature

- The signature ensures token integrity and is generated using the header, payload, and a secret key.

#### Example

##### ALGORITHM - HS256

**ENCODE**

```
import jwt
from datetime import datetime, timedelta

# Secret key (phải giữ bí mật)
SECRET_KEY = "my-secret-key"
ALGORITHM = "HS256"

# Payload (claims)
payload = {
    "sub": "22011995",
    "username": "Đào An Sơn",
    "role": "admin",
    "iat": datetime.utcnow(),                      # issued at
    "exp": datetime.utcnow() + timedelta(minutes=30)  # expires in 30 minutes
}

# Tạo JWT
token = jwt.encode(payload, SECRET_KEY, algorithm=ALGORITHM)

print(token)
```

**DECODE**

```
try:
    decoded = jwt.decode(
        token,
        SECRET_KEY,
        algorithms=["HS256"]
    )
    print(decoded)
except jwt.ExpiredSignatureError:
    print("Token has expired")
except jwt.InvalidTokenError:
    print("Invalid token")

```

##### ALGORITHM - RS256

```
Private key  ──(encode / sign)──▶  JWT  ──(verify)──▶  Public key
```

- Step 1: Generate an RSA key pair (one-time setup)

```
openssl genrsa -out private.pem 2048
```

| Component          | Explanation                                                     |
| ------------------ | --------------------------------------------------------------- |
| `openssl`          | Runs the OpenSSL command-line tool.                             |
| `genrsa`           | Generates an RSA private key.                                   |
| `-out private.pem` | Saves the generated private key to a file named `private.pem`.  |
| `2048`             | Specifies the key size in bits.                                 |
| _(note)_           | 2048 bits is the minimum recommended secure size for RSA today. |

```
openssl rsa -in private.pem -pubout -out public.pem
```

| Component         | Explanation                                            |
| ----------------- | ------------------------------------------------------ |
| `openssl`         | Runs the OpenSSL command-line tool.                    |
| `rsa`             | Works with RSA keys (view, convert, or extract parts). |
| `-in private.pem` | Uses the private key file `private.pem` as input.      |
| `-pubout`         | Extracts the public key from the private key.          |
| `-out public.pem` | Writes the public key to a file named `public.pem`.    |

- Step 2: Encode JWT by RS256 (Python)

**Install library**

```
pip install PyJWT cryptography
```

**Encode**

```
import jwt
from datetime import datetime, timedelta

# Đọc private key
with open("private.pem", "r", encoding="utf-8") as f:
    private_key = f.read()

# Payload
payload = {
    "sub": "22011995",
    "name": "Dao An Son",
    "role": "admin",
    "iat": datetime.utcnow(),
    "exp": datetime.utcnow() + timedelta(minutes=30)
}

# Encode JWT (RS256)
token = jwt.encode(
    payload,
    private_key,
    algorithm="RS256"
)

print("JWT token:")
print(token)
```

**Decode and Verify JWT by Public Key**

```
import jwt

# Đọc public key
with open("public.pem", "r", encoding="utf-8") as f:
    public_key = f.read()

token = "<DÁN TOKEN VỪA TẠO VÀO ĐÂY>"

try:
    decoded = jwt.decode(
        token,
        public_key,
        algorithms=["RS256"]
    )
    print("Decoded payload:")
    print(decoded)

except jwt.ExpiredSignatureError:
    print("❌ Token đã hết hạn")

except jwt.InvalidTokenError as e:
    print("❌ Token không hợp lệ:", str(e))
```

### Transport Layer Security

- Transport Layer Security (TLS) is a security protocol that protects data while it is being transmitted over a network. It provides encryption, integrity, and authentication between a client (like a browser) and a server.

## Documentation

### OpenAPI Specification

- The OpenAPI Specification (OAS), formerly known as Swagger, is the most popular tool for documenting HTTPbased APIs

## Trade-Offs

### Advantages

| Aspect                    | Description                                                                                                                                                                                                                                                                                                                                     |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Scalability**           | REST was designed for internet-scale systems. Its stateless nature allows each request to be handled independently, which makes horizontal scaling easier and more efficient.                                                                                                                                                                   |
| **Interoperability**      | REST relies on standardized HTTP, which is supported by most programming languages, libraries, and platforms. This allows systems built with different technologies to communicate easily. Statelessness also simplifies integration across applications and services.                                                                          |
| **Debugging and Tooling** | REST APIs commonly use human-readable formats such as JSON or XML, making debugging and data inspection easier than binary protocols like Protocol Buffers. REST is also supported by a rich ecosystem of tools, including servers, caches, load balancers, proxies, firewalls, monitoring systems, testing tools, and documentation utilities. |

### Disadvantages

| Aspect                           | Description                                                                                                                                                                                                                                                              |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Limited Vocabulary**           | REST relies on a small set of HTTP verbs (GET, POST, PUT, DELETE, etc.). These verbs may be insufficient to clearly express complex domain-specific operations that do not map well to standard HTTP semantics.                                                          |
| **Security Challenges**          | The optional _code-on-demand_ constraint allows servers to send executable code to clients. If misused, this can introduce security risks such as cross-site scripting (XSS), injection attacks, and unauthorized data access.                                           |
| **Limitations of Statelessness** | Because REST is stateless, each request must contain all required information. This can increase payload size and makes REST less suitable for stateful workflows (e.g., shopping carts). State management is often pushed to the client using cookies or local storage. |
| **No Standardized Versioning**   | REST does not define a built-in versioning mechanism. Clients are sensitive to API changes, and frequent version updates can cause breaking changes. While HATEOAS can reduce coupling, it does not eliminate the need for clients to understand API semantics.          |
