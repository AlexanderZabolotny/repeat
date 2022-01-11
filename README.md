# Шпаргалка по командам
###### В этом параграфе приведена сухая шпаргалка по командам Git. Я далеко не спец в этой системе контроля версий, так что ошибки в терминологии или еще в чем-то вполне возможны.

- Создать новый репозиторий:

git init project-name

- Если вы планируете клонировать его по ssh с удаленной машины, также скажите:

git config --bool core.bare true
… иначе при git push вы будете получать странные ошибки вроде:

Refusing to update checked out branch: refs/heads/master
By default, updating the current branch in a non-bare repository
is denied, because it will make the index and work tree inconsistent
with what you pushed, and will require 'git reset --hard' to match
the work tree to HEAD.

- Клонировать репозиторий с удаленной машины:

git clone git@bitbucket.org:afiskon/hs-textgen.git

- Если хотим пушить один код в несколько репозиториев:

git remote add remotename git@gitlab.example.ru:repo.git

- Добавить файл в репозиторий:

git add text.txt

- Удалить файл:

git rm text.txt

- Текущее состояние репозитория (изменения, неразрешенные конфликты и тп):

git status
- Сделать коммит:

git commit -a -m "Commit description"

Сделать коммит, введя его описание с помощью $EDITOR:

git commit -a

- Замержить все ветки локального репозитория на удаленный репозиторий (аналогично вместо origin можно указать и remotename, см выше):

git push origin

- Аналогично предыдущему, но делается пуш только ветки master:

git push origin master

- Запушить текущую ветку, не вводя целиком ее название:

git push origin HEAD

- Замержить все ветки с удаленного репозитория:

git pull origin

- Аналогично предыдущему, но накатывается только ветка master:

git pull origin master

- Накатить текущую ветку, не вводя ее длинное имя:

git pull origin HEAD

- Скачать все ветки с origin, но не мержить их в локальный репозиторий:

git fetch origin

- Аналогично предыдущему, но только для одной заданной ветки:

git fetch origin master

- Начать работать с веткой some_branch (уже существующей):

git checkout -b some_branch origin/some_branch

- Создать новый бранч (ответвится от текущего):

git branch some_branch

- Переключиться на другую ветку (из тех, с которыми уже работаем):

git checkout some_branch

- Получаем список веток, с которыми работаем:

git branch # звездочкой отмечена текущая ветвь

- Просмотреть все существующие ветви:

git branch -a # | grep something

- Замержить some_branch в текущую ветку:

git merge some_branch

- Удалить бранч (после мержа):

git branch -d some_branch

- Просто удалить бранч (тупиковая ветвь):

git branch -D some_branch

- История изменений:

git log

- История изменений в обратном порядке:

git log --reverse

- История конкретного файла:

git log file.txt

- Аналогично предыдущему, но с просмотром сделанных изменений:

git log -p file.txt

- История с именами файлов и псевдографическим изображением бранчей:

git log --stat --graph

- Изменения, сделанные в заданном коммите:

git show d8578edf8458ce06fbc5bb76a58c5ca4a58c5ca4

- Посмотреть, кем в последний раз правилась каждая строка файла:

git blame file.txt

- Удалить бранч из репозитория на сервере:

git push origin :branch-name

- Откатиться к конкретному коммиту (хэш смотрим в «git log»):

git reset --hard d8578edf8458ce06fbc5bb76a58c5ca4a58c5ca4

- Аналогично предыдущему, но файлы на диске остаются без изменений:

git reset --soft d8578edf8458ce06fbc5bb76a58c5ca4a58c5ca4

- Попытаться обратить заданный commit:

git revert d8578edf8458ce06fbc5bb76a58c5ca4a58c5ca4

- Просмотр изменений (суммарных, а не всех по очереди, как в «git log»):

git diff # подробности см в "git diff --help"

- Используем vimdiff в качестве программы для разрешения конфликтов (mergetool) по умолчанию:

git config --global merge.tool vimdiff

- Отключаем диалог «какой mergetool вы хотели бы использовать»:

git config --global mergetool.prompt false

- Отображаем табы как 4 пробела, например, в «git diff»:

git config --global core.pager 'less -x4'

- Создание глобального файла .gitignore:

git config --global core.excludesfile ~/.gitignore_global

- Разрешение конфликтов (когда оные возникают в результате мержа):

git mergetool

- Создание тэга:

git tag some_tag # за тэгом можно указать хэш коммита

- Удаление untracked files:

git clean -f

- «Упаковка» репозитория для увеличения скорости работы с ним:

git gc

- Иногда требуется создать копию репозитория или перенести его с одной машины на другую. Это делается примерно так:

mkdir -p /tmp/git-copy
cd /tmp/git-copy
git clone --bare git@example.com:afiskon/cpp-opengl-tutorial1.git
cd cpp-opengl-tutorial1.git
git push --mirror git@example.com:afiskon/cpp-opengl-tutorial2.git
Следует отметить, что Git позволяет использовать короткую запись хэшей. Вместо «d8578edf8458ce06fbc5bb76a58c5ca4a58c5ca4» можно писать «d8578edf» или даже «d857».

Дополнение: Также в 6-м пункте Мини-заметок номер 9 приводится пример объединения коммитов с помощью git rebase, а в 10-м пункте Мини-заметок номер 11 вы найдете пример объединения двух репозиториев в один без потери истории.

- Работа с сабмодулями
Более подробно сабмодули и зачем они нужны объясняется в заметке Простой кроссплатформенный OpenGL-проект на C++. Здесь упомянем самое главное.

- Добавить сабмодуль:

git submodule add https://github.com/glfw/glfw glfw

- Инициализация сабмодулей:

git submodule init

- Обновление сабмодулей, например, если после git pull поменялся коммит, на который смотрит сабмодуль:

git submodule update

- Удаление сабмодуля производится так:

Скажите git rm --cached имя_сабмодуля;
Удалите соответствующие строчки из файла .gitmodules;
Также грохните соответствующую секцию в .git/config;
Сделайте коммит;
Удалите файлы сабмодуля;
Удалите каталог .git/modules/имя_сабмодуля;
