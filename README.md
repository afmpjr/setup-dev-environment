# setup-dev-environment

This repositori intends be a short/easy way to setup a local development environment on Windows.

We're going to:

## Setup the Windows Subsistem for Linux (WSL) and the Ubuntu distribution.
```
(To be done...)
```
## Essential packages
```
sudo apt install zip unzip
```

<details>
<summary>Docker</summary>

(To be done...)


```
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

</details>

<details>
<summary>Optional</summary>

<details>
<summary>PHP, Composer and Laravel</summary>

```
sudo apt install php-fpm php-mbstring php-xml php-mysql php-curl php-zip
```

### Install Composer
```
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/b
in --filename=composer```
```   
Add the following to .bashrc file (nano ~/.bashrc)
```
export PATH="~/.config/composer/vendor/bin:$PATH"
```
### Create a Laravel project using Composer
```
composer create-project --prefer-dist laravel/laravel YOUR_PROJECT_NAME
```
</details>

<details>
<summary>NodeJS, NPM,Yarn</summary>
(To be done...)

</details>


</details>