# Мидтерм: Инфраструктурный репозиторий

**Студент:** Albert Temirbekov
**Репозиторий:** https://github.com/trewqryt/infra-project
**Среда:** Ubuntu (виртуальная машина VirtualBox), Git 2.53.0, Docker

---

## Блок 1. Основы

### Задание 1.1. Настройка и первый коммит

**Что сделал:** настроил Git (имя, почта, ветка по умолчанию `main`), создал на GitHub пустой репозиторий
`infra-project` (без README, `.gitignore` и лицензии), склонировал его по SSH, создал `.gitignore` и сделал
из него первый коммит.

```bash
git config --global user.name "Albert Temirbekov"
git config --global user.email "temirbekov_a@iuca.kg"
git config --global init.defaultBranch main
git config --global --list
git clone git@github.com:trewqryt/infra-project.git
printf '.env\n*.log\n*.retry\n' > .gitignore
git add .gitignore
git commit -m "chore: add .gitignore"
git show --stat 881e503
```

Настройки Git:

<img width="757" height="95" alt="1" src="https://github.com/user-attachments/assets/9d3c2d8e-04ff-437c-9c38-46c9391c2cfd" />


Пустой репозиторий на GitHub (страница быстрой настройки, файлов в нём ещё нет):

<img width="922" height="870" alt="5" src="https://github.com/user-attachments/assets/447f26ad-db08-4a31-b7a9-5964639133a0" />


Содержимое `.gitignore` и первый коммит `881e503` (изменён только один файл `.gitignore`, 3 строки):

<img width="720" height="330" alt="2" src="https://github.com/user-attachments/assets/23cff3ee-a61a-4979-92ee-c140eac443ae" />


**Пояснение:** Git теперь знает моё имя и почту, чтобы подписывать коммиты. Файл `.gitignore` это список того,
что Git должен игнорировать: `.env` (секреты), `*.log` (логи) и `*.retry` (служебные файлы Ansible). Первый
коммит содержит только этот файл, как и требовалось.

---

### Задание 1.2. Файлы проекта

**Что сделал:** скопировал файлы из архива в репозиторий, создал файлы `.env` и `app.log` (их Git не должен
видеть), сделал три отдельных коммита и отправил всё на сервер.

```bash
cp -r ~/midterm/infra-project-starter/. ~/infra-project/
echo "DB_PASSWORD=secret123" > .env
echo "any log text" > app.log
git status

git add README.md .env.example
git commit -m "docs: add README and .env.example"
git add docker/
git commit -m "feat(docker): add Dockerfile and index page"
git add ansible/
git commit -m "feat(ansible): add ansible config and playbook"
git push -u origin main
```

Копирование файлов и `git status`. В списке нет `.env` и `app.log`:

<img width="947" height="617" alt="3" src="https://github.com/user-attachments/assets/1c21f087-7db5-4f7e-a2d1-eb0401378a4f" />

Отправка на сервер и история (4 коммита, самый нижний `881e503 chore: add .gitignore`):

<img width="952" height="486" alt="4" src="https://github.com/user-attachments/assets/91ca4b01-66ef-47a7-a326-9d05c83fa8bd" />

**Пояснение:** `.env` и `app.log` лежат в папке, но Git их не показывает, потому что они перечислены в
`.gitignore`. Поэтому они не попали на сервер. Файлы проекта закоммичены тремя отдельными коммитами: README и
`.env.example`, затем `docker/`, затем `ansible/`. `git push` отправил всё на GitHub.

---

### Задание 1.3. Просмотр изменений

**Что сделал:** дописал строку в `README.md`, сравнил изменения командами `git diff` и `git diff --staged` до и
после `git add`, затем закоммитил.

```bash
echo "Учебный инфраструктурный репозиторий: Docker и Ansible." >> README.md
git diff
git add README.md
git diff
git diff --staged
git commit -m "docs: describe project in README"
git push
```

## Блок 2. История и ветки

### Задание 2.1. История коммитов

**Что сделал:** посмотрел историю тремя командами.

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

Начало вывода `git show HEAD~1`:

```
commit 2930cf6...
Author: Albert Temirbekov <temirbekov_a@iuca.kg>

    feat(ansible): add ansible config and playbook
```

**Пояснение:** `HEAD` это текущий коммит (`2537ce5`), а `HEAD~1` его родитель, то есть коммит на один шаг назад:
**`2930cf6` с сообщением `feat(ansible): add ansible config and playbook`**.

---

### Задание 2.2. Ветка и слияние

**Что сделал:** создал ветку `feature/healthcheck`, добавил в `docker/Dockerfile` строку `HEALTHCHECK`, вернулся
в `main`, слил ветку и удалил её.

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

Слияние (в выводе есть слово **Fast-forward**):

<img width="777" height="277" alt="6" src="https://github.com/user-attachments/assets/28b75c8c-bbbd-4c30-9453-de02a2632a08" />

Удаление ветки и отправка на сервер:

