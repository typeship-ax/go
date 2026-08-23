# Changelog

## 0.6.0 (2026-08-23)

### Added
- `projects.retrieveGithubHealth()`: GET `/projects/{project_id}/github`

### Changed
- `projects.list()`
  - `return-type-changed`: nested response schema changed
- `projects.create()`
  - `body-field-type-changed`: packages: nested schema changed
  - `return-type-changed`: nested response schema changed
- `projects.retrieve()`
  - `return-type-changed`: nested response schema changed
- `projects.update()`
  - `body-field-type-changed`: packages: nested schema changed
  - `return-type-changed`: nested response schema changed

## 0.6.0 (2026-08-23) (16 breaking)

### Added
- `specRevisions.list()`: GET /projects/{project_id}/spec_revisions
- `specRevisions.retrieve()`: GET /spec_revisions/{spec_revision_id}
- `specRevisions.retrieveContent()`: GET /spec_revisions/{spec_revision_id}/content

### Removed (breaking)
- `specVersions.list()`: GET /projects/{project_id}/spec_versions
- `specVersions.retrieve()`: GET /spec_versions/{spec_version_id}
- `specVersions.retrieveContent()`: GET /spec_versions/{spec_version_id}/content

### Changed
- `generate.run()`
  - **breaking** body-field-type-changed: spec: nested schema changed
  - **breaking** body-field-type-changed: config: nested schema changed
  - **breaking** return-type-changed: nested response schema changed
  - **breaking** error-added: 400: BadRequestError
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-type-changed: 413: PayloadTooLargeError<ErrorModel> -> PayloadTooLargeError<ErrorModel>
  - **breaking** error-type-changed: 422: UnprocessableEntityError<ErrorModel> -> UnprocessableEntityError<ErrorModel>
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>
  - **breaking** error-type-changed: default: ApiResponseError<ErrorModel> -> ApiResponseError<ErrorModel>
- `projects.list()`
  - **breaking** return-type-changed: nested response schema changed
  - **breaking** error-type-changed: 400: BadRequestError<ErrorModel> -> BadRequestError<ErrorModel>
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>
- `projects.create()`
  - param-added: Idempotency-Key: string
  - **breaking** body-field-type-changed: source: SourceInput -> ProjectSourceInput
  - **breaking** body-field-now-required: source
  - **breaking** body-field-type-changed: packages: nested schema changed
  - **breaking** body-field-type-changed: spec_patches: nested schema changed
  - **breaking** body-field-type-changed: config: nested schema changed
  - **breaking** body-field-removed: spec_url
  - **breaking** return-type-changed: nested response schema changed
  - documentation-changed: summary or description changed
  - **breaking** error-type-changed: 400: BadRequestError<ErrorModel> -> BadRequestError<ErrorModel>
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 402: PaymentRequiredError<ErrorModel> -> PaymentRequiredError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-added: 409: ConflictError
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>
  - **breaking** error-added: 500: InternalServerError
- `projects.retrieve()`
  - **breaking** return-type-changed: nested response schema changed
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>
- `projects.delete()`
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>
- `projects.update()`
  - **breaking** body-field-type-changed: source: SourceInput -> ProjectSourceInput
  - **breaking** body-field-type-changed: packages: nested schema changed
  - **breaking** body-field-type-changed: spec_patches: nested schema changed
  - **breaking** body-field-type-changed: config: nested schema changed
  - **breaking** body-field-removed: spec_url
  - **breaking** return-type-changed: nested response schema changed
  - **breaking** error-type-changed: 400: BadRequestError<ErrorModel> -> BadRequestError<ErrorModel>
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 402: PaymentRequiredError<ErrorModel> -> PaymentRequiredError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>
- `projects.listGenerations()`
  - **breaking** return-type-changed: nested response schema changed
  - **breaking** error-type-changed: 400: BadRequestError<ErrorModel> -> BadRequestError<ErrorModel>
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>
- `projects.generate()`
  - **breaking** return-type-changed: { data: Array<Generation | GenerationFailure>; } -> GenerationBatch
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 402: PaymentRequiredError<ErrorModel> -> PaymentRequiredError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-type-changed: 422: UnprocessableEntityError<ErrorModel> -> UnprocessableEntityError<ErrorModel>
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>
  - **breaking** error-type-changed: 500: InternalServerError<ErrorModel> -> InternalServerError<ErrorModel>
- `generations.retrieve()`
  - **breaking** return-type-changed: nested response schema changed
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>
- `generations.retrieveFile()`
  - **breaking** error-added: 400: BadRequestError
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>
- `account.retrieve()`
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>
- `apiKeys.list()`
  - **breaking** error-type-changed: 400: BadRequestError<ErrorModel> -> BadRequestError<ErrorModel>
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>
- `apiKeys.revoke()`
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 403: ForbiddenError<ErrorModel> -> ForbiddenError<ErrorModel>
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-type-changed: 429: RateLimitedError<ErrorModel> -> RateLimitedError<ErrorModel>

## 0.5.0 (2026-08-22)

### Changed
- `generate.run()`
  - body-field-type-changed: config: nested schema changed
- `projects.list()`
  - return-type-changed: nested response schema changed
- `projects.create()`
  - body-field-type-changed: config: nested schema changed
  - return-type-changed: nested response schema changed
- `projects.retrieve()`
  - return-type-changed: nested response schema changed
- `projects.update()`
  - body-field-type-changed: config: nested schema changed
  - return-type-changed: nested response schema changed

## 0.5.0 (2026-08-22) (18 breaking)

