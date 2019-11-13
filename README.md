# Daily Flugel

## Prerequisites
**DOCKER** https://www.docker.com/
* Windows: https://docs.docker.com/docker-for-windows/install/
  * System Requirements
    * Windows 10 64-bit: Pro, Enterprise, or Education (Build 15063 or later).
    * Hyper-V and Containers Windows features must be enabled.
  * The following hardware prerequisites are required to successfully run Client Hyper-V on Windows 10:
    * 64 bit processor with Second Level Address Translation (SLAT)
    * 4GB system RAM
    * BIOS-level hardware virtualization support must be enabled in the BIOS settings. For more information, see Virtualization.
* Mac: https://docs.docker.com/docker-for-mac/install/
* Linux: https://docs.docker.com/install/linux/docker-ce/ubuntu/
  * Usually just an `apt install` of some kind

**GIT** https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
**Github Account** https://github.com/

**SSH**: https://www.ssh.com/ssh/keygen/

To make your life easier, it's nice to set up ssh.

Generate a key and for now, don't use a password on the keygen as you're just going to find it frustrating

```bash
ssh-keygen -t rsa
```

Add the contents of your ssh public key to github

```bash
cat ~/.ssh/id_rsa.pub
```

copy the output and paste in the github settings -> SSH and GPG : https://github.com/settings/keys

now you should be able to use git commands on the console without having to authenticate everytime

If you did not set up ssh, then you will use git with the `https` option

## Installation

git clone this code in to the parent directory of your choice; I use `~/Projects/dailyflugel`
```bash
cd ~/Projects
git clone git@github.com:zombiesplat/dailyflugel.git
```

cd to that directory and do a `git submodule update` to get the laradock sub repo

```bash
cd dailyflugel
git submodule update
```

copy the env-example to .env

```bash
cd laradock
cp env-example .env
```

Update the laradock/.env file for your system by searching for the key and updating the value to what you see below

```dotenv
WORKSPACE_TIMEZONE=America/Phoenix

# Change the separator from : to ; on Windows
COMPOSE_PATH_SEPARATOR=:

### Docker Sync ###########################################

# If you are using Docker Sync. For `osx` use 'native_osx', for `windows` use 'unison', for `linux` docker-sync is not required
DOCKER_SYNC_STRATEGY=native_osx
```

copy the laravel env file

```bash
cd ..
cp .env.example .env
```

your .env should have the following settings for the database. The host `mysql` is because of the Docker name given 
```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=default
DB_USERNAME=default
DB_PASSWORD=secret
```

Start laradock

```bash
cd laradock
docker-compose up -d nginx mysql
```

Enter the docker workspace. This is like an ssh tunnel but to a special docker container.

```bash
docker-compose exec workspace bash
```

run composer commands from this bash prompt

```bash
composer install
```

run npm or yarn commands from this bash prompt to compile your javascript and css assets

```bash
yarn install
yarn dev
```

run the artisan key generate
```
php artisan key:generate
```

## MySQL Migrations

This is also a pretty common task but also part of your initial setup
```
php artisan migrate
```

# Regular tasks

A handy tool for development is to have the yarn watch for any changes in js or css files and
recompile. use the `watch` directive for this.

```bash
yarn watch
```
