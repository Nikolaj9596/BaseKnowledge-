# Instruction mount s3 backet to local folder

## Устанавливаем s3fs

```bash
apt update
apt install s3fs
```

В корневом каталоге пользователя (в данной инструкции — /home/s3admin) создаем папку s3disk.
К этой папке мы смонтируем ранее созданный контейнер s3sel из объектного хранилища Selectel.
Вводим:

```bash
mkdir s3disk
```
## Производим монтирование контейнера

Вернемся к контейнеру объектного хранилища s3sel.
Копируем логин и пароль из панели управления, сохраняем эту информацию в отдельном файле ~/.passwd-s3fs.
Также стоит ограничить доступ к файлу — например, открыть доступ только владельцу.
Вводим следующие строки:

```bash
echo '136692_s3selectel:d5Qwiq`S7P' > ${HOME}/.passwd-s3fs
chmod 600 ${HOME}/.passwd-s3fs
```
Используем установленную утилиту s3fs, чтобы смонтировать контейнер s3sel к созданной папке s3disk.
В качестве endpoint — [[https://s3.storage.selcloud.ru]]. 
Далее выполняем команду следующего вида: 
```bash
s3fs <имя контейнера> <путь к каталогу> -o allow_other -o passwd_file=<путь к файлу паролей> -o use_path_request_style -o endpoint=<регион> -o url= 
```
Подставим значения, актуальные для данной инструкции:

```bash
s3fs s3sel /home/s3admin/s3disk -o allow_other -o passwd_file=/home/s3admin/.passwd-s3fs -o use_path_request_style -o endpoint=ru-1 -o url=https://s3.storage.selcloud.ru
```
## Автоматизируем запуск s3fs

Чтобы не монтировать контейнер каждый раз вручную, можно настроить автоматизацию запуска s3fs.
Это делается через изменение конфигурационного файла fstab. 
Вводим в консоль команду: 

```bash
nano /etc/fstab
```

Добавляем в файл строку следующего вида:

```bash
s3fs <имя контейнера> <путь к каталогу> fuse allow_other,nonempty,passwd_file=<путь к файлу паролей>,use_path_request_style,endpoint=<регион>,url= 0 0
```

Добавляем данные инструкции:
```bash
s3fs#s3sel /home/s3admin/s3disk fuse allow_other,nonempty,passwd_file=/home/s3admin/.passwd-s3fs,use_path_request_style,endpoint=ru-1,url=[] 0 0
```
