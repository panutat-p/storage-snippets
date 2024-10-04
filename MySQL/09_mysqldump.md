# mysqldump

https://dev.mysql.com/doc/refman/8.4/en/mysqldump.html

## Install

```sh
apt install -y mysql-client
```

```sh
brew install mysql-client
```

Mac Intel
```sh
export PATH="$PATH:/usr/local/opt/mysql-client/bin"
```

Mac Arm
```sh
export PATH="$PATH:/opt/homebrew/opt/mysql-client/bin"
```

```sh
ls /opt/homebrew/opt/mysql-client/bin
```

```sh
ls /opt/homebrew/bin/mysqldump
```

## Export

Dump only one database
```sh
mkdir $HOME/mysqldump

mysqldump demo \
-h=127.0.0.1 \
-u=root \
-P=1234 \
--lock-all-tables \
> $HOME/mysqldump/demo.sql
```

Options
* `--lock-all-tables`
* `--no-create-db`
* `--no-create-info`
* `--no-data`
* `--single-transaction`
