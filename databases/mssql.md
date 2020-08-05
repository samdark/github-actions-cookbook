# MSSQL

```yaml
on:
  - pull_request
  - push

name: build

jobs:
  tests:
    name: example
    services:
      mssql:
        image: mcr.microsoft.com/mssql/server:2017-latest
        env:
          SA_PASSWORD: YourStrong!Passw0rd
          ACCEPT_EULA: Y
          MSSQL_PID: Developer
        ports:
          - 1433:1433
        options: --name=mssql --health-cmd="/opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'YourStrong!Passw0rd' -Q 'SELECT 1'" --health-interval=10s --health-timeout=5s --health-retries=3

    steps:
      - name: Create MS SQL Database
        run: |
          sudo apt-get update && sudo apt-get install -y mssql-tools
          /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'YourStrong!Passw0rd' -Q 'CREATE DATABASE test_db'
```