# Мидтерм: Инфраструктурный репозиторий

**Студент:** Albert Temirbekov
**Репозиторий:** https://github.com/trewqryt/infra-project
**Среда:** Ubuntu (виртуальная машина VirtualBox), Git 2.53.0, Docker

> Места для скриншотов отмечены так: **📸 СКРИНШОТ**. Положите файл в папку `screenshots/` с указанным именем,
> и картинка подтянется автоматически. Места, помеченные `<<< ВСТАВЬТЕ ВЫВОД >>>`, заполните выводом своей команды.

---

## Блок 1. Основы

### Задание 1.1. Настройка и первый коммит

Настройка Git и клонирование репозитория по SSH:

```bash
git config --global user.name "Albert Temirbekov"
git config --global user.email "temirbekov_a@iuca.kg"
git config --global init.defaultBranch main
git config --global --list
git clone git@github.com:trewqryt/infra-project.git
```

Создание `.gitignore` и первый коммит:

```bash
printf '.env\n*.log\n*.retry\n' > .gitignore
cat .gitignore
git add .gitignore
git commit -m "chore: add .gitignore"
git show --stat 881e503
```

```
<<< ВСТАВЬТЕ ВЫВОД cat .gitignore и git show --stat 881e503 >>>
```

**Пояснение:** создан пустой репозиторий `infra-project` на GitHub, склонирован по SSH. Первый коммит `881e503`
содержит только файл `.gitignore` со строками `.env`, `*.log`, `*.retry`.

**📸 СКРИНШОТ:** `git show --stat 881e503` (в коммите только `.gitignore`)

![git show первого коммита](screenshots/1-1-first-commit.png)

**📸 СКРИНШОТ:** страница репозитория `infra-project` на GitHub

![репозиторий на GitHub](screenshots/1-1-github-repo.png)

---

### Задание 1.2. Файлы проекта

Файлы из архива скопированы в репозиторий, созданы `.env` и `app.log`:

```bash
cp -r ~/midterm/infra-project-starter/. ~/infra-project/
echo "DB_PASSWORD=secret123" > .env
echo "any log text" > app.log
git status
```

Вывод `git status` (файлов `.env` и `app.log` в списке нет):

```
On branch main
Your branch is based on 'origin/main', but the upstream is gone.
  (use "git branch --unset-upstream" to fixup)

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .env.example
        README.md
        ansible/
        docker/

nothing added to commit but untracked files present (use "git add" to track)
```

Три отдельных коммита и отправка на сервер:

```bash
git add README.md .env.example
git commit -m "docs: add README and .env.example"

git add docker/
git commit -m "feat(docker): add Dockerfile and index page"

git add ansible/
git commit -m "feat(ansible): add ansible config and playbook"

git push -u origin main
```

