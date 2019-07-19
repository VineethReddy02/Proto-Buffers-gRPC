# Proto-Buffers-gRPC
FIndings and Explorations in protocol buffers and gRPC

## Limitations with HTTP/1.1

1. HTTP 1.1 opens a new TCP connection to a server at each request.
2. It does not compress headers (which are plaintext).
3. It only works with Request/Response mechanism (no server push).
4. These inefficiences add more latency and increase network packet size.

## Advantages with HTTP/2

1. The client & server can push messages in parallel over the same TCP connection
2. This greatly reduces latency.
3. Server can push streams (multiple messages) for one request from the client which will reduce the round trips between
   client and server.
4. HTTP/2 supports header compressions, This have much less impact on packet size(less bandwidth).
5. Average http request may have over 20 headers, due to cookies,content cache and application headers.
6. HTTP/2 is binary while HTTP/1 is text which is not efficient over the network.
7. Protocol buffers is a binary protocol and makes it a great match for HTTP/2
8. HTTP/2 is secure (SSL is not required but recommended by default)

## Types of API in gRPC

1. Unary (default client to server request/response based communication).
2. Server Streaming (client request server and server responds back with stream of responses).
3. Client Streaming (client creates a streaming request connection with server and server sends back a single response).
4. Bi Directional Streaming (client sends a stream of requests to server and server sends back stream of responses 
   back to client) 
   
## Scalability in gRPC

1. gRPC servers are asynchronous by default
2. This means they do not block threads on request
3. Therefore each gRPC server can serve millions of requets in parallel
4. gRPC Clients can be asynchronous or synchronous (blocking)
5. The client decides which model works best for the performance needs
6. gRPC Clients can perform client side load balancing

## gRPC vs REST

### gRPC

1. Protocol Buffers - smaller,faster
2. HTTP/2(lower latency) 
3. Bidirectional & Async
4. Stream Support
5. API oriented (no constraints-free design)
6. Code Generation through Protocol Buffers in any language-1st  class citizen
7. RPC based- gRPC does the plumbing for us

### REST

1. JSON- text based,slower, bigger
2. HTTP1.1 (higher latency)
3. Client -> Server requets only
4. Request/Response support only
5. CRUD oriented(Create-Retrieve-Update-Delete/POST GET PUT DELETE)
6. Code generation through OpenAPI/Swagger(add-on)-2nd class citizen
7. HTTP verns based- we have to write the plumbing or use a 3rd party library
