
<!-- templates v1.0.0: speckit-constitution/backend.md (draft: not yet used in a project) -->
## Backend Principles

### Twelve-Factor Services

Services follow [The Twelve-Factor App](https://12factor.net):

- Config, including secrets, comes from the environment or a secret manager, never from code.
- Processes are stateless. State lives in backing services (database, cache, queue, object storage), each reached through config.
- Logs are written as an event stream to stdout, in a structured format.
- Development, staging, and production stay as similar as possible.
- Processes start fast and shut down gracefully.

Rationale: these rules make a service easy to deploy, scale, and replace.

### Backend Security (OWASP ASVS)

Every feature MUST meet [OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/) 5.0 at Level 1, and at Level 2 when it handles personal or payment data. These rules are the ones that most often apply:

- Access control is enforced on the server, denies by default, and is checked on every request.
- Database queries use parameterized statements or the ORM. Never build queries from strings.
- Passwords, if stored at all, use a slow password hash (Argon2id, scrypt, or bcrypt).
- Database roles and service accounts get the least privilege they need.
- Rate limits protect sign-in and expensive endpoints.

Rationale: the server holds every user's data, so one gap exposes all of them.

### Data and Operations

- Schema changes are versioned migrations in the repository. Each migration is tested on a copy of production-like data before release. A destructive migration needs owner approval.
- Backups are automated and follow the 3-2-1 rule (three copies, two media, one off-site). A restore is tested at least once per quarter.
- Every service exposes a health check and reports errors to a monitoring tool.
- Every production change (deploy, migration, config change) needs owner approval and has a written rollback step.
- Files (images, videos, documents) go to object storage. The database stores only their references.

Rationale: data loss and long outages are the failures users do not forgive.
