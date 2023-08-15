# Diary

A simple diary for the web.

## ⚠️ Work in progress ⚠️

This project is still in development. You can use it, but the frontend sucks (especially on mobile). Furthermore, the backend will get a few changes in the future. If you are interested in the current state, you can look at the [development branch](https://github.com/dodaucy/diary/tree/develop).

## Setup

> [!IMPORTANT]
> It is recommended to put the project behind a reverse proxy, such as [NGINX](https://www.nginx.com/).

### Docker Compose (recommended)

If you set up the project with Docker Compose, you don't need to set up your own database. The database is automatically set up with Docker Compose.

#### 1. Install Docker and other dependencies

> [!NOTE]
> This setup is for Ubuntu/Debian. For other distros, see [Docker Compose](https://docs.docker.com/compose/install/).

> [!NOTE]
>If you use an Ubuntu derivative distro, such as Linux Mint, you may need to use `UBUNTU_CODENAME` instead of `VERSION_CODENAME`.

```bash
sudo apt update

sudo apt install docker.io ca-certificates curl gnupg git -y

sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

#### 2. Ensure Docker is running

```bash
sudo systemctl start docker

sudo systemctl enable docker
```

#### 3. Download [docker-compose.yml](https://raw.githubusercontent.com/dodaucy/diary/master/docker-compose.yml)

```bash
curl -fsSL https://raw.githubusercontent.com/dodaucy/diary/master/docker-compose.yml -o docker-compose.yml
```

#### 4. Configure

Create a `.env` file in the same directory as the `docker-compose.yml` file. See [example.env](/example.env) for an example configuration.

#### 5. Run

```bash
sudo docker compose up -d
```

> [!NOTE]
> Run `docker compose pull && docker compose build && docker compose down && docker compose up -d` to update the containers.

### Docker

#### 1. Install docker and other dependencies

Follow steps 1 and 2 from the [Docker Compose](#docker-compose-recommended) section.

#### 2. Clone and build

```bash
git clone https://github.com/dodaucy/diary.git

cd diary

sudo docker build -t diary .
```

#### 3. Configure

Create a `.env` file in the diary directory. See [example.env](/example.env) for an example configuration.

#### 4. Run

Replace `YOUR_ENDPOINT` with the endpoint you want to use. For example 127.0.0.1:8080 or 0.0.0.0:80.

```bash
sudo docker run -d -p YOUR_ENDPOINT:8000 --env-file ./env --restart always --name diary diary
```

> [!NOTE]
> Run `git pull && sudo docker build -t diary . && sudo docker stop diary && sudo docker rm diary` to update the container. After that, run the command above again.

### Manual

#### 1. Install dependencies

```bash
sudo apt update

sudo apt install python3 python3-pip python3-venv git -y
```

#### 2. Clone and build

```bash
git clone https://github.com/dodaucy/diary.git

cd diary

python3 -m venv venv

source venv/bin/activate

python3 -m pip install -r requirements.txt

deactivate
```

#### 3. Configure

Create a `.env` file in the diary directory. See [example.env](/example.env) for an example configuration.

#### 4. Run

Replace `YOUR_HOSTNAME` and `YOUR_PORT` with the hostname and port you want to use. For example 127.0.0.1 and 8080 or 0.0.0.0 and 80. If the diary is behind a reverse proxy (which is recommended), you should append `--proxy-headers --forwarded-allow-ips *` to the command. Otherwise, the rate limit will not work properly.

```bash
source venv/bin/activate && python3 -m uvicorn main:app --host YOUR_HOSTNAME --port YOUR_PORT ; deactivate
```

> [!NOTE]
> Run `git pull && source venv/bin/activate && python3 -m pip install -Ur requirements.txt && deactivate` to update the diary. After that, run the command above again.

## License

**MIT**

Copyright (C) 2022 - 2023 dodaucy
