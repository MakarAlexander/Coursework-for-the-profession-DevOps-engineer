# Coursework-for-the-profession-DevOps-engineer "Макарцев Александр Владимирович"
Курсовая работа на профессии "DevOps-инженер с нуля"

Основная задача развернуть облачную инфраструктуру посредством Terraform и Ansible, в соответствии с [заданием](./Курсовой%20проект.md)

---

### Поднимаем инфраструктуру через Terraform:

- Инициализируем terraform

```sh
terraform init
```

![1](./other/screenshots/1.png)

- Разворачиваем инфрастуктуру через terraform

```sh
terraform apply
```

![2](./other/screenshots/2.png)

![3](./other/screenshots/3.png)

- Выводим output-ansible-hosts в отдельный файл hosts и удаляем всё лишнее

```sh
terraform output output-ansible-hosts > ../ansible/hosts
```

- Созданная инфраструктура в Yandex-cloud

![4](./other/screenshots/4.png)

Проверка доступности ВМ через ping

```sh
ansible all -m ping
```

![5](./other/screenshots/5.png)

---

### Сайт

>Target group:

![6](./other/screenshots/6.png)

>Backend group:

![7](./other/screenshots/7.png)

>HTTP router:

![8](./other/screenshots/8.png)

>Application load balancer:

![9](./other/screenshots/9.png)

[Запускаем playbook для верстки сайтов](./ansible/playbooks/webservers-playbook.yml)

>Обязательно нужно проверить, что сервисы nginx, node_exporter и nginx_logexporter работают

```sh
ansible-playbook main.yml -t webservers
```
![10](./other/screenshots/10.png)
![11](./other/screenshots/11.png)
![12](./other/screenshots/12.png)
![13](./other/screenshots/13.png)

Проверяем, что сайт работает

```sh
curl -v 84.201.181.146:80
```

![14](./other/screenshots/14.png)
![15](./other/screenshots/15.png)

---

### Мониторинг

[Поднятие мониторинга](./ansible/playbooks/monitoring-playbook.yml) (при первой прокатке может быть ошибка импортирования дашборда в Grafana, в этом случае требуется прокатить роль ещё раз)

```sh
ansible-playbook main.yml -t grafana
```

![16](./other/screenshots/16.png)
![17](./other/screenshots/17.png)
![18](./other/screenshots/18.png)

Проверяем, что импортированные дашборды работают корректно

>NGINX Servers Metrics:

![19](./other/screenshots/19.png)
![20](./other/screenshots/20.png)

>Node Exporter Full:

![21](./other/screenshots/21.png)

---

### Логи

[Поднятие системы логирования](./ansible/playbooks/log-playbook.yml) (обязательно нужно проверить, что контейнеры поднялись. Возможна ошибка при установке пакетов для docker, прокатываем роль ещё раз)

```sh
ansible-playbook main.yml -t elasticsearch
```

![22](./other/screenshots/22.png)
![23](./other/screenshots/23.png)
![24](./other/screenshots/24.png)
![25](./other/screenshots/25.png)
![26](./other/screenshots/26.png)

[Поднятие filebeat](./ansible/playbooks/log-filebeat-playbook.yml)

```sh
ansible-playbook main.yml -t filebeat
```

![27](./other/screenshots/27.png)
![28](./other/screenshots/28.png)

Проверяем Kibana на наличие логов из Nginx

![29](./other/screenshots/29.png)
![30](./other/screenshots/30.png)

---

### Сеть