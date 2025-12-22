- [REST API](#rest-api)
  - [WHAT IS THE REST API](#what-is-the-rest-api)
  - [URL - Uniform Resource Locator](#url---uniform-resource-locator)
- [The Six Constraints](#the-six-constraints)

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

```
http://www.example.com:80/blog/2025/search?tag=api&sort=date#latest
│───│   │   │        │ │ │                 │               │
│   │   │   │        │ │ │                 │               └─ Fragment
│   │   │   │        │ │ │                 └─ Fragment identifier (#)
│   │   │   │        │ │ └─ Query parameters (tag=api&sort=date)
│   │   │   │        │ └─ Query separator (?)
│   │   │   │        └─ Path (/blog/2025/search)
│   │   │   └─ Port (80)
│   │   │
│   │   └─ Domain (example.com)
│   │
│   └─ Subdomain (www)
│
└─ Scheme (http)

```

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

# The Six Constraints
