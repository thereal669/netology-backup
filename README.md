# Домашнее задание к занятию "`Резервное копирование`" - `Хавилов Виталий`

---

### Задание 1

Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию /tmp/backup

Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые).

Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.

На проверку направить скриншот с командой и результатом ее выполнения.

![Задание 1](https://raw.githubusercontent.com/thereal669/netology-backup/main/pics/task_1_rsync.jpg)

---


### Задание 2

Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.

Резервная копия должна быть полностью зеркальной.

Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции.

Резервная копия размещается локально, в директории /tmp/backup.

На проверку направить файл crontab и скриншот с результатом работы утилиты.


### Содержимое скрипта:

```
#!/bin/bash

# Скрипт резервного копирования домашней директории
BACKUP_DIR="/tmp/backup"
SOURCE_DIR="$HOME"
LOG_TAG="home-backup"

# Создаем директорию для бэкапа если не существует
mkdir -p "$BACKUP_DIR"

# Выполняем резервное копирование
if rsync -avh --delete --exclude='.*/' --checksum "$SOURCE_DIR/" "$BACKUP_DIR/" > /dev/null 2>&1; then
    logger -t "$LOG_TAG" "Резервное копирование домашней директории УСПЕШНО завершено"
    exit 0
else
    logger -t "$LOG_TAG" "ОШИБКА: Резервное копирование домашней директории НЕ УДАЛОСЬ"
    exit 1
fi

```

### Добавляем в crontab:

`0 2 * * * /usr/local/bin/backup_home.sh`

### Все содержимое файла:

```
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command
0 2 * * * /usr/local/bin/backup_home.sh
```

### Результат работы скрипта:

![Задание 2](https://raw.githubusercontent.com/thereal669/netology-backup/main/pics/task_2_crontab.jpg)
