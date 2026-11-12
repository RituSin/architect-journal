# Chapter 4: HTTP, REST and APIs

## HTTP in one paragraph

HTTP is a conversation of two message types: a request from the client and a response from the server. The server never speaks first. HTTP is stateless: the server forgets you after answering, so every request must carry everything needed, such as a token.

## A request has four parts

Method (what to do), path (which thing), headers (facts about the request) and an optional body. Example: GET /users/42 with an Authorization header.

## A response has three parts

Status line (a number and a word), headers, and a body.

## Methods

GET reads. POST creates or triggers an action. PUT replaces. PATCH changes part. DELETE removes. GET, PUT and DELETE are safe to repeat (idempotent). POST is not.

## Status codes

2xx worked: 200 OK, 201 Created, 204 No Content. 3xx moved: 301 permanent, 304 cached copy is fine. 4xx the client made a mistake: 400 bad request, 401 not logged in, 403 logged in but not allowed, 404 not found, 409 conflict, 429 too many requests. 5xx the server failed: 500 server bug, 503 overloaded or down.

## Headers worth knowing

Content-Type is the body format. Authorization carries who you are. Cache-Control says how long a response may be reused. Cookie and Set-Cookie let a stateless server remember a login. Access-Control-Allow-Origin is the CORS header: the browser blocks a page from reading a response from another origin unless the server allows it. For complicated requests the browser first sends an OPTIONS request, called a preflight, to ask permission.

## REST rules

Resources are nouns in the URL. The method is the verb. Responses are representations, usually JSON. Each request is self-contained. Use status codes honestly.

## A batch endpoint and status codes

When one request holds many items and only some fail, no single status is honest. One common choice is HTTP 200 with a per-item result in the body. The cost is that monitoring based on error status codes will not see item failures, so such an endpoint needs its own failure metrics.