```
[main 3885cad] docs: add README and .env.example
 2 files changed, 7 insertions(+)
[main 9acb19e] feat(docker): add Dockerfile and index page
 2 files changed, 7 insertions(+)
[main 2930cf6] feat(ansible): add ansible config and playbook
 4 files changed, 29 insertions(+)
...
To github.com:trewqryt/infra-project.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

```
2930cf6 (HEAD -> main, origin/main) feat(ansible): add ansible config and playbook
9acb19e feat(docker): add Dockerfile and index page
3885cad docs: add README and .env.example
881e503 chore: add .gitignore
```

**Пояснение:** файлы `.env` и `app.log` не отображаются в `git status`, потому что их имена перечислены в
`.gitignore`, поэтому Git их игнорирует и они не попали на сервер. История содержит `.gitignore` первым коммитом
и три отдельных коммита проекта.

**📸 СКРИНШОТ:** `git status` без `.env` и `app.log`

![git status](screenshots/1-2-git-status.png)

**📸 СКРИНШОТ:** `git push` и `git log --oneline` (4 коммита)

![git push и log](screenshots/1-2-push-log.png)

**📸 СКРИНШОТ:** страница репозитория на GitHub: список файлов без `.env` и `app.log`

![файлы в репозитории](screenshots/1-2-github-files.png)

---

### Задание 1.3. Просмотр изменений

```bash
echo "Учебный инфраструктурный репозиторий: Docker и Ansible." >> README.md
git diff
git add README.md
git diff
git diff --staged
git commit -m "docs: describe project in README"
git push
```

`git diff` до `git add` (изменение видно):

```
<<< ВСТАВЬТЕ ВЫВОД git diff до git add >>>
```

`git diff` после `git add` (вывод пустой):

```
<<< пусто >>>
```

`git diff --staged` после `git add` (изменение видно):

```
<<< ВСТАВЬТЕ ВЫВОД git diff --staged >>>
```

**Пояснение:** второй `git diff` пустой, потому что эта команда сравнивает рабочие файлы с **индексом**
(staging area). После `git add README.md` изменение попало в индекс, и рабочая копия совпадает с ним, поэтому
различий нет. Изменения, которые ждут коммита, сравниваются с последним коммитом командой `git diff --staged`,
поэтому она показывает правку README.

**📸 СКРИНШОТ:** `git diff` до `git add`

![git diff до add](screenshots/1-3-diff-before.png)

**📸 СКРИНШОТ:** пустой `git diff` и `git diff --staged` после `git add`

![git diff после add и staged](screenshots/1-3-diff-after.png)

---

## Блок 2. История и ветки

### Задание 2.1. История коммитов

```bash
git log --oneline
git log --oneline --graph --all
git show HEAD~1
```

```
2537ce5 (HEAD -> main, origin/main) docs: describe project in README
2930cf6 feat(ansible): add ansible config and playbook
9acb19e feat(docker): add Dockerfile and index page
3885cad docs: add README and .env.example
881e503 chore: add .gitignore
```

```
* 2537ce5 (HEAD -> main, origin/main) docs: describe project in README
* 2930cf6 feat(ansible): add ansible config and playbook
* 9acb19e feat(docker): add Dockerfile and index page
* 3885cad docs: add README and .env.example
* 881e503 chore: add .gitignore
```

Вывод `git show HEAD~1` (начало):

```
commit 2930cf6...
Author: Albert Temirbekov <temirbekov_a@iuca.kg>

    feat(ansible): add ansible config and playbook

diff --git a/ansible/ansible.cfg b/ansible/ansible.cfg
...
```

**Пояснение:** `HEAD~1` соответствует коммиту **`2930cf6` — `feat(ansible): add ansible config and playbook`**.
`HEAD` указывает на последний коммит (`2537ce5`), а `HEAD~1` на его родителя.

**📸 СКРИНШОТ:** вывод `git log --oneline`, `git log --oneline --graph --all`, `git show HEAD~1`

![история коммитов](screenshots/2-1-history.png)

---

### Задание 2.2. Ветка и слияние

```bash
git switch -c feature/healthcheck
echo 'HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1' >> docker/Dockerfile
git add docker/Dockerfile
git commit -m "feat(docker): add healthcheck"
git switch main
git merge feature/healthcheck
git branch -d feature/healthcheck
git push
```

Вывод `git merge` (в нём слово **Fast-forward**):

```
<<< ВСТАВЬТЕ ВЫВОД git merge feature/healthcheck >>>
```

```
Deleted branch feature/healthcheck (was 08393b1).
```

**Пояснение:** в `main` не было новых коммитов после создания ветки, поэтому слияние произошло как
**fast-forward**: указатель `main` просто переместился на коммит `08393b1`. Строка `HEALTHCHECK` теперь есть в
`docker/Dockerfile` в ветке `main`, а ветка `feature/healthcheck` удалена.

**📸 СКРИНШОТ:** `git merge feature/healthcheck` со словом Fast-forward

![fast-forward](screenshots/2-2-fast-forward.png)

**📸 СКРИНШОТ:** `cat docker/Dockerfile` со строкой HEALTHCHECK и удаление ветки

![Dockerfile и удаление ветки](screenshots/2-2-dockerfile-delete.png)

---

### Задание 2.3. Конфликт слияния

```bash
git switch -c fix/app-port
sed -i 's/app_port: 8080/app_port: 9090/' ansible/group_vars/all.yml
git commit -am "fix(ansible): set app_port to 9090"

git switch main
sed -i 's/app_port: 8080/app_port: 8000/' ansible/group_vars/all.yml
git commit -am "fix(ansible): set app_port to 8000"

