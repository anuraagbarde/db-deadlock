```sh
docker compose up
```

```sh
brew install mysql-client
```

mac

```sh
echo 'export PATH="/usr/local/opt/mysql-client/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

```sh
mysql -u admin -h 127.0.0.1 -P 3306 test_db -p
```

```sql
CREATE TABLE `table1` ( `id` INT NOT NULL AUTO_INCREMENT, `name` VARCHAR(255) NOT NULL, `marks` INT NOT NULL, PRIMARY KEY (`id`) ) ENGINE=InnoDB;
INSERT INTO table1 (id, name, marks) VALUES (1, "abc", 5);
INSERT INTO table1 (id, name, marks) VALUES (2, "xyz", 1);
```

Connection 1 (One terminal window)

```sql
BEGIN;
UPDATE table1 SET marks=marks-1 WHERE id=1; -- X lock acquired on 1
```

Connection 2 (Another terminal window)

```sh
BEGIN;
UPDATE table1 SET marks=marks+1 WHERE id=2; -- X lock acquired on 2
UPDATE table1 SET marks=marks-1 WHERE id=1; -- LOCK WAIT!
```

Back to Connection 1

```sql
UPDATE table1 SET marks=marks+1 WHERE id=2;
```

You should see the following error:

```
ERROR 1213 (40001): Deadlock found when trying to get lock; try restarting transaction
```

references: https://stackoverflow.com/a/32757025
