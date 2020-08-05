# PostgreSQL

```yaml
on:
  - pull_request
  - push

name: build

jobs:
  tests:
    name: example
    services:
      postgres:
        image: postgres:9.6
        env:
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: test_db
        ports:
          - 5432:5432
        options: --name=postgres --health-cmd="pg_isready" --health-interval=10s --health-timeout=5s --health-retries=3

    steps:
      - name: Create PostgreSQL Database
        env:
          PGHOST: 127.0.0.1
          PGUSER: postgres
          PGDATABASE: test_db
          PGPASSWORD: postgres
          PGPORT: ${{ job.services.postgres.ports['5432'] }}
        run: |
          sudo apt-get update && sudo apt-get install -y postgresql-client
          psql -q -c "CREATE ROLE test WITH PASSWORD 'test' LOGIN;" -U postgres && psql -q -c 'CREATE DATABASE test_db WITH OWNER = test;' -U postgres && psql -q -c 'GRANT ALL PRIVILEGES ON DATABASE test_db TO test;' -U postgres
```
