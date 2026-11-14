> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# List Jobs

> List jobs with filters and pagination. Pass next_page_token as page_token.



## OpenAPI

````yaml /api-sdk/openapi.json get /jobs
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
  /jobs:
    get:
      tags:
        - Jobs
      summary: List Jobs
      description: >-
        List jobs with filters and pagination. Pass next_page_token as
        page_token.
      operationId: list_jobs_jobs_get
      parameters:
        - name: page_size
          in: query
          required: false
          schema:
            type: integer
            minimum: 0
            description: >-
              Number of items; zero uses the default (50). Values above 100 are
              capped.
            default: 50
            title: Page Size
          description: >-
            Number of items; zero uses the default (50). Values above 100 are
            capped.
        - name: page_token
          in: query
          required: false
          schema:
            type: string
            maxLength: 4096
            default: ''
            title: Page Token
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
        - name: sort
          in: query
          required: false
          schema:
            type: string
            maxLength: 100
            description: Sort field; prefix '-' for descending. Nulls sort last.
            enum:
              - id
              - name
              - state
              - created_at
              - updated_at
              - started_at
              - finished_at
              - '-id'
              - '-name'
              - '-state'
              - '-created_at'
              - '-updated_at'
              - '-started_at'
              - '-finished_at'
            default: '-created_at'
            title: Sort
          description: Sort field; prefix '-' for descending. Nulls sort last.
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
                $ref: '#/components/schemas/JobsResponse'
        default:
          $ref: '#/components/responses/ProblemDetails'
      x-codeSamples:
        - lang: python
          label: Harbor SDK
          source: |
            from harbor import Harbor

            client = Harbor()
            result = client.jobs.list(page_size=10)
            print(result)
            client.close()
components:
  schemas:
    JobsResponse:
      properties:
        items:
          items:
            $ref: '#/components/schemas/JobSummary'
          type: array
          title: Items
        total:
          type: integer
          minimum: 0
          title: Total
          description: Total matching items, before pagination.
        next_page_token:
          type: string
          title: Next Page Token
          default: ''
      type: object
      required:
        - items
        - total
        - next_page_token
      title: JobsResponse
    JobSummary:
      properties:
        created_at:
          type: string
          format: date-time
          title: Created At
        updated_at:
          type: string
          format: date-time
          title: Updated At
        started_at:
          anyOf:
            - type: string
              format: date-time
            - type: 'null'
          title: Started At
        finished_at:
          anyOf:
            - type: string
              format: date-time
            - type: 'null'
          title: Finished At
        id:
          type: string
          format: uuid
          title: Id
        name:
          type: string
          title: Name
        state:
          type: string
          enum:
            - queued
            - running
            - paused
            - cancelling
            - finished
            - cancelled
          title: State
        rollouts:
          $ref: '#/components/schemas/RolloutCounts'
        n_retries:
          type: integer
          minimum: 0
          title: N Retries
          default: 0
        n_errors:
          type: integer
          minimum: 0
          title: N Errors
          default: 0
        agent_names:
          items:
            type: string
          type: array
          title: Agent Names
        model_names:
          items:
            type: string
          type: array
          title: Model Names
        model_providers:
          items:
            type: string
          type: array
          title: Model Providers
        reasoning_efforts:
          items:
            type: string
          type: array
          title: Reasoning Efforts
        sources:
          items:
            type: string
          type: array
          title: Sources
        rewards:
          additionalProperties:
            $ref: '#/components/schemas/MeanAggregate'
          type: object
          title: Rewards
        usage:
          $ref: '#/components/schemas/UsageAggregates'
      type: object
      required:
        - created_at
        - updated_at
        - started_at
        - finished_at
        - id
        - name
        - state
        - rollouts
        - n_retries
        - n_errors
        - agent_names
        - model_names
        - model_providers
        - reasoning_efforts
        - sources
        - rewards
        - usage
      title: JobSummary
      description: >-
        All-attempt counts/usage, and valid rewards from latest attempts only.


        n_errors counts attempts with a non-cancellation execution error, once
        per

        attempt. Storage errors are separate. Each metric's count is its
        coverage.
    RolloutCounts:
      properties:
        n_total:
          type: integer
          minimum: 0
          title: N Total
        n_queued:
          type: integer
          minimum: 0
          title: N Queued
          default: 0
        n_running:
          type: integer
          minimum: 0
          title: N Running
          default: 0
        n_paused:
          type: integer
          minimum: 0
          title: N Paused
          default: 0
        n_cancelling:
          type: integer
          minimum: 0
          title: N Cancelling
          default: 0
        n_cancelled:
          type: integer
          minimum: 0
          title: N Cancelled
          default: 0
        n_finished:
          type: integer
          minimum: 0
          title: N Finished
          default: 0
      type: object
      required:
        - n_total
        - n_queued
        - n_running
        - n_paused
        - n_cancelling
        - n_cancelled
        - n_finished
      title: RolloutCounts
    MeanAggregate:
      properties:
        mean:
          anyOf:
            - type: number
            - type: 'null'
          title: Mean
        count:
          type: integer
          minimum: 0
          title: Count
          default: 0
      type: object
      required:
        - mean
        - count
      title: MeanAggregate
    UsageAggregates:
      properties:
        n_input_tokens:
          $ref: '#/components/schemas/SumAggregate'
        n_cached_input_tokens:
          $ref: '#/components/schemas/SumAggregate'
        n_output_tokens:
          $ref: '#/components/schemas/SumAggregate'
        cost_usd:
          $ref: '#/components/schemas/SumAggregate'
      type: object
      required:
        - n_input_tokens
        - n_cached_input_tokens
        - n_output_tokens
        - cost_usd
      title: UsageAggregates
    SumAggregate:
      properties:
        sum:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Sum
        count:
          type: integer
          minimum: 0
          title: Count
          default: 0
      type: object
      required:
        - sum
        - count
      title: SumAggregate
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