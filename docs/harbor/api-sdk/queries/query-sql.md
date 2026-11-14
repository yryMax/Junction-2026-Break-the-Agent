> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Query Sql

> Run read-only SQL on public views. Supply statements as SQL strings or [SQL, parameters...]. Use LIMIT for large results.



## OpenAPI

````yaml /api-sdk/openapi.json post /query
openapi: 3.1.0
info:
  title: Harbor API
  version: '2026-10-30'
  description: The harbor serve API.
servers:
  - url: http://127.0.0.1:{port}
    description: Local server
    variables:
      port:
        default: '8390'
  - url: https://{host}
    description: Remote server
    variables:
      host:
        default: your-server.example.com
security:
  - {}
  - BearerAuth: []
tags:
  - name: Jobs
  - name: Rollouts
  - name: Files
  - name: Queries
  - name: Agents
  - name: Sandboxes
  - name: Hub
paths:
  /query:
    post:
      tags:
        - Queries
      summary: Query Sql
      description: >-
        Run read-only SQL on public views. Supply statements as SQL strings or
        [SQL, parameters...]. Use LIMIT for large results.
      operationId: query_sql_query_post
      parameters:
        - name: Harbor-API-Version
          in: header
          required: false
          schema:
            type: string
            default: '2026-10-30'
            title: Harbor-Api-Version
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/QueryRequest'
            example:
              - SELECT state, count(*) AS n FROM rollout GROUP BY state
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/QueryResponse'
        default:
          $ref: '#/components/responses/ProblemDetails'
      x-codeSamples:
        - lang: python
          label: Harbor SDK
          source: >
            from harbor import Harbor


            client = Harbor()

            result = client.query_sql(["SELECT state, count(*) AS n FROM rollout
            GROUP BY state"])

            print(result)

            client.close()
components:
  schemas:
    QueryRequest:
      items:
        $ref: '#/components/schemas/SqlStatement'
      type: array
      maxItems: 16
      minItems: 1
      title: QueryRequest
    QueryResponse:
      properties:
        results:
          items:
            anyOf:
              - $ref: '#/components/schemas/QueryRows'
              - $ref: '#/components/schemas/QueryError'
          type: array
          title: Results
      type: object
      required:
        - results
      title: QueryResponse
    SqlStatement:
      anyOf:
        - type: string
        - items:
            anyOf:
              - $ref: '#/components/schemas/SqlValue'
              - additionalProperties:
                  $ref: '#/components/schemas/SqlValue'
                type: object
          type: array
    QueryRows:
      properties:
        rows:
          items:
            additionalProperties:
              $ref: '#/components/schemas/SqlValue'
            type: object
          type: array
          title: Rows
      type: object
      required:
        - rows
      title: QueryRows
    QueryError:
      properties:
        error_v2:
          $ref: '#/components/schemas/QueryErrorInfo'
      type: object
      required:
        - error_v2
      title: QueryError
    SqlValue:
      anyOf:
        - type: string
        - type: integer
        - type: number
        - type: 'null'
    QueryErrorInfo:
      properties:
        message:
          type: string
          title: Message
        sqlite:
          anyOf:
            - $ref: '#/components/schemas/SQLiteError'
            - type: 'null'
      type: object
      required:
        - message
      title: QueryErrorInfo
    SQLiteError:
      properties:
        code:
          type: integer
          title: Code
        extended_code:
          type: integer
          title: Extended Code
      type: object
      required:
        - code
        - extended_code
      title: SQLiteError
  responses:
    ProblemDetails:
      description: Request failed (RFC 9457).
      content:
        application/problem+json:
          schema:
            additionalProperties: true
            description: >-
              RFC 9457 response, with Hub's error tag and optional extension
              fields.
            properties:
              type:
                const: about:blank
                default: about:blank
                title: Type
                type: string
              title:
                title: Title
                type: string
              status:
                title: Status
                type: integer
              detail:
                title: Detail
                type: string
              _tag:
                title: Tag
                type: string
            required:
              - title
              - status
              - detail
              - _tag
            title: ProblemDetails
            type: object
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      description: Required only when the server has HARBOR_API_KEY configured.

````

This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.