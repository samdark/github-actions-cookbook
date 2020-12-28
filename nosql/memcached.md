# Memcached

```yaml
on:
  - pull_request
  - push

name: build

jobs:
  tests:
    name: example
    services:
      memcached:
        image: memcached
        ports:
          - 11211/tcp        
        options: --health-cmd "timeout 5 bash -c 'cat < /dev/null > /dev/tcp/127.0.0.1/11211'" --health-interval 10s --health-timeout 5s --health-retries 5
```