<img width="906" height="272" alt="7" src="https://github.com/user-attachments/assets/3e7f2036-c9b0-493c-b67d-be585ba125b1" />

**Пояснение:** пока я работал в ветке, в `main` новых коммитов не появилось. Поэтому Git просто передвинул
`main` вперёд на мой коммит `08393b1`, без отдельного коммита слияния. Это и называется fast-forward. Ветка
после слияния больше не нужна, поэтому я её удалил.

---

### Задание 2.3. Конфликт слияния

**Что сделал:** изменил одну и ту же строку (`app_port`) в двух ветках по-разному: 9090 в `fix/app-port` и 8000 в
`main`. При слиянии Git показал конфликт, я решил его вручную.

```bash
git switch -c fix/app-port
sed -i 's/app_port: 8080/app_port: 9090/' ansible/group_vars/all.yml
git commit -am "fix(ansible): set app_port to 9090"
git switch main
sed -i 's/app_port: 8080/app_port: 8000/' ansible/group_vars/all.yml
git commit -am "fix(ansible): set app_port to 8000"
git merge fix/app-port
```

Файл `all.yml` **с маркерами** конфликта:

<img width="801" height="212" alt="8" src="https://github.com/user-attachments/assets/528d516a-0ca1-4ecc-95e3-b6fbba5f9683" />

Файл после решения конфликта (выбран порт 9090, маркеров нет):

<img width="922" height="381" alt="9" src="https://github.com/user-attachments/assets/a67dc76e-2967-43cf-90ac-19cadc57d5d0" />

Завершение слияния и граф истории (виден merge-коммит `ccf8d4e`):

<img width="761" height="351" alt="10" src="https://github.com/user-attachments/assets/18c8df19-35cb-42a4-a4e5-efe41934c152" />

**Пояснение:** Git не может сам решить, какой порт правильный (8000 или 9090), поэтому он пометил спорное место
маркерами `<<<<<<<`, `=======`, `>>>>>>>`. Я оставил значение 9090, удалил маркеры и закоммитил результат. В
графе видно две линии, которые сходятся в коммит слияния.

---

## Блок 3. Удалённый репозиторий и Merge Request

### Задание 3.1. Получение изменений с сервера

**Что сделал:** отредактировал `README.md` прямо на GitHub (кнопка Edit) и сохранил коммитом в `main`, затем
подтянул его на свой компьютер.

```bash
git pull
git log --oneline
```

В истории появился коммит из веб-интерфейса **`0c7a82d Update README.md`** (виден на скриншоте в задании 5.2:
стоит между `ccf8d4e` и `c4e9464`).


### Задание 3.2. Merge Request (Pull Request)

**Что сделал:** создал ветку `feature/ansible-curl`, добавил в `ansible/playbook.yml` задачу установки `curl`,
отправил ветку на сервер, открыл Pull Request в `main`, слил его на GitHub и подтянул результат.

```bash
git switch -c feature/ansible-curl
git commit -am "feat(ansible): add curl installation task"
git push -u origin feature/ansible-curl
git switch main
git pull
grep -n curl ansible/playbook.yml
```

Описание Pull Request: «Добавлена задача установки curl в `ansible/playbook.yml`, чтобы утилита была доступна на
серверах для диагностики и скачивания файлов».

Результат после слияния:

```
Updating 0c7a82d..686419f
Fast-forward
 ansible/playbook.yml | 5 +++++
 1 file changed, 5 insertions(+)
23:    - name: Install curl
25:        name: curl
```

Слитый Pull Request виден в истории как коммит `686419f Merge pull request #1 from
trewqryt/feature/ansible-curl` (скриншот в задании 5.2).


### Задание 3.3. Уборка веток

**Что сделал:** удалил слитую ветку локально и на сервере и очистил список удалённых веток.

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

**Пояснение:** после слияния ветка `feature/ansible-curl` больше не нужна. В списке `git branch -a` её нет ни
среди локальных, ни среди удалённых веток.

---

## Блок 4. Docker

### Задание 4.1. Собрать и запустить образ

**Что сделал:** в новой ветке `feature/docker-page` заменил `docker/index.html` на страницу со своим именем,
добавил `EXPOSE 80` в `Dockerfile`, собрал образ, запустил контейнер и проверил страницу через `curl`, затем
оформил изменения Pull Request'ом.

```bash
git switch -c feature/docker-page
sed -i '/^COPY/a EXPOSE 80' docker/Dockerfile
docker build -t infra-web:1.0 docker/
docker run -d --name infra-web -p 8080:80 infra-web:1.0
curl http://localhost:8080
docker rm -f infra-web
git add docker/ README.md
git commit -m "feat(docker): add personal page and expose port"
git push -u origin feature/docker-page
```

`Dockerfile` со строкой `EXPOSE 80` после `COPY`:

<img width="947" height="177" alt="11" src="https://github.com/user-attachments/assets/006de53d-78ef-447f-95a4-c78199f79e80" />

Запуск контейнера и проверка страницы (`curl` показывает мою страницу), затем контейнер удалён:

