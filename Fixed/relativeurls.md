# Relative Urls

Various elements in the OpenAPI spec are defined as being URLs.  Many of these properties also support relative references.  How these relative references are resolved depends on a number of factors.

To resolve relative URLs it is first necessary to idenfify the base path defined by the selected Server Url.  If the Server Urls are relative, then the location of the OpenAPI definition must be considered.

## Relative Server URLs

 ```yaml
openapi: 3.0.0
info:
  title: Example with relative server URL
  version: 1.0.0
  termsOfService: eula.html
servers:
- url: /api
- url: /staging/api
- url: ../sandbox/api
- url: misplaced/api
paths: {}
components:
  parameters:
    top: 
      $ref: `/specs/commonparameters.yaml#/components/parameters/top`
 ```

For the above definition, located at https://example.org/specs/openapi.yaml, the following URLs are valid:

#### Resolved Server URLs:
```
https://example.org/api
https://example.org/staging/api
https://example.org/sandbox/api
https://example.org/specs/misplaced/api
```

#### Resolved TermsOf Service Urls:
```
https://example.org/api/eula.html
https://example.org/staging/api/eula.html
https://example.org/sandbox/api/eula.html
https://example.org/specs/misplaced/api/eula.html
```

#### Resolved reference object:
```
https://example.org/specs/commonparameters.yaml#/components/parameters/top
```