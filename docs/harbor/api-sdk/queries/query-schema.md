> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Query Schema

> Discover public SQL views and columns before using query_sql; optionally select one view.



## OpenAPI

````yaml /api-sdk/openapi.json get /query/schema
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
  /query/schema:
    get:
      tags:
        - Queries
      summary: Query Schema
      description: >-
        Discover public SQL views and columns before using query_sql; optionally
        select one view.
      operationId: query_schema_query_schema_get
      parameters:
        - name: view
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                maxLength: 100
              - type: 'null'
            title: View
        - name: Harbor-API-Version
          in: header
          required: false
          schema:
            type: string
            default: '2026-10-30'
            title: Harbor-Api-Version
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/SchemaResponse'
        default:
          $ref: '#/components/responses/ProblemDetails'
      x-codeSamples:
        - lang: python
          label: Harbor SDK
          source: |
            from harbor import Harbor

            client = Harbor()
            result = client.query_schema(view="rollout")
            print(result)
            client.close()
components:
  schemas:
    SchemaResponse:
      properties:
        views:
          items:
            $ref: '#/components/schemas/SchemaView'
          type: array
          title: Views
      type: object
      required:
        - views
      title: SchemaResponse
    SchemaView:
      properties:
        name:
          type: string
          title: Name
        columns:
          items:
            $ref: '#/components/schemas/SchemaColumn'
          type: array
          title: Columns
      type: object
      required:
        - name
        - columns
      title: SchemaView
    SchemaColumn:
      properties:
        name:
          type: string
          title: Name
        type:
          anyOf:
            - type: string
            - type: 'null'
          title: Type
      type: object
      required:
        - name
        - type
      title: SchemaColumn
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