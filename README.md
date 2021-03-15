# Web-dev
# Stair

![coverage](https://gitlab.com/ConnectivCorporation/contract/stair-lab/stair-dev/badges/develop/coverage.svg)
![pipeline](https://gitlab.com/ConnectivCorporation/contract/stair-lab/stair-dev/badges/develop/pipeline.svg)

## Installation

If your computer have installed `docker` and `docker-compose` => Go to step 1.3

### 1.1 Install Docker on Ubuntu 16.04

```markdown
sudo apt-get install apt-transport-https ca-certificates
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install -y docker-ce
sudo systemctl enable docker
sudo usermod -aG docker ${USER}
sudo reboot
```

### 1.2 Install docker-compose

- Install using python-pip:

```markdown
sudo apt-get -y install python-pip
sudo pip install docker-compose
```

- [OR] Install with the latest version of Docker Compose: [Docker compose](https://github.com/docker/compose/releases/tag/)

```markdown
sudo curl -L https://github.com/docker/compose/releases/download/1.26.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

- **Test the installation**: `docker-compose --version`

### 1.3  Install project

#### Clone project

```markdown
git clone git@gitlab.com:ConnectivCorporation/contract/stair-lab/stair-dev.git
```

#### Build & setup docker compose

```markdown
cd stair-dev
docker-compose build
docker-compose up
```

#### Setup backend

- Install composer and permission

```markdown
docker exec -it stair-server bash
cd stair-api.test.connectiv.vn/
composer install
chown -R $USER:www-data storage/ bootstrap/
chmod -R 777 storage/ bootstrap/
```

- Modify the .env file to suit your need

copy file backend/.env.example to backend/.env

```markdown
cp .env.example .env
```

- When you have the .env with your database connection set up you can run your migrations

```markdown
php artisan config:clear
php artisan migrate --seed
```

- Permit user accounts: docker

- You should be done with the basic installation and configuration.

- If ERROR: for stair-mysql  Cannot start service mysql: driver failed programming external connectivity on endpoint template-mysql

```markdown
sudo service mysql stop
docker-compose down
docker-compose up -d
```

#### Setup frontend

- Install & setup reactjs

```markdown
cd stair-dev/frontend/
cp .env.example .env
yarn
yarn start
```

#### Setup git hooks (optional)

Git hooks are scripts that Git executes before or after events such as:
commit, push, and receive.

- Get into inside root project

```markdown
cp .gitlab/hooks/pre-commit .git/hooks/pre-commit
sudo chmod a+x .git/hooks/pre-commit
```

#### Setup cron to run queue/schedule jobs

```markdown
php artisan queue:work --queue=high,default
php artisan schedule:run
```

Cron to run schedule tasks

```bash
* * * * * cd /path-to-project && php artisan schedule:run >> /dev/null 2>&1
```
