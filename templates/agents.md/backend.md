
<!-- templates v1.0.0: agents.md/backend.md (draft: not yet used in a project) -->
## Backend

### Build and run

```bash
[RUN_COMMAND]        # start the service locally
[TEST_COMMAND]       # unit and integration tests
[MIGRATE_COMMAND]    # apply database migrations locally
```

- Local backing services: [SERVICES, for example PostgreSQL and Redis via docker compose].
- Config: copy `.env.example` to `.env`. Never commit `.env`.

### Environments

| Environment | URL | Deploys from | Who may deploy |
|-------------|-----|--------------|----------------|
| Development | [URL] | [BRANCH] | Anyone |
| Production | [URL] | [TAG_OR_BRANCH] | Owner approval only |

### Deploy and rollback

- Steps live in [RUNBOOK, for example docs/RUNBOOK.md]: deploy, run migrations, roll back, restore a backup.
- Never run a migration, deploy, or config change against production without owner approval.
- Never test destructive operations on real data.
