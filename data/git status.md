# git status

- Команда `git status` всегда подскажет, что происходит с файлом: например, он добавлен в список «на коммит» или ещё вообще не отслеживается, или изменён.
- `git status` показывает явно следующие состояния файлов: `untracked`, `staged` и `modified`.
- `git status` подсказывает, какие команды можно выполнить, чтобы поменять состояние файла.

# Как читать git status

Частая ошибка при использовании Git — закоммитить лишнее или, наоборот, забыть добавить важный файл в коммит. Этого легко избежать, если не забывать проверять статусы файлов с помощью команды `git status`. Как читать её вывод, покажем в этом уроке.

### Какие состояния показывает `git status`

Большинство файлов в типичном проекте будут находиться в состоянии `tracked` (то есть закоммичены и не изменены после коммита). Вы не увидите это состояние в выводе команды `git status` — иначе она бы каждый раз выводила список вообще всех файлов проекта.

В итоге `git status` показывает только следующие состояния файлов:

- `staged` (`Changes to be committed` в выводе `git status`);
- `modified` (`Changes not staged for commit`);
- `untracked` (`Untracked files`).

### Подготавливаем репозиторий

Чтобы попрактиковаться, инициализируйте новый репозиторий `~/dev/git-status-lesson`. Создайте в нём файл `README.md` и закоммитьте его.

Скопировать кодBASH

```bash
$ cd ~/dev
$ mkdir git-status-lesson
$ cd git-status-lesson
$ git init
# тут Git выведет что-нибудь, но мы это пропустим
$ touch README.md
$ git add README.md
$ git commit -m 'Добавить README'
~~~~# по традиции первым создадим и закоммитим файл README.md
```

Дальше вы будете добавлять в репозиторий файлы и смотреть на их статусы.

### Типичные варианты вывода `git status`

Рассмотрим четыре примера состояний, в которых может находиться ваш репозиторий.

1. **Нет ни `staged`, ни `modified`, ни `untracked`файлов.**

Если ничего не менять в `git-status-lesson` после первого коммита, то в нём не должно быть ни изменённых файлов (`modified`), ни новых (`untracked`), ни добавленных в список на коммит (`staged`). Вызовите команду `git status`. Её вывод будет примерно таким.

Скопировать кодBASH

```bash
$ git status
On branch master
nothing to commit, working tree clean
```

Это означает, что в репозитории нет новых или изменённых файлов. Последняя строка `nothing to commit, working tree clean` буквально переводится как «нечего коммитить, рабочая директория чиста».

Первая строка `On branch master` сообщает, что текущая ветка — `master`.

1. **Найдены неотслеживаемые файлы.**

Создайте в папке `~/dev/git-status-lesson` файл `fileA.txt`. Теперь в репозитории есть новый файл в состоянии `untracked`. Снова вызовите команду `git status`. Результат будет таким.

Скопировать кодBASH

```bash
$ touch fileA.txt
$ git status
On branch master
Untracked files: # найдены неотслеживаемые файлы
  (use "git add <file>..." to include in what will be committed)
        fileA.txt

nothing added to commit but untracked files present (use "git add" to track)
```

Файл `fileA.txt` отображается в секции неотслеживаемых файлов — `Untracked files`. Это значит, что он не был добавлен в репозиторий через `git add`.

💡 Обратите внимание: в самом выводе `git status` есть подсказка, какую команду использовать, чтобы добавить файл в список на коммит: **Use** `git add <file>` **to include in what will be committed** (англ. «используйте `git add <file>`, чтобы добавить в список на коммит»).

Добавьте `fileA.txt` в staging area с помощью `git add` и снова запросите `git status`.

Скопировать кодBASH

```bash
$ git add fileA.txt
$ git status
On branch master
Changes to be committed: # новая секция
  (use "git restore --staged <file>..." to unstage)
        new file:   fileA.txt
```

💡 В этот раз `git status` подсказывает, что существует команда `git restore`. Мы познакомим вас с ней в одном из будущих уроков.

Теперь `fileA.txt` находится в секции `Changes to be committed` (англ. «изменения, которые попадут в коммит»). Если сейчас выполнить коммит, то в репозитории будет зафиксирована текущая версия этого файла. Закоммитьте его.

Скопировать кодBASH

```bash
$ git commit -m 'Добавить файл fileA.txt'
# тут будет вывод комманды commit, он нас не интересует
$ git status
On branch master
nothing to commit, working tree clean
```

Вывод команды `git status` такой же, какой был после первого коммита: «Директория чиста».

1. **Найдены изменения, которые не войдут в коммит**

Теперь откройте файл `fileA.txt` и добавьте в него несколько слов — например, `Это файл A!`. Сохраните `fileA.txt` и вызовите команду `git status`. Её результат будет такой.

Скопировать кодBASH

```bash
# внесли в fileA.txt правки
# запросили статус
$ git status
On branch master
Changes not staged for commit: # ещё одна секция
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   fileA.txt
```

Файл `fileA.txt` был изменён, но ещё не добавлен в staging area после этого. Так он оказался в секции `Changes not staged for commit` (англ. «изменения, которые не подготовлены к коммиту»). Эта секция соответствует статусу `modified`.

Подготовьте правки к коммиту с помощью `git add`.

Скопировать кодBASH

```bash
$ git add fileA.txt
$ git status
On branch master
Changes to be committed: # все изменения готовы к коммиту
  (use "git restore --staged <file>..." to unstage)
        modified:   fileA.txt
```

Теперь в коммит попадёт уже новая версия файла `fileA.txt`.

💡 Обратите внимание: хотя вывод команды `git status` очень похож на тот, который был после первого добавления файла `fileA.txt`, они всё же отличаются.

Когда совсем новый файл попадает в staging area, перед его названием указывается `new file`. Вот так: `new file: fileA.txt`.

Если файл уже однажды попадал в историю (с помощью коммита) и был изменён, после выполнения `git add` он будет записан уже так: `modified: fileA.txt`.

1. **Файл добавлен в staging area, но после этого изменён**

Вы добавили файл в staging area, но перед самым коммитом вспомнили важную мелочь. Например, вместо одного восклицательного знака в конце строки `Это файл A!` нужно поставить три.

Откройте текстовый редактор и добавьте нужные правки. Теперь можно выполнить коммит, но в любой непонятной ситуации сначала стоит вызвать `git status`. Он покажет следующее.

Скопировать кодBASH

```bash
# изменили fileA.txt
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
          modified:   fileA.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
          modified:   fileA.txt
```

Файл попал и в `staged` (`Changes to be committed`), и в `modified` (`Changes not staged for commit`). В staging area находится версия файла с одним восклицательным знаком, а в `Changes not staged for commit` — уже изменённая версия, с тремя.

Чтобы закоммитить самую свежую версию файла, нужно снова выполнить `git add` перед коммитом.