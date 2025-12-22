- [REST API](#rest-api)
  - [WHAT IS THE REST API](#what-is-the-rest-api)
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
- [The Six Constraints](#the-six-constraints)
  - [Uniform Interface](#uniform-interface)
  - [Stateless](#stateless)
  - [Cacheable](#cacheable)
  - [Client-Server](#client-server)
  - [Layered System](#layered-system)
  - [Code on Demand](#code-on-demand)

# REST API

## WHAT IS THE REST API

- Technically, **REST** is an acronym for **“REpresentational State Transfer”** which is simply an architectural style originally written about by **Roy Fielding** in his doctoral dissertation

- Makes an HTTP request to a URL…
  - Using one of the standard HTTP methods (GET, PUT, POST, PATCH, DELETE, etc.)…
  - With some content (usually JSON) in the body…
- And waits for a response, which:
  - Indicates status via an HTTP response code
  - And usually has more JSON in the body.

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

## Responses HTTP codes

### 1xx — Informational

| Code    | Meaning             |
| ------- | ------------------- |
| **100** | Continue            |
| **101** | Switching Protocols |
| **102** | Processing          |

### 2xx — Success

| Code    | Meaning                       |
| ------- | ----------------------------- |
| **200** | OK                            |
| **201** | Created                       |
| **202** | Accepted                      |
| **203** | Non-Authoritative Information |
| **204** | No Content                    |
| **205** | Reset Content                 |
| **206** | Partial Content               |

### 3xx — Redirection

| Code    | Meaning                    |
| ------- | -------------------------- |
| **300** | Multiple Choices           |
| **301** | Moved Permanently          |
| **302** | Found (Temporary Redirect) |
| **303** | See Other                  |
| **304** | Not Modified               |
| **307** | Temporary Redirect         |
| **308** | Permanent Redirect         |

### 4xx — Client Errors

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

### 5xx — Server Errors

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

# The Six Constraints

- The REST architectural style describes six constraints. These constraints, applied to the architecture, were originally communicated by Roy Fielding in his doctoral dissertation and defines the basis of RESTful-style

## Uniform Interface

- The uniform interface constraint defines the interface between clients and servers

## Stateless

## Cacheable

## Client-Server

## Layered System

## Code on Demand