### Added
- `projects.retrieve()`: GET /projects/{project_id}
- `generations.retrieve()`: GET /generations/{generation_id}
- `generations.retrieveFile()`: GET /generations/{generation_id}/file
- `specVersions.retrieve()`: GET /spec_versions/{spec_version_id}
- `specVersions.retrieveContent()`: GET /spec_versions/{spec_version_id}/content
- `account.retrieve()`: GET /me

### Removed (breaking)
- `projects.get()`: GET /projects/{project_id}
- `projects.mcpUsage()`: GET /projects/{project_id}/mcp_usage
- `generations.get()`: GET /generations/{generation_id}
- `generations.getFile()`: GET /generations/{generation_id}/file
- `specVersions.get()`: GET /spec_versions/{spec_version_id}
- `specVersions.getContent()`: GET /spec_versions/{spec_version_id}/content
- `account.me()`: GET /me
- `usage.retrieve()`: GET /usage

### Changed
- `generate.run()`
  - body-field-type-changed: spec: nested schema changed
  - **breaking** body-field-type-changed: config: nested schema changed
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-added: 403: ForbiddenError
  - **breaking** error-type-changed: 413: PayloadTooLargeError<ErrorModel> -> PayloadTooLargeError<ErrorModel>
  - **breaking** error-type-changed: 422: UnprocessableEntityError<ErrorModel> -> UnprocessableEntityError<ErrorModel>
  - **breaking** error-added: 429: RateLimitedError
  - **breaking** error-type-changed: default: ApiResponseError<ErrorModel> -> ApiResponseError<ErrorModel>
- `projects.list()`
  - **breaking** return-type-changed: { data: Project[]; next_cursor?: string | null; } -> ProjectList
  - **breaking** error-added: 400: BadRequestError
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-added: 403: ForbiddenError
  - **breaking** error-added: 429: RateLimitedError
- `projects.create()`
  - **breaking** body-field-type-changed: source: Source -> SourceInput
  - **breaking** body-field-type-changed: config: nested schema changed
  - **breaking** return-type-changed: nested response schema changed
  - **breaking** error-type-changed: 400: BadRequestError<ErrorModel> -> BadRequestError<ErrorModel>
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 402: PaymentRequiredError<ErrorModel> -> PaymentRequiredError<ErrorModel>
  - **breaking** error-added: 403: ForbiddenError
  - **breaking** error-added: 429: RateLimitedError
- `projects.delete()`
  - **breaking** param-type-changed: project_id: string -> ProjectId
  - **breaking** return-type-changed: { deleted: true; } -> DeletedProject
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-added: 403: ForbiddenError
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-added: 429: RateLimitedError
- `projects.update()`
  - **breaking** param-type-changed: project_id: string -> ProjectId
  - **breaking** body-field-type-changed: source: Source -> SourceInput
  - **breaking** body-field-type-changed: config: nested schema changed
  - **breaking** return-type-changed: nested response schema changed
  - **breaking** error-type-changed: 400: BadRequestError<ErrorModel> -> BadRequestError<ErrorModel>
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 402: PaymentRequiredError<ErrorModel> -> PaymentRequiredError<ErrorModel>
  - **breaking** error-added: 403: ForbiddenError
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-added: 429: RateLimitedError
- `projects.listGenerations()`
  - **breaking** param-type-changed: project_id: string -> ProjectId
  - **breaking** return-type-changed: { data: Generation[]; next_cursor?: string | null; } -> GenerationList
  - **breaking** error-added: 400: BadRequestError
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-added: 403: ForbiddenError
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-added: 429: RateLimitedError
- `projects.generate()`
  - **breaking** param-type-changed: project_id: string -> ProjectId
  - **breaking** return-type-changed: nested response schema changed
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-type-changed: 402: PaymentRequiredError<ErrorModel> -> PaymentRequiredError<ErrorModel>
  - **breaking** error-added: 403: ForbiddenError
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-type-changed: 422: UnprocessableEntityError<ErrorModel> -> UnprocessableEntityError<ErrorModel>
  - **breaking** error-added: 429: RateLimitedError
  - **breaking** error-type-changed: 500: InternalServerError<ErrorModel> -> InternalServerError<ErrorModel>
- `specVersions.list()`
  - **breaking** param-type-changed: project_id: string -> ProjectId
  - **breaking** return-type-changed: { data: SpecVersion[]; next_cursor?: string | null; } -> SpecVersionList
  - **breaking** error-added: 400: BadRequestError
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-added: 403: ForbiddenError
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-added: 429: RateLimitedError
- `apiKeys.list()`
  - **breaking** return-type-changed: { data: ApiKey[]; next_cursor?: string | null; } -> ApiKeyList
  - **breaking** error-added: 400: BadRequestError
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-added: 403: ForbiddenError
  - **breaking** error-added: 429: RateLimitedError
- `apiKeys.revoke()`
  - **breaking** return-type-changed: nested response schema changed
  - **breaking** error-type-changed: 401: UnauthorizedError<ErrorModel> -> UnauthorizedError<ErrorModel>
  - **breaking** error-added: 403: ForbiddenError
  - **breaking** error-type-changed: 404: NotFoundError<ErrorModel> -> NotFoundError<ErrorModel>
  - **breaking** error-added: 429: RateLimitedError

## 0.3.0 (2026-08-22)

### Changed
- `generate.run()`
  - return-type-changed: nested response schema changed
- `projects.listGenerations()`
  - return-type-changed: nested response schema changed
- `projects.generate()`
  - return-type-changed: nested response schema changed
  - documentation-changed: summary or description changed
- `generations.get()`
  - return-type-changed: nested response schema changed
