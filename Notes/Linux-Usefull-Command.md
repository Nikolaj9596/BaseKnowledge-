---
id: Linux-Useful-Command
aliases:
  - The linux useful command
tags:
  - linux
  - command
---
# The linux useful command

## The command for clean cache
```bash
sudo sync; echo 1 | sudo tee /proc/sys/vm/drop_caches
```
## Git

Чтобы рекурсивно клонировать репозиторий с подтягиванием всех подмодулей, используйте команду:

```bash
git clone --recurse-submodules <URL_репозитория>
```
Эта команда клонирует репозиторий и автоматически инициализирует и обновляет все его подмодули.
Если у вас уже есть клонированный репозиторий, но подмодули еще не инициализированы, выполните следующие команды:

```bash
git submodule update --init --recursive
```
Эта команда инициализирует и обновляет все подмодули рекурсивно.
Чтобы позже обновить подмодули до последних коммитов в их ветках, выполните:
```bash
git submodule update --remote --recursive
```
Эта команда подтянет последние изменения из удалённых репозиториев для всех подмодулей.
