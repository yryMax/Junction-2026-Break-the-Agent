> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Job Filters

> Discover available job filter values and counts, optionally narrowed by other filters.



## OpenAPI

````yaml /api-sdk/openapi.json get /jobs/filters
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
  /jobs/filters:
    get:
      tags:
        - Queries
      summary: Job Filters
      description: >-
        Discover available job filter values and counts, optionally narrowed by
        other filters.
      operationId: job_filters_jobs_filters_get
      parameters:
        - name: field
          in: query
          required: true
          schema:
            type: string
            enum:
              - state
              - task
              - agent_name
              - agent_version
              - model_name
              - model_provider
              - reasoning_effort
              - source
            title: Field
        - name: limit
          in: query
          required: false
          schema:
            type: integer
            maximum: 1000
            minimum: 1
            description: Maximum choices.
            default: 100
            title: Limit
          description: Maximum choices.
        - name: task
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
              minLength: 1
              maxLength: 4096
            maxItems: 100
            description: >-
              Task names across versions, or name@sha256:<full digest> pins.
              Repeated values use OR.
            title: Task
          description: >-
            Task names across versions, or name@sha256:<full digest> pins.
            Repeated values use OR.
        - name: state
          in: query
          required: false
          schema:
            anyOf:
              - enum:
                  - queued
                  - running
                  - paused
                  - cancelling
                  - finished
                  - cancelled
                type: string
              - type: 'null'
            title: State
        - name: name
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                maxLength: 200
              - type: 'null'
            title: Name
        - name: q
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                maxLength: 200
              - type: 'null'
            title: Q
        - name: agent_name
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
            maxItems: 100
            title: Agent Name
        - name: agent_version
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
            maxItems: 100
            title: Agent Version
        - name: model_name
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
            maxItems: 100
            title: Model Name
        - name: model_provider
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
            maxItems: 100
            title: Model Provider
        - name: reasoning_effort
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
            maxItems: 100
            title: Reasoning Effort
        - name: source
          in: query
          required: false
          schema:
            type: array
            items:
              type: string
            maxItems: 100
            title: Source
        - name: created_after
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: date-time
              - type: 'null'
            title: Created After
        - name: created_before
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: date-time
              - type: 'null'
            title: Created Before
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
                $ref: '#/components/schemas/FilterValuesResponse'
        default:
          $ref: '#/components/responses/ProblemDetails'
      x-codeSamples:
        - lang: python
          label: Harbor SDK
          source: |
            from harbor import Harbor

            client = Harbor()
            result = client.jobs.filters("agent_name")
            print(result)
            client.close()
components:
  schemas:
    FilterValuesResponse:
      properties:
        field:
          type: string
          enum:
            - state
            - task
            - agent_name
            - agent_version
            - model_name
            - model_provider
            - reasoning_effort
            - source
          title: Field
        values:
          items:
            $ref: '#/components/schemas/FilterOption'
          type: array
          title: Values
        truncated:
          type: boolean
          title: Truncated
          default: false
      type: object
      required:
        - field
        - values
      title: FilterValuesResponse
    FilterOption:
      properties:
        value:
          type: string
          title: Value
        count:
          type: integer
          title: Count
      type: object
      required:
        - value
        - count
      title: FilterOption
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