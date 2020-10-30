# MongoDB

```yaml
on:
  - pull_request
  - push

name: build

jobs:
  tests:
    name: example
    services:    
        mongodb:
            image: mongo
            ports:
                - 27017:27017

    steps:
      - name: Run tests
        run: test
```
