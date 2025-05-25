# Полный справочник по командам Git

## 1. Настройка Git
```bash
# Установка имени пользователя
git config --global user.name "Ваше Имя"

# Установка email
git config --global user.email "ваш@email.com"

# Установка редактора по умолчанию
git config --global core.editor "nano"

# Просмотр всех настроек
git config --list

# Автоматическая конвертация концов строк
git config --global core.autocrlf true  # Для Windows
git config --global core.autocrlf input # Для Linux/Mac

# Кэширование учётных данных
git config --global credential.helper cache
```
2. Создание репозитория

```bash
# Инициализация нового репозитория
git init

# Клонирование существующего репозитория
git clone https://github.com/user/repo.git
git clone --depth 1 https://url  # Неглубокий клон (только последний коммит)

# Создание пустого репозитория (без рабочей директории)
git init --bare
```
3. Основные команды

```bash
# Добавление файлов в индекс
git add file.txt         # Конкретный файл
git add .                # Все изменения
git add -A               # Все изменения (включая удалённые)
git add -p               # Интерактивное добавление

# Проверка статуса
git status
git status -s            # Краткий вывод

# Фиксация изменений
git commit -m "Сообщение"
git commit -a -m "Сообщение"  # Добавить и закоммитить все изменения
git commit --amend       # Изменить последний коммит

# Просмотр изменений
git diff                 # Неиндексированные изменения
git diff --staged        # Индексированные изменения
git diff HEAD~3 HEAD     # Сравнение коммитов
```
4. Ветвление


```bash
# Создание веток
git branch feature       # Создать ветку
git branch -d feature    # Удалить ветку
git branch -m old new    # Переименовать ветку
git branch -a            # Показать все ветки (включая удалённые)

# Переключение между ветками
git checkout feature     # Переключиться на ветку
git checkout -b feature  # Создать и переключиться

# Современная альтернатива checkout (Git 2.23+)
git switch feature       # Переключиться
git switch -c feature    # Создать и переключиться

# Слияние веток
git merge feature        # Влить feature в текущую ветку
git merge --abort        # Отменить слияние
git merge --no-ff        # Запретить fast-forward

# Управление ветками
git branch --merged      # Показать слитые ветки
git branch --no-merged   # Показать не слитые ветки
git branch --track dev origin/dev  # Отслеживать удалённую ветку
```
5. Работа с удалёнными репозиториями

```bash
# Управление удалёнными источниками
git remote add origin https://url
git remote -v            # Показать удалённые репозитории
git remote remove origin
git remote rename old new

# Получение изменений
git fetch origin         # Получить изменения без слияния
git fetch --prune        # Удалить устаревшие ссылки

# Отправка изменений
git push origin main
git push -u origin main  # Установить upstream
git push --force-with-lease  # Безопасный форсированный пуш

# Получение + слияние
git pull origin main     # fetch + merge
git pull --rebase        # fetch + rebase
```
6. Отмена изменений

```bash
# Отмена в рабочей директории
git restore file.txt          # Отменить неиндексированные изменения
git restore --staged file.txt # Убрать из индекса

# Сброс коммитов
git reset HEAD~1              # Мягкий сброс (сохраняет изменения)
git reset --mixed HEAD~1      # По умолчанию (сбрасывает индекс)
git reset --hard HEAD~1       # Полное удаление изменений

# Перезапись истории
git revert HEAD               # Создать отменяющий коммит
git commit --amend            # Исправить последний коммит
```
7. Просмотр истории

```bash
# Основная команда
git log
git log --oneline            # Краткий формат
git log -p                   # С изменениями
git log --graph              # Графическое представление
git log --since="2 weeks ago"
git log --author="John"

# Поиск в истории
git blame file.txt           # Кто менял строки
git grep "текст"             # Поиск по проекту
git show HEAD~2              # Показать конкретный коммит

# Анализ
git shortlog                 # Статистика по авторам
git log --stat               # Статистика изменений
```
8. Теги

```bash
# Создание тегов
git tag v1.0                 # Легковесный тег
git tag -a v1.0 -m "Описание"  # Аннотированный тег
git tag -s v1.0              # Подписанный тег

# Управление тегами
git tag -d v1.0              # Удалить тег
git push origin --tags       # Отправить все теги
git push origin v1.0         # Отправить конкретный тег
git checkout v1.0            # Переключиться на тег
```
9. Слияние и перебазирование

```bash
# Перебазирование
git rebase main              # Перенести коммиты на основную ветку
git rebase --abort           # Отменить перебазирование
git rebase -i HEAD~3         # Интерактивное перебазирование

# Разрешение конфликтов
git mergetool                # Запуск инструмента слияния
git add resolved-file.txt    # После разрешения конфликта
git rebase --continue        # Продолжить перебазирование
```

10. Временное сохранение (Stash)

```bash
git stash                    # Сохранить изменения
git stash save "Сообщение"
git stash list               # Показать сохранённые стеши
git stash apply stash@{1}    # Применить конкретный стек
git stash pop                # Применить и удалить последний
git stash drop stash@{0}     # Удалить конкретный стек
git stash clear              # Очистить все стеши
```

11. Подмодули

```bash
git submodule add https://url  # Добавить подмодуль
git submodule update --init  # Инициализировать подмодули
git submodule foreach 'git pull origin main'  # Обновить все подмодули
git submodule deinit path    # Удалить подмодуль
```
12. Диагностика и обслуживание

```bash
git fsck                     # Проверка целостности
git gc                       # Сборка мусора
git reflog                   # История всех действий
git bisect start             # Бинарный поиск бага
git worktree add ../branch   # Добавить рабочее дерево
```
13. Расширенные команды

```bash
# Патчи
git format-patch HEAD~2      # Создать патчи из коммитов
git am patch-file            # Применить патч

# Переписывание истории
git filter-branch --tree-filter 'command'
git replace                  # Замена объектов

# Пакетная работа
git bundle create repo.bundle HEAD  # Создать бандл

# Подпись коммитов
git commit -S -m "Signed commit"
```
14. Хуки (Git Hooks)

```bash
# Примеры хуков
pre-commit     # Перед коммитом
post-receive   # После получения изменений
pre-push       # Перед отправкой
```
# Путь к хукам
.git/hooks/
15. Алиасы

```bash
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
16. Работа с файлами

```bash
git rm file.txt             # Удалить файл из индекса
git mv old.txt new.txt      # Переименовать файл
git ls-files                # Показать отслеживаемые файлы
git clean -fd               # Удалить неотслеживаемые файлы
```
17. Дополнительные инструменты

```bash
git archive --format zip -o repo.zip HEAD  # Архивировать репозиторий
git instaweb                               # Веб-интерфейс
git cherry-pick commit_hash  # Перенести конкретный коммит
```

```bash
git help <команда>
man git-<команда>
```
