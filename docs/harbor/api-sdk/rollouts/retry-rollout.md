> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Retry Rollout

> Retry selected latest terminal attempts after cleanup. Returns new IDs in input order.



## OpenAPI

````yaml /api-sdk/openapi.json post /rollouts/retry
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
  /rollouts/retry:
    post:
      tags:
        - Rollouts
      summary: Retry Rollout
      description: >-
        Retry selected latest terminal attempts after cleanup. Returns new IDs
        in input order.
      operationId: retry_rollout_rollouts_retry_post
      parameters:
        - name: Idempotency-Key
          in: header
          required: false
          schema:
            anyOf:
              - type: string
                minLength: 1
                maxLength: 200
              - type: 'null'
            title: Idempotency-Key
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
              $ref: '#/components/schemas/RolloutActionRequest'
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/RetryResponse'
        default:
          $ref: '#/components/responses/ProblemDetails'
      x-codeSamples:
        - lang: python
          label: Harbor SDK
          source: |
            from harbor import Harbor

            client = Harbor()
            result = client.rollouts.retry("ROLLOUT_ID")
            print(result)
            client.close()
components:
  schemas:
    RolloutActionRequest:
      properties:
        rollout_ids:
          items:
            type: string
            format: uuid
          type: array
          minItems: 1
          title: Rollout Ids
      additionalProperties: false
      type: object
      required:
        - rollout_ids
      title: RolloutActionRequest
    RetryResponse:
      properties:
        rollout_ids:
          items:
            type: string
            format: uuid
          type: array
          title: Rollout Ids
      type: object
      required:
        - rollout_ids
      title: RetryResponse
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