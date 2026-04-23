# Ansible AWG Report

Инструкция по запуску плейбука, который читает `clientsTable` из контейнера `amnezia-awg2` и формирует markdown-отчет.

## 1) Что нужно

- Docker Desktop (или Docker Engine), контейнер `amnezia-awg2` должен быть запущен.
- В контейнере должен быть `sqlite3`.
- Ansible должен быть установлен в среде, из которой запускается плейбук.

> На Windows Ansible обычно запускают через WSL (Ubuntu).

## 2) Проверить и настроить переменные

Открой [vars/awg_report.yml](vars/awg_report.yml) и проверь:

- `awg_container_name`
- `awg_sqlite_db_path`
- `awg_table_name`
- `awg_column_client`
- `awg_column_rx_bytes`
- `awg_column_tx_bytes`
- `awg_column_local_subnet`

Если имена колонок отличаются в твоей БД — замени их в этом файле.

## 3) Запуск вручную

Из корня проекта запусти:

1. Проверка синтаксиса:

   ansible-playbook -i inventories/hosts.yml --syntax-check playbooks/awg_clients_report.yml

2. Выполнение:

   ansible-playbook playbooks/awg_clients_report.yml

Если запускаешь без `ansible.cfg`, укажи inventory явно:

ansible-playbook -i inventories/hosts.yml --syntax-check playbooks/awg_clients_report.yml
ansible-playbook -i inventories/hosts.yml playbooks/awg_clients_report.yml

## 4) Запуск через Ubuntu-скрипт (рекомендуется)

Скрипт: [scripts/ubuntu_run_awg_report.sh](scripts/ubuntu_run_awg_report.sh)

1. Обычный запуск:

   bash scripts/ubuntu_run_awg_report.sh

2. Если Ansible не установлен (установить и запустить):

   bash scripts/ubuntu_run_awg_report.sh --install

## 5) Где результат

Отчет сохраняется в [reports/clients_traffic_report.md](reports/clients_traffic_report.md).

## 6) Если на Windows ошибка "ansible-playbook не найден"

Запускай через WSL:

1. Установи WSL и Ubuntu.
2. В Ubuntu установи Ansible.
3. Открой проект в WSL-пути и выполняй команды из раздела "Запуск".

## 7) Быстрая проверка контейнера

Перед запуском убедись, что контейнер активен:

- docker ps

И что в нем доступен `sqlite3`:

- docker exec amnezia-awg2 sqlite3 --version
