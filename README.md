# Infra project

Учебный инфраструктурный репозиторий курса 

<!-- Допишите сюда -->
Учебный инфраструктурный репозиторий: Docker и Ansible.
Изменено через веб-интерфейс GitHub.

## Запуск в Docker

```bash
docker build -t infra-web:1.0 docker/
docker run -d --name infra-web -p 8080:80 infra-web:1.0
curl http://localhost:8080
```
