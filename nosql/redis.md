# Redis

```yaml
on:
  - pull_request
  - push

name: build

jobs:
  tests:
    name: example
    services:
      redis:
        image: redis
        ports:
          - 6379:6379
        options:
          --health-cmd "redis-cli ping"
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5

    steps:
      - name: Run tests
        run: test
        env:
          # The hostname used to communicate with the Redis service container
          REDIS_HOST: localhost
          # The default Redis port
          REDIS_PORT: 6379
```
