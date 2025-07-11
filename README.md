# 📊 Ansible: Развертывание Prometheus, Node Exporter и Grafana
Репозиторий содержит Ansible-плейбуки для автоматизированного развертывания Prometheus, Node Exporter и Grafana на серверах.
## Быстрый старт
**Предварительные требования**
- Ansible >= 2.9 (`ansible --version`)
- Доступ к целевым серверам по **SSH**
### 1. Клонирование репозитория
```bash
git clone git@github.com:etozhedima2001/ansible_monitoring.git
cd ansible-monitoring
```
### 2. Настройка инвентаризации (`inventory/hosts.yml`)
Отредактируйте алиасы и имена переменных
```yml
<имя алиаса> ansible_host="{{ server1_IP }}" ansible_connection=ssh ansible_user="{{ server1_user  }}"
<имя алиаса> ansible_host="{{ server2_IP  }}" ansible_connection=ssh ansible_user="{{ server2_user }}"
<имя алиаса> ansible_host="{{ server3_IP  }}" ansible_connection=ssh ansible_user="{{ server3_user }}"

# Если надо
[<имя группы>]
<имя алиаса>
<имя алиаса>

[<имя группы групп>:children]
<имя группы>
<имя группы>
```
### 3. Создание файла с переменными
Создайте файл `secret.yml` и добавьте переменные
```yml
server1_IP: "<ip>"
server2_IP: "<ip>"
server3_IP: "<ip>"
server1_user: "<user>"
server2_user: "<user>"
server2_user: "<user>"
```
### 4. Шифрование секретов
Зашифруйте `secret.yml`
```bash
ansible-vault encrypt --vault-od @prompt secret.yml
```
### 5. Редактирование плейбуков
Меняйте `hosts: <имя алиаса>` в плейбуках

### 6. Настройка Prometheus
На хосте с Prometheus вручную настройте файл `/etc/prometheus/prometheus.yml`<br>

```yml
  - job_name: "node"

    static_configs:
      - targets:
          - "localhost:9100"
          - "<ip>:9100"
          - "<ip>:9100"
```

### Примечания
- для установки Grafana на локалке у вас должен находится deb пакет `/tmp/grafana-enterprise_12.0.2_amd64.deb`

