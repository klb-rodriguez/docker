# Docker Templates

<div align="center">
	<a href="#">
		<img alt="Docker" title="Docker" src="https://drive.google.com/uc?export=view&id=1En6Hqeki8rjjKOyPLIsdUDUScJQ7Wv7z" width="450" style="width: 450px;">
	</a>
</div>

## TOC

- [Overview](#overview)
- [Installation](#installation)
- [How to use](#how-to-use)
- [Contributors](#contributors)
- [Links](#links)
- [License](#license)

## Overview

The purpose of this repository is to provide the Dockerfile files necessary for the creation of Docker Images, required for the operation of PHP-based applications with Symfony 2 or higher framework and Laravel.

## Installation

Listed below are the steps for the installation of the software required for the correct operation of the Docker repository:

### 1 Software requirements

| Software | Version |
| -------- | ------- |
| Docker   | \>=18.x |
| Git      | \>=2.20 |

### 2 Docker installation

Docker installation steps are described in the following link: [**Click here**](https://docs.docker.com/engine/install/), it is recommended the configuration of using Docker as a non-root user on Linux.

### 3 Git installation

Git installation steps are described in the following link: [**Click here**](https://git-scm.com/downloads).

### 4 Clone project

To clone the project run the following command as non-root user:

**Repository**

```bash
git clone https://github.com/klb-rodriguez/docker.git
```

### 5 Build image

Go to the directory of the image you want to create, for instance if you want to create the apache image using php 7.0 go to the `Dockerfile/php/7.0` directory, instead, if you want to crate the apache image using php 5.6 go to the `Dockerfile/php/5.6` directory.
Run the following command inside the php version root direactory:

**Apache and php 7.0**

```bash
docker build -t php:7.0-cst .
```

**Apache and php 5.6**

```bash
docker build -t php:5.6-cst .
```

**Note**: _Do not omit the period at the end of the command._

### 6 Checking the created docker image

To verify that the image has been successfully created, run the following command:

```bash
docker images
```

TAn image with the name php:7.0-cst or php:5.6-cst should appear, something similar to the following (the IMAGE ID, CREATED and SIZE may vary).

```bash
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
php                 7.0-cst            128c7f78f17c        18 seconds ago      746MB
php                 5.6-cst            f3be9430ca74        5 minutes ago       509MB
```

## How to use

The steps for using Docker images are described below.:

### 1 Creating the container

To create the container from the image it is necessary to go to the root directory where you have the project you want to dockerize and run the following command project you want to run and execute the following command:

```bash
docker run -d --name <containerName> -p <port>:80 -v "$PWD":/var/www/html/ -v "$PWD"/../apache_log/:/var/log/apache2/ <repository>:<tag>
```

En donde:

- **&lt;containerName&gt;**: This is the name to be given to the container to be created..
- **&lt;port&gt;**: This is the local port through which port 80 of the container will be accessed, this port can be any valid port number, just make sure that this port is not being used by another application.
- In addition, the **apache_log** folder must exist outside the root directory of the project so that the container's apache logs are stored in that directory, the path can be changed to any other to another one that is convenient for you.
- **&lt;repository&gt;**: This is the name of the docker repository, in this case as both are php iamgenes the name is php due to when the images were created they were named with that repository.
- **&lt;tag&gt;**: It is the name that is given to the version of the repository for our case it can be 7.0-cst or 5.6-cst, it will depend on which image has been created and which one you want to generate the container.

Example:

**Apache and php 7.0**

```bash
docker run -d --name app1 -p 90:80 -v "$PWD":/var/www/html/ -v "$PWD"/../apache_log/:/var/log/apache2/ php:7.0-cst
```

**Apache and php 5.6**

```bash
docker run -d --name app2 -p 91:80 -v "$PWD":/var/www/html/ -v "$PWD"/../apache_log/:/var/log/apache2/ php:5.6-cst
```

### 2 Verifying the created container

It is necessary to verify that there are no errors when creating the container from the image by executing the following command:

```bash
docker ps
```

The console should show something similar to the following:

```bash
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS             PORTS                 NAMES
2e5ce1a467ad        php:7.0-cst        "docker-php-entryp..."   8 seconds ago       Up 7 seconds       0.0.0.0:90->80/tcp    app1
a2c97f7eef06        php:5.6-cst        "docker-php-entryp..."   8 seconds ago       Up 7 seconds       0.0.0.0:91->80/tcp    app2
```

If the container appears with the name that was assigned to it when it was created, it indicates that everything has been executed correctly, if it does not appear, check if the container was closed when it was created with the command **docker ps -a**.

### 3 Web Server access

Once the container has been created, the application can be accessed from the web browser by simply executing the following path: **http://localhost:port**, where **port** is the port number given to access the container in the [**step 1**](#1-creating-the-container).

## Contributors

<div align="center">
    <table>
        <tr>
            <td align="center">
                <div align="center">
                    <a href="https://github.com/klb-rodriguez"  target="_blank"><img  style="width: 90px; height: 90px;" width="90" src="https://avatars.githubusercontent.com/u/3440216?v=4"></a><br />
                    Caleb Rodriguez<br/>
                </div>
            </td>
        </tr>
    </table>
</div>

## Links

The following are external links to help with the technologies used in the development of the project:

- Container [Docker](https://docs.docker.com/).
- Version control [Git](https://git-scm.com/doc).

## License

<a rel="license" href="https://www.gnu.org/licenses/gpl-3.0.en.html"><img alt="Licencia GNU GPLv3" style="border-width:0" src="https://upload.wikimedia.org/wikipedia/commons/9/93/GPLv3_Logo.svg" width="96" /></a>

This project is under the <a rel="license" href="https://github.com/klb-rodriguez/docker/blob/master/LICENSE">GNU General Public License v3.0</a>
