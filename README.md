# Setup your DEV Environment on Windows from scratch

This repositori intends be a short/easy way to setup a local development environment on Windows 10.

We're going to:

<details>
<summary>Required: Setup the Windows Subsistem for Linux (WSL) and the Ubuntu distribution.</summary>

Run the following command on Windows PowerShell:
```Shell
wsl --install ubuntu
```
Once the instalation is complete, you'll be able to open a new WSL Ubuntu terminal.

Update the `apt` package index
```bash
sudo apt update && sudo apt upgrade -y
```
Install the following essential packages
```bash
sudo apt install zip unzip
```
</details>

<details>
<summary>Recommended: Install Docker</summary>

Follow "Install Docker Engine on Ubuntu" documentation at https://docs.docker.com/engine/install/ubuntu/

```bash
sudo apt install -y ca-certificates curl gnupg lsb-release
```
```bash
sudo mkdir -m 0755 -p /etc/apt/keyrings && \
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```bash
sudo apt update && \
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
```bash
sudo groupadd docker
sudo usermod -aG docker $USER
sudo service docker start
```
Verify that you can run docker commands without sudo 
```bash
docker run hello-world
```
</details>

<details>
<summary>Optional: PHP, Composer and Laravel</summary>

```bash
sudo apt install php-fpm php-mbstring php-xml php-mysql php-curl php-zip
```

### Install Composer
```bash
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer```
```   
Add the following to `.bashrc` file (`nano ~/.bashrc`)
```bash
export PATH="~/.config/composer/vendor/bin:$PATH"
```
And now we're ready to create a new Laravel project
```bash
composer create-project --prefer-dist laravel/laravel YOUR_PROJECT_NAME
```
</details>

<details>
<summary>Optional: Set a MariaDB instance using Docker Compose</summary>

Create a `docker-compose.yml`  file with the following content (or adapt as to your needs):
```yaml
version: '2'
services:
  mysql:
    container_name: mariadb
    restart: always
    image: mariadb:latest
    environment:
      MYSQL_ROOT_PASSWORD: 'password'
      MYSQL_USER: 'test'
      MYSQL_PASS: 'pass'
    volumes:
      - ~/projects/database/mariadb:/var/lib/mysql
    ports:
      - 3306:3306
```
* the `volumes` specifies that the database data will be persisted on `~/projects/database/mariadb` folder.

We're now able to start the database service using the following command:
```bash
docker-compose up -d
```
</details>

<details>
<summary>Optional: Install NVM, NodeJS, NPM/Yarn</summary>

Easiest way is to install the NVM (Node Version Manager)
```bash
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash 
source ~/.bashrc
```
And then install Node using NVM
```
nvm install node
```
And now we check the installed versions
```bash
$ nvm --version
0.39.3
$ node --version
v19.7.0
$ npm --version
9.5.0
```
And we're ready to create a new project
```bash
npx create-react-app YOUR_PROJECT_NAME
```

> If you want to use Yarn and it's not yet installed, run:
```bash
npm install -g yarn
```

</details>

<details>
<summary>Optional: Run Vue projects using Docker Compose</summary>

> Previous step: **Install NVM, NodeJS, NPM/Yarn** is required

In order to run Vue applications, it's also required to install the Vue package.

```bash
npm install -g @vue/cli
```

Then we're able to create a new Vue project.

```bash
vue create YOUR_PROJECT_NAME
```

Now we can choose to either run our project using the `npm run serve` command or creating a `docker-compose` file.

So, as we aim to use Docker, let's create a `docker-compose.yml` file with the following content:

```yaml
version: '3'
services:
  app:
    # chosing the node image as below or any other (eg. latest)
    image: node:14-alpine
    # Defining the default folder of our application on the container
    working_dir: /app
    ports:
      - '8080:8080'
    volumes:
      # pointing our project root folder to the working dir defined above
      - '.:/app'
    # npm will install our vue dependencies and run our app
    command: sh -c 'npm install && npm run serve'
```

And now everything must be working. Let's run this service ...

```bash
docker-compose up
```

If there's no errors, we can just make it run in the background by adding the flag `-d`.

```bash
docker-compose up -d
```

</details>

---
At this point, your environment is ready to run either PHP Laravel, NodeJS and/or Vue projects, using the MariaDB database (if needed)
