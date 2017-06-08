# The simple Webhook 
In this example API, the operation allows a client to subscribe to a webhook by POSTing a JSON representation that has the callback URL as a property of the response body.  The operation has a `callback` object called `mainHook` that describes the HTTP request that will be made when the webhook is triggered by the origin server.

```yaml
openapi: 3.0.0
info:
  title: A simple webhook subscription
  version: 1.0.0
```

### Subscription Operation (`#simpleSubscribe`)
For a client to tell the server that it wishes to receive a webhook callback it must first subscribe.  The subscribe operation is described like any other OpenAPI operation.

``` yaml
paths: 
  '/simple/subscribe':
    post:
      operationId: simpleSubscribe
      requestBody:
        content:
          application/json: 
            schema:
              type: object
              properties:
                url: 
                  type: string
                  format: uri 
      responses:
        '201':
          description: Created subscription to webhook
```
The one additional requirement is that the client send to the server a URL to be used for the webhook callback.  The API designer can choose where that URL is passed.  It could be in a query parameter, the body or even a header.  In this example it is a property of a JSON object in the body.

### Callback 

In order to describe to the webhook consumer what to expect, a `callback` object can be added to the subscribe operation.

``` yaml
paths:
  '/simple/subscribe':
    post:
      callbacks:
        mainHook:
          '$request.body#/url':
            post:
              requestBody:
                content:
                  application/json:
                    schema:
                      type: object
                      properties:
                        message:
                          type: string
              responses:
                '200':
                  description: webhook successfully processed
```