<img width="947" height="277" alt="12" src="https://github.com/user-attachments/assets/a417f5eb-c53e-4ce1-a7f6-af0d081631f1" />


Раздел «Запуск в Docker» добавлен в `README.md`:

````markdown
## Запуск в Docker

```bash
docker build -t infra-web:1.0 docker/
docker run -d --name infra-web -p 8080:80 infra-web:1.0
curl http://localhost:8080
```
````

Слитый Pull Request виден в истории как коммит `82d8e5d Merge pull request #2 from
trewqryt/feature/docker-page` (скриншоты в задании 5.2).

> страницы Pull Request #2 на GitHub со статусом **Merged**.

**Пояснение:** `docker build` собрал образ с веб-сервером nginx и моей страницей. `docker run -p 8080:80`
запустил контейнер и пробросил порт 80 контейнера на порт 8080 компьютера, поэтому `curl` по адресу
`localhost:8080` показывает страницу с моим именем. `EXPOSE 80` документирует, что контейнер слушает порт 80.
После проверки контейнер удалён командой `docker rm -f`.

---

## Блок 5. Исправление ошибок и контрольные вопросы

### Задание 5.1. Отмена изменений и правка коммита

**Что сделал:** дописал лишнюю строку в `docker/Dockerfile` и вернул файл командой `git restore`. Затем сделал
коммит с опечаткой в сообщении (`updte`) и исправил его командой `git commit --amend` до `git push`.

```bash
echo "лишняя строка" >> docker/Dockerfile
git restore docker/Dockerfile
echo "Дополнительная заметка." >> README.md
git commit -am "docs: updte readme"
git commit --amend -m "docs: update readme"
git push
```

В истории на сервере остался коммит с правильным сообщением **`d8d7bf2 docs: update readme`**, опечатки
`updte` нет (скриншот ниже, в задании 5.2).

>  команды `git restore` и `git commit --amend`, если он у вас есть.

**Пояснение:** `git restore` отменяет изменения в файле, которые ещё не закоммичены: файл возвращается к
состоянию последнего коммита. `git commit --amend -m` заменяет сообщение последнего коммита. Это безопасно,
пока коммит не отправлен на сервер, поэтому я исправил сообщение до `git push`.

---

### Задание 5.2. Отмена опубликованного коммита

**Что сделал:** удалил из `.gitignore` строку `*.log`, закоммитил и отправил (`0e5fc8b`). Затем отменил этот
коммит командой `git revert` и снова отправил.

```bash
sed -i '/^\*\.log$/d' .gitignore
git commit -am "chore: remove *.log from gitignore"
git push
git revert 0e5fc8b --no-edit
git push
```

История после удаления строки (`0e5fc8b chore: remove *.log from gitignore` уже на сервере, HEAD на нём):

<img width="957" height="445" alt="13" src="https://github.com/user-attachments/assets/097420af-f746-4c00-8e87-fd97e56016fd" />

Откат: создан коммит `40a913f Revert "chore: remove *.log from gitignore"`, `.gitignore` снова содержит
`*.log`:

<img width="956" height="927" alt="14" src="https://github.com/user-attachments/assets/4e1a5456-f791-4898-b725-3680a12dac92" />

**Пояснение:** для коммита, который уже отправлен на сервер, используют `git revert`, а не удаляют его из
истории. Другие люди могли уже скачать этот коммит, а удаление из истории потребовало бы force-push, который
ломает их копии репозитория (и запрещён правилами задания). `git revert` не стирает прошлое, а добавляет новый
коммит, который отменяет изменение.

---

### Задание 5.3. Контрольные вопросы

**1. Что такое коммит и зачем нужна область индекса (staging area)?**
Коммит это сохранённый «снимок» проекта с автором, датой и сообщением. Индекс это промежуточная зона, куда я
сначала добавляю нужные изменения командой `git add`. Так я сам выбираю, что попадёт в следующий коммит
(например, в задании 1.2 я разделил файлы на три коммита).

**2. Что такое ветка в Git?**
Ветка это подвижный указатель на коммит, отдельная линия разработки. В ветке можно работать и пробовать
изменения, не трогая `main`, а потом слить результат обратно.

**3. Что такое HEAD?**
HEAD это указатель на то место, где я сейчас нахожусь: обычно на текущую ветку и её последний коммит. Новые
коммиты создаются от HEAD, а `HEAD~1` это коммит на шаг раньше.

**4. Чем `git fetch` отличается от `git pull`?**
`git fetch` только скачивает новые данные с сервера и ничего не меняет в моей рабочей ветке. `git pull` делает
`fetch` и сразу вливает скачанные изменения в текущую ветку.

**5. Почему файл `.env` нельзя добавлять в репозиторий?**
В `.env` хранятся пароли и токены. Если он попадёт в репозиторий, секреты останутся в истории навсегда, даже
после удаления файла, и их увидят все, у кого есть доступ. Поэтому `.env` внесён в `.gitignore`, а в
репозитории лежит только шаблон `.env.example` без настоящих значений.
