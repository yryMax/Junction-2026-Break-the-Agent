> ## Documentation Index
> Fetch the complete documentation index at: https://docs.harborframework.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Upload Job

> Upload a finished or cancelled job to Hub using the server's Hub credentials.



## OpenAPI

````yaml /api-sdk/openapi.json post /hub/jobs/{job_id}/upload
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
  /hub/jobs/{job_id}/upload:
    post:
      tags:
        - Hub
      summary: Upload Job
      description: >-
        Upload a finished or cancelled job to Hub using the server's Hub
        credentials.
      operationId: upload_job_hub_jobs__job_id__upload_post
      parameters:
        - name: job_id
          in: path
          required: true
          schema:
            type: string
            format: uuid
            title: Job Id
        - name: Harbor-API-Version
          in: header
          required: false
          schema:
            type: string
            default: '2026-10-30'
            title: Harbor-Api-Version
      requestBody:
        content:
          application/json:
            schema:
              anyOf:
                - $ref: '#/components/schemas/UploadJobRequest'
                - type: 'null'
              title: Body
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UploadJobResponse'
        '422':
          $ref: 8b94a467-d69f-4b14-a1a0-a4ac7da81e5f
        default:
          $ref: 8b94a467-d69f-4b14-a1a0-a4ac7da81e5f
      x-codeSamples:
        - lang: python
          label: Harbor SDK
          source: |
            from harbor import Harbor

            client = Harbor()
            result = client.hub.jobs.upload("JOB_ID", visibility="private")
            print(result)
            client.close()
components:
  schemas:
    UploadJobRequest:
      properties:
        visibility:
          anyOf:
            - type: string
              enum:
                - public
                - private
            - type: 'null'
          title: Visibility
          description: Default to private on first upload; retain visibility on re-upload.
        org:
          anyOf:
            - type: string
            - type: 'null'
          title: Org
      additionalProperties: false
      type: object
      title: UploadJobRequest
    UploadJobResponse:
      properties:
        job_id:
          type: string
          format: uuid
          title: Job Id
      type: object
      required:
        - job_id
      title: UploadJobResponse
  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      description: Required only when the server has HARBOR_API_KEY configured.

````

This documentation is built and hosted on [Mintlify](https://mintlify.com), a developer documentation platform.