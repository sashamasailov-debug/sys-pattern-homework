# Домашнее задание «Резервное копирование»

Александр Масайлов

---

## Задание 1

Сделал резервное копирование домашней директории с помощью rsync.

Команда:

```bash
rsync -avc --delete --exclude='.*' ~/ /tmp/backup/
```

Проверка:

```bash
ls /tmp/backup
ls -a /tmp/backup
```

Скриншоты:

![img](img/img1.png)

![img](img/img2.png)

---

## Задание 2

Создал скрипт backup.sh:

```bash
#!/bin/bash

rsync -avc --delete --exclude='.*' /home/user/ /tmp/backup/

if [ $? -eq 0 ]; then
    logger "Backup completed successfully"
else
    logger "Backup failed"
fi
```

Сделал файл исполняемым:

```bash
chmod +x ~/backup.sh
```

Настроил cron:

```cron
0 2 * * * /home/user/backup.sh
```

Проверка:

```bash
crontab -l
```

Скриншоты:

![img](img/img3.png)

![img](img/img4.png)

