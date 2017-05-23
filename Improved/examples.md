# Examples


# Multiple Media Types and Multiple Examples

```yaml
openapi: 3.0.0
info:
  title: Examples of the new example object
  version: 1.0.0
paths:
  '/names':
    get:
      responses:
        '200':
          content:
            'application/json': 
              schema:
                type: array
                items:
                  type: string
              examples:
                list:
                  summary: List of Names
                  description: |- 
                                    A long and _very_ detailed description of this representation that includes rich text.
                  value:
                    - Bob
                    - Diane
                    - Mary
                    - Bill
                empty:
                  summary: Empty
                  value: {}
            'application/xml': 
              examples:
                list:
                  summary: List of names
                  value: "<Users><User name='Bob'/><User name='Diane'/><User name='Mary'/><User name='Bill'/></Users>"
                empty:
                  summmary: Empty list
                  value: "<Users/>"
            'text/plain':
              examples:
                list:
                  summary: List of names
                  value: "Bob,Diane,Mary,Bill"
                empty:
                  summary: Empty
                  value: ""
```

# Referencing Examples

```yaml
openapi: 3.0.0
info:
  title: Examples of the new example object using a $ref and a external link
  version: 1.0.0
paths:
  '/pets':
    get:
      responses:
        '200':
          application/json: 
            schema:
              $ref: '#/components/schemas/Pet'
            examples:
              dog:
                summary: An example of a dog with a cat's name
                value:
                  name: Puma
                  petType: Dog
                  color: Black
                  gender: Female
                  breed: Mixed
              frog:
                $ref: '#/components/examples/frog-example'
              cat:
                summary: An example of a cat
                externalValue: '/examples/cat.json'
```