git merge fix/app-port
```

Git сообщил о конфликте в `ansible/group_vars/all.yml`. Содержимое файла **с маркерами** конфликта:

```
---
<<<<<<< HEAD
app_port: 8000
=======
app_port: 9090
>>>>>>> fix/app-port
app_user: deploy
```

Конфликт решён: выбран порт 9090, маркеры удалены:

```bash
printf -- '---\napp_port: 9090\napp_user: deploy\n' > ansible/group_vars/all.yml
git add ansible/group_vars/all.yml
git commit --no-edit
git push
```

Итоговый файл (маркеров нет):

```
---
app_port: 9090
app_user: deploy
```

```
git log --oneline --graph

*   ccf8d4e (HEAD -> main, origin/main) Merge branch 'fix/app-port'
|\
| * 28e6917 (fix/app-port) fix(ansible): set app_port to 9090
* | b59b4f9 fix(ansible): set app_port to 8000
|/
* 08393b1 feat(docker): add healthcheck
* 2537ce5 docs: describe project in README
* 2930cf6 feat(ansible): add ansible config and playbook
* 9acb19e feat(docker): add Dockerfile and index page
* 3885cad docs: add README and .env.example
* 881e503 chore: add .gitignore
```

**Пояснение:** обе ветки изменили одну и ту же строку (`app_port`) по-разному (8000 в `main` и 9090 в
`fix/app-port`), и Git не смог слить их автоматически. Я вручную выбрал значение 9090, удалил маркеры
`<<<<<<<`, `=======`, `>>>>>>>` и завершил слияние коммитом `ccf8d4e`. Он виден в графе как merge-коммит.

**📸 СКРИНШОТ:** `cat ansible/group_vars/all.yml` с маркерами конфликта

![файл с маркерами](screenshots/2-3-conflict-markers.png)

**📸 СКРИНШОТ:** итоговый файл без маркеров

![файл без маркеров](screenshots/2-3-resolved.png)

**📸 СКРИНШОТ:** `git log --oneline --graph` с merge-коммитом

![граф с merge-коммитом](screenshots/2-3-graph.png)

---

## Блок 3. Удалённый репозиторий и Merge Request

### Задание 3.1. Получение изменений с сервера

README.md отредактирован через веб-интерфейс GitHub (кнопка Edit) и сохранён коммитом в `main`. Затем локально:

```bash
git pull
git log --oneline
```

```
<<< ВСТАВЬТЕ ВЫВОД git pull и git log --oneline (виден коммит из веб-интерфейса) >>>
```

**Пояснение:** `git pull` скачал с сервера коммит `0c7a82d Update README.md`, сделанный через веб-интерфейс, и
добавил его в локальную историю `main`. Он виден в `git log --oneline` между `ccf8d4e` и `c4e9464`.

**📸 СКРИНШОТ:** GitHub: вкладка Commits с коммитом из веб-интерфейса

![коммит из веб-интерфейса](screenshots/3-1-web-commit.png)

**📸 СКРИНШОТ:** `git pull` и `git log --oneline`

![git pull](screenshots/3-1-pull.png)

---

### Задание 3.2. Merge Request

```bash
git switch -c feature/ansible-curl
# в ansible/playbook.yml добавлена задача:
#     - name: Install curl
#       ansible.builtin.package:
#         name: curl
#         state: present
git commit -am "feat(ansible): add curl installation task"
git push -u origin feature/ansible-curl
```

Открыт Pull Request `feature/ansible-curl` → `main` с описанием:

> Добавлена задача установки curl в `ansible/playbook.yml`, чтобы утилита была доступна на серверах для
> диагностики и скачивания файлов.

Pull Request слит через веб-интерфейс GitHub, затем:

```bash
git switch main
git pull
grep -n curl ansible/playbook.yml
```

```
Updating 0c7a82d..686419f
Fast-forward
 ansible/playbook.yml | 5 +++++
 1 file changed, 5 insertions(+)
