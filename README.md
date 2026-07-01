# Домашнее задание «Репликация и масштабирование. Часть 1»

Александр Масайлов

---

## Задание 1

### Master-Slave

В этой схеме есть главный сервер (Master), на который записываются все данные. Второй сервер (Slave) автоматически получает изменения с Master. Обычно запись идёт на Master, а чтение можно делать со Slave.

### Master-Master

В этой схеме оба сервера являются главными и могут принимать запись. Данные синхронизируются между ними. Такая схема сложнее, но если один сервер перестанет работать, можно продолжить работать со вторым.

---

## Задание 2

Настроил репликацию Master-Slave между двумя виртуальными машинами.

### Настройка Master

Изменил файл `mysqld.cnf`:

- `bind-address = 0.0.0.0`
- `server-id = 1`
- `log_bin = /var/log/mysql/mysql-bin.log`

Скриншот:

![img](скрин1.png)

Создал пользователя для репликации:

```sql
CREATE USER 'repl'@'10.129.0.11' IDENTIFIED BY 'password';

GRANT REPLICATION SLAVE ON *.* TO 'repl'@'10.129.0.11';

FLUSH PRIVILEGES;
```

Получил параметры Master:

```sql
SHOW MASTER STATUS;
```

Скриншот:

![img](скрин2.png)

---

### Настройка Slave

Изменил файл `mysqld.cnf`:

- `bind-address = 0.0.0.0`
- `server-id = 2`

Скриншот:

![img](скрин3.png)

Настроил подключение к Master:

```sql
CHANGE REPLICATION SOURCE TO
SOURCE_HOST='10.129.0.16',
SOURCE_USER='repl',
SOURCE_PASSWORD='password',
SOURCE_LOG_FILE='mysql-bin.000001',
SOURCE_LOG_POS=876;

START REPLICA;
```

Проверил состояние репликации:

```sql
SHOW REPLICA STATUS\G
```

Репликация работает:

- Replica_IO_Running: Yes
- Replica_SQL_Running: Yes

Скриншот:

![img](скрин4.png)
