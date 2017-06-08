# The Enhanced Webhook 
In this example we have a subscribe operation to register for callbacks that is also linked to a second operation that can be used to unsubscribe.

```yaml
openapi: 3.0.0
info:
  title: An enhanced webhook subscription
  version: 1.0.0
```

## The Subscribe Operation
The subscribe operation is similar to that of the simple example, but in the return payload a webhook identifier is returned.  The client will use this identifier as a parameter to the unsubscribe operation.

```yaml
paths:
  '/hooks':
    post:
      requestBody:    
        description: body provided by consumer with callback URL
        required: true
        content:
          application/json:
            example:
              callback-url: https://consumer.com/hookcallback        
      responses:      
        '201':
          description: Successfully subscribed
          content:
            application/json:
              example:
                hookId: 23432-32432-45454-97980
```
An example of calling this operation would be:

```http
POST http://example.org/hooks HTTP/1.1
Content-Type: application/json

{
  "callback-url" : "https://consumer.com/hookcallback"
}
```
And the response would be:

```http
201 Created
Content-Type: application/json

{
  "hookId": "23432-32432-45454-97980"
}
```

## The Callback Description
The callback description is the same as for the simple case.

```yaml
paths:
  '/hooks':
    post:
      callbacks:
        hookEvent:
          '$request.body#/callback-url':
            post:
              requestBody:
                content:
                  application/json:
                    example:
                      message: Here is the event
              responses:
                '200':
                  description: Expected responses from callback consumer
```



## The Unsubscribe Link
The subscription response also contains a links map that has an `unsubscribe` link object.

```yaml
paths:
  '/hooks':
    post:
      responses:      
        '201':
          links:
            unsubscribe:
              operationId: cancelHookCallback
              parameters: 
                id: $response.body#/hookId
```
The link parameters map contains the mapping between the unsubscribe operation parameter Id and the hookId returned by the subscribe response. 

## The Unsubscribe Operation
The unsubscribe operation is a regular operation that also happens to be a target of a link object.

```yaml
paths:
  '/hooks/{id}':
    delete:
      operationId: cancelHookCallback
      parameters:
        name: id
        in: path
        schema: 
          type: string
      responses:
        '200':
          description: Successfully cancelled callback
```