23:    - name: Install curl
25:        name: curl
```

**Пояснение:** изменение сделано в отдельной ветке и попало в `main` через Pull Request с описанием. Сообщение
коммита оформлено по Conventional Commits (`feat(ansible): ...`). Задача установки curl есть в
`ansible/playbook.yml` в ветке `main`.

**📸 СКРИНШОТ:** GitHub: открытый Pull Request с описанием «что и зачем изменено»

![PR с описанием](screenshots/3-2-pr-open.png)

**📸 СКРИНШОТ:** GitHub: тот же Pull Request со статусом **Merged**

![PR слит](screenshots/3-2-pr-merged.png)

**📸 СКРИНШОТ:** `git pull` и `grep -n curl ansible/playbook.yml`

![pull и grep](screenshots/3-2-pull-grep.png)

---

### Задание 3.3. Уборка веток

```bash
git branch -d feature/ansible-curl
git push origin --delete feature/ansible-curl
git fetch --prune
git branch -a
```

```
Deleted branch feature/ansible-curl (was c4e9464).
To github.com:trewqryt/infra-project.git
 - [deleted]         feature/ansible-curl
  fix/app-port
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
```

**Пояснение:** слитая ветка удалена локально и на сервере. После `git fetch --prune` ветки
`feature/ansible-curl` нет ни среди локальных, ни среди удалённых.

**📸 СКРИНШОТ:** `git branch -a` после `git fetch --prune`

![git branch -a](screenshots/3-3-branches.png)

---

## Блок 4. Docker

### Задание 4.1. Собрать и запустить образ

```bash
git switch main
git pull
git switch -c feature/docker-page
```

Страница `docker/index.html`:

```html
<!DOCTYPE html>
<html lang="ru">
<head><meta charset="utf-8"><title>infra-project</title></head>
<body><h1>Albert Temirbekov — infra-project</h1></body>
</html>
```

В `docker/Dockerfile` строка `EXPOSE 80` добавлена после `COPY`:

```bash
sed -i '/^COPY/a EXPOSE 80' docker/Dockerfile
cat docker/Dockerfile
```

```
<<< ВСТАВЬТЕ ВЫВОД cat docker/Dockerfile (FROM, COPY, EXPOSE 80, HEALTHCHECK) >>>
```

Сборка, запуск и проверка:

```bash
docker build -t infra-web:1.0 docker/
docker run -d --name infra-web -p 8080:80 infra-web:1.0
curl http://localhost:8080
docker rm -f infra-web
```

Последние строки `docker build`:

```
<<< ВСТАВЬТЕ ПОСЛЕДНИЕ СТРОКИ docker build >>>
```

Вывод `curl http://localhost:8080`:

```
<<< ВСТАВЬТЕ ВЫВОД curl >>>
```

Раздел, добавленный в `README.md`:

````markdown
## Запуск в Docker

```bash
docker build -t infra-web:1.0 docker/
docker run -d --name infra-web -p 8080:80 infra-web:1.0
curl http://localhost:8080
```
````

Коммит и Pull Request:

```bash
git add docker/ README.md
git commit -m "feat(docker): add personal page and expose port"
git push -u origin feature/docker-page
```

Pull Request `feature/docker-page` → `main` создан с описанием и слит через веб-интерфейс. После слияния:

```
Updating 686419f..82d8e5d
Fast-forward
 README.md         | 8 ++++++++
 docker/Dockerfile | 1 +
 docker/index.html | 4 ++--
 3 files changed, 11 insertions(+), 2 deletions(-)
```

**Пояснение:** образ собран на основе `nginx:1.27-alpine`, контейнер запущен с пробросом порта 8080 на 80 и
отдаёт страницу с моим именем. Изменения оформлены коммитом в формате Conventional Commits и слиты в `main`
через Pull Request.

**📸 СКРИНШОТ:** последние строки `docker build` (успешная сборка)

![docker build](screenshots/4-1-docker-build.png)

**📸 СКРИНШОТ:** `curl http://localhost:8080` с вашей страницей

![curl](screenshots/4-1-curl.png)

**📸 СКРИНШОТ:** `cat docker/Dockerfile` со строкой `EXPOSE 80`

![Dockerfile](screenshots/4-1-dockerfile.png)

**📸 СКРИНШОТ:** GitHub: слитый Pull Request из `feature/docker-page`

![PR docker-page](screenshots/4-1-pr-merged.png)

---

## Блок 5. Исправление ошибок и контрольные вопросы

