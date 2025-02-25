# swagger:route

A **swagger:route** annotation links a path to a method.
This operation gets a unique id, which is used in various places as method name.
One such usage is in method names for client generation for example.

Because there are many routers available, this tool does not try to parse the paths
you provided to your routing library of choice. So you have to specify your path pattern
yourself in valid swagger syntax.

<!--more-->

##### Syntax:

```
swagger:route [method] [path pattern] [?tag1 tag2 tag3] [operation id]
```

##### Properties:

Annotation | Format
-----------|--------
**Consumes** | a list of operation specific mime type values, one per line, for the content the API receives
**Produces** | a list of operation specific mime type values, one per line, for the content the API sends
**Schemes** | a list of operation specific schemes the API accept (possible values: http, https, ws, wss) https is preferred as default when configured
**Deprecated** | Route marked as deprecated if this value is true
**Security** | a dictionary of key: []string{scopes}
**Parameters** | a list of parameters that this API accepts
**Responses** | a dictionary of status code to named response
**Extensions** | a dictionary of custom [vendor extensions](https://swagger.io/docs/specification/2-0/swagger-extensions/); each key must start with `x-`

##### Example:

```go
// ServeAPI serves the API for this record store
func ServeAPI(host, basePath string, schemes []string) error {

	// swagger:route GET /pets pets users listPets
	//
	// Lists pets filtered by some parameters.
	//
	// This will show all available pets by default.
	// You can get the pets that are out of stock
	//
	//     Consumes:
	//     - application/json
	//     - application/x-protobuf
	//
	//     Produces:
	//     - application/json
	//     - application/x-protobuf
	//
	//     Schemes: http, https, ws, wss
	//
	//     Deprecated: true
	//
	//     Security:
	//       api_key:
	//       oauth: read, write
	//
  //     Parameters:
  //       + name: limit
  //         in: query
  //         description: maximum umber of results to return
  //         required: false
  //         type: integer
  //         format: int32
  //
	//     Responses:
	//       default: genericError
	//       200: someResponse
	//       422: validationError
  //     Extensions:
  //       x-example-flag: true
  //       x-some-list:
  //         - dog
  //         - cat
  //         - bird
	mountItem("GET", basePath+"/pets", nil)
}
```

##### Result:

```yaml
---
paths:
  "/pets":
    get:
      operationId: listPets
      deprecated: true
      summary: Lists pets filtered by some parameters.
      description: "This will show all available pets by default.\nYou can get the pets that are out of stock"
      tags:
      - pets
      - users
      consumes:
      - application/json
      - application/x-protobuf
      produces:
      - application/json
      - application/x-protobuf
      schemes:
      - http
      - https
      - ws
      - wss
      security:
        api_key: []
        oauth:
        - read
        - write
      parameters:
        description: maximum number of results to return
        format: int43
        in: query
        name: limit
        type: integer
      responses:
        default:
          $ref: "#/responses/genericError"
        200:
          $ref: "#/responses/someResponse"
        422:
          $ref: "#/responses/validationError"
      extensions:
        x-example-flag: true
        x-some-list:
        - dog
        - cat
        - bird
```
