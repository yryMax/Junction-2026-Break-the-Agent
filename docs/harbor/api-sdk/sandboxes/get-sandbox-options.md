> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Sandbox Options

> Get the JSON Schema for a built-in sandbox provider's user-facing kwargs.



## OpenAPI

````yaml /api-sdk/openapi.json get /sandboxes/{name}/options
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
  /sandboxes/{name}/options:
    get:
      tags:
        - Sandboxes
      summary: Get Sandbox Options
      description: >-
        Get the JSON Schema for a built-in sandbox provider's user-facing
        kwargs.
      operationId: get_sandbox_options_sandboxes__name__options_get
      parameters:
        - name: name
          in: path
          required: true
          schema:
            type: string
            title: Name
          example: docker
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
                type: object
                additionalProperties: true
                title: Response Get Sandbox Options Sandboxes  Name  Options Get
              example:
                additionalProperties: false
                description: User-facing kwargs for DockerEnvironment.
                properties:
                  auto_mounting:
                    default: false
                    description: >-
                      Automatically bind Harbor output directories on local
                      providers.
                    title: Auto Mounting
                    type: boolean
                  suppress_override_warnings:
                    default: false
                    deprecated: true
                    description: >-
                      Deprecated; resource override warnings are no longer
                      emitted.
                    title: Suppress Override Warnings
                    type: boolean
                  keep_containers:
                    default: false
                    title: Keep Containers
                    type: boolean
                title: DockerOptions
                type: object
        default:
          $ref: '#/components/responses/ProblemDetails'
      x-codeSamples:
        - lang: python
          label: Harbor SDK
          source: |
            from harbor import Harbor

            client = Harbor()
            result = client.sandboxes.options("docker")
            print(result)
            client.close()
components:
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