### Задание 5.1. Отмена незакоммиченных изменений и правка коммита

Отмена незакоммиченной правки:

```bash
echo "лишняя строка" >> docker/Dockerfile
git status
git restore docker/Dockerfile
git status
```

```
<<< ВСТАВЬТЕ ВЫВОД git status до и после git restore >>>
```

Правка сообщения коммита с опечаткой (до `git push`):

```bash
echo "Дополнительная заметка." >> README.md
git commit -am "docs: updte readme"
git commit --amend -m "docs: update readme"
git log --oneline
git push
```

Фрагмент `git log --oneline` (в сообщении нет опечатки `updte`):

```
d8d7bf2 docs: update readme
82d8e5d Merge pull request #2 from trewqryt/feature/docker-page
```

**Пояснение:** `git restore` вернул файл `docker/Dockerfile` к состоянию последнего коммита, лишняя строка
пропала. `git commit --amend -m` заменил сообщение последнего локального коммита, а так как это произошло
до `git push`, коммита с опечаткой `updte` на сервере нет.

**📸 СКРИНШОТ:** `git restore`, `git commit --amend` и `git log --oneline`

![restore и amend](screenshots/5-1-restore-amend.png)

---

### Задание 5.2. Отмена опубликованного коммита

```bash
sed -i '/^\*\.log$/d' .gitignore
git commit -am "chore: remove *.log from gitignore"
git push
git revert 0e5fc8b --no-edit
git push
```

```
[main 40a913f] Revert "chore: remove *.log from gitignore"
 1 file changed, 1 insertion(+)
```

```
40a913f (HEAD -> main, origin/main, origin/HEAD) Revert "chore: remove *.log from gitignore"
f2c29ec docs: update readme
0e5fc8b chore: remove *.log from gitignore
d8d7bf2 docs: update readme
82d8e5d Merge pull request #2 from trewqryt/feature/docker-page
```

`cat .gitignore` (строка `*.log` снова на месте):

```
.env
*.log
*.retry
```

**Пояснение:** коммит удаления `*.log` (`0e5fc8b`) отправлен на сервер, затем отменён коммитом `40a913f`
(`Revert`). Для коммита, который уже отправлен на сервер, используют `git revert`, а не удаляют его из
истории, потому что другие люди могли уже получить этот коммит, а переписывание опубликованной истории
потребовало бы force-push, который ломает чужие копии репозитория (и запрещён правилами задания).
`git revert` создаёт новый коммит-отмену и сохраняет историю целой.

**📸 СКРИНШОТ:** `git log --oneline` с коммитом удаления `*.log` и коммитом `Revert ...`

![revert](screenshots/5-2-revert.png)

---

### Задание 5.3. Контрольные вопросы

**1. Что такое коммит и зачем нужна область индекса (staging area)?**
Коммит это сохранённый снимок состояния проекта с автором, датой и сообщением. Индекс это промежуточная зона,
в которую я добавляю нужные изменения командой `git add` перед коммитом. Благодаря ей я выбираю, какие именно
изменения войдут в следующий коммит (например, разбить правки на несколько коммитов, как в задании 1.2).

**2. Что такое ветка в Git?**
Ветка это подвижный указатель на коммит. Она позволяет вести отдельную линию разработки, не затрагивая
основную ветку, а потом слить результат обратно.

**3. Что такое HEAD?**
HEAD это указатель на текущее место в репозитории: обычно на текущую ветку, а через неё на последний коммит, на
котором я работаю. Новые коммиты создаются от HEAD, а `HEAD~1` это его родитель.

**4. Чем `git fetch` отличается от `git pull`?**
`git fetch` только скачивает новые данные с сервера и ничего не меняет в рабочих файлах и текущей ветке.
`git pull` делает `fetch` и сразу вливает скачанные изменения в текущую ветку.

**5. Почему файл `.env` нельзя добавлять в репозиторий?**
В `.env` обычно хранятся пароли, токены и другие секреты. Попав в историю Git, они остаются там даже после
удаления файла и видны всем, кто имеет доступ к репозиторию. Поэтому `.env` добавляют в `.gitignore`, а в
репозитории хранят только шаблон `.env.example` без реальных значений.
