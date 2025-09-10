# 10-03
Резервное копирование
### Задание 1
- Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию `/tmp/backup`
- Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
- Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
- На проверку направить скриншот с командой и результатом ее выполнения

### *Решение:*
```bash
rsync -a --checksum --verbose --delete --progress --exclude '.*' /home/test/ /tmp/backup
```
<img width="1434" height="845" alt="image" src="https://github.com/user-attachments/assets/1aa9744f-9a43-4f10-b9b7-b52900e71fba" />
<img width="798" height="840" alt="image" src="https://github.com/user-attachments/assets/72343afd-91a2-4cc3-953a-e78cbdb0584a" />



### Задание 2
- Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
- Резервная копия должна быть полностью зеркальной
- Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
- Резервная копия размещается локально, в директории `/tmp/backup`
- На проверку направить файл crontab и скриншот с результатом работы утилиты.

<img width="704" height="527" alt="image" src="https://github.com/user-attachments/assets/3feb8e36-c5e9-4a38-a8b5-46c7d11f3387" />



### Cкрипт

```bash
#!/bin/bash
copy=$1
paste=$2

if [ ! -d $copy ]; then
	echo -e "\033[31m----- | $(date +"%Y-%m-%d | %H:%M:%S") | Backup completed Unsuccessfully! | Source directory does not exist. | -----\033[0m" >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
elif [ ! -d $paste ]; then
	echo -e "\033[31m----- | $(date +"%Y-%m-%d | %H:%M:%S") | Backup completed Unsuccessfully! | Destination directory does not exist. | -----\033[0m" >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
else
	rsync -a --checksum --verbose --delete --progress --exclude '.*' $copy $paste >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
  echo -e "\033[32m----- | $(date +"%Y-%m-%d | %H:%M:%S") | Backup completed Successfully! | -----\033[0m" >> /var/log/rsync-cron-$(date +"%Y-%m-%d").log
fi
```


### Результат
<img width="636" height="301" alt="image" src="https://github.com/user-attachments/assets/72e3ef89-be82-4991-83ea-13b6608c0872" />
<img width="1018" height="800" alt="image" src="https://github.com/user-attachments/assets/1c7d071d-b237-4a89-8815-de3eaad1b662" />



---
