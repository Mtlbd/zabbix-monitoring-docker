# 🚀 Deploy Zabbix with Docker

This guide shows how to quickly set up **Zabbix** using **Docker** and **Docker Compose**. This is a fast way to run Zabbix for testing, development, or even production (with proper configurations).

---

## 📅 Requirements

- Docker
- Docker Compose

Install Docker and Docker Compose if they are not already installed:

```bash
sudo apt update
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker --now
```

---

## 📂 Folder Structure

Create a project directory:

```bash
mkdir ~/zabbix-docker && cd ~/zabbix-docker
```

---

## 📄 Create `docker-compose.yml`

Create the file `docker-compose.yml` with the following content:

```yaml
version: '3.5'

services:
  mysql-server:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: zabbixroot
      MYSQL_DATABASE: zabbix
      MYSQL_USER: zabbix
      MYSQL_PASSWORD: zabbixpass
    volumes:
      - mysql_data:/var/lib/mysql

  zabbix-server:
    image: zabbix/zabbix-server-mysql:alpine-latest
    environment:
      DB_SERVER_HOST: mysql-server
      MYSQL_USER: zabbix
      MYSQL_PASSWORD: zabbixpass
      MYSQL_DATABASE: zabbix
    depends_on:
      - mysql-server
    ports:
      - "10051:10051"

  zabbix-web:
    image: zabbix/zabbix-web-nginx-mysql:alpine-latest
    environment:
      DB_SERVER_HOST: mysql-server
      MYSQL_USER: zabbix
      MYSQL_PASSWORD: zabbixpass
      MYSQL_DATABASE: zabbix
      ZBX_SERVER_HOST: zabbix-server
      PHP_TZ: Asia/Tehran
    ports:
      - "8080:8080"
    depends_on:
      - zabbix-server

volumes:
  mysql_data:
```

---

## 🚀 Start Zabbix Stack

Run the stack with Docker Compose:

```bash
docker-compose up -d
```

Verify containers:

```bash
docker ps
```

---

## 🌐 Access Zabbix UI

Open your browser and go to:

```
http://<your-server-ip>:8080
```

Default credentials:

- Username: `Admin`
- Password: `zabbix`

---

## 🔧 Stop or Remove Stack

To stop the services:

```bash
docker-compose down
```

To remove volumes too:

```bash
docker-compose down -v
```

---

> Created by [Mohammad Talebi](https://linkedin.com/in/mtlbd) – DevOps Engineer 👨‍💻

