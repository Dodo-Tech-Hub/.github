
<!-- templates v1.0.0: speckit-constitution/api.md (draft: not yet used in a project) -->
## API Principles

### Contract First (OpenAPI)

- The API contract is an [OpenAPI](https://spec.openapis.org/oas/latest.html) 3.1 file in the repository. The contract changes in the same pull request as the code, and contract tests check that the code matches it.
- HTTP methods and status codes follow their meaning in [RFC 9110](https://www.rfc-editor.org/rfc/rfc9110). GET, PUT, and DELETE are safe to retry.
- Errors use the Problem Details format of [RFC 9457](https://www.rfc-editor.org/rfc/rfc9457).
- List endpoints are paginated.

Rationale: a written contract lets clients and server change independently and catches breaking changes in review.

### Versioning and Compatibility

- The API follows [Semantic Versioning](https://semver.org). A breaking change needs a new major version, published next to the old one.
- A deprecated endpoint or field is announced (for example with the `Deprecation` and `Sunset` HTTP headers) before it is removed.

Rationale: clients update on their own schedule, not the server's.

### API Security (OWASP API Security Top 10)

Every endpoint is checked against the [OWASP API Security Top 10](https://owasp.org/API-Security/) (2023 edition):

- Authorization is checked per object and per property, not only per endpoint.
- Authentication uses OAuth 2.0 or OpenID Connect with short-lived access tokens.
- Request size, rate, and resource use are limited. A limit returns `429 Too Many Requests` with a `Retry-After` header.
- Responses return only the fields the client needs.
- Every endpoint is listed in the contract. Old and debug endpoints are removed.

Rationale: an API is reached directly, without the safety of a UI in front of it.
