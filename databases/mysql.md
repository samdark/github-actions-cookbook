# MySQL

```yaml
on:
  - pull_request
  - push

name: build

jobs:
  tests:
    name: example
    services:
      mysql:
        image: mysql:5.7
        env:
          MYSQL_USER: test
          MYSQL_PASSWORD: test
          MYSQL_ROOT_PASSWORD: root
          MYSQL_DATABASE: test_database
        ports:
          - 3306:3306
        options: --health-cmd="mysqladmin ping" --health-interval=10s --health-timeout=5s --health-retries=3
    steps:
      
      - name: Create MySQL Database
        run: |
          sudo apt-get update && sudo apt-get install -y mysql-client
          mysql --host 127.0.0.1 --port ${{ job.services.mysql.ports['3306'] }} -uroot -proot -e 'CREATE SCHEMA `test_database` CHARACTER SET utf8 COLLATE utf8_general_ci; GRANT ALL ON `test_database`.* TO test@localhost IDENTIFIED BY "test"; FLUSH PRIVILEGES;'
```
