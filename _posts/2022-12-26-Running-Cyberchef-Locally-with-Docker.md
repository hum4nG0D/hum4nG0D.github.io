---
title: "Running Cyberchef Locally with Docker"
subtitle: "Running Cyberchef Locally with Docker."
description: "Running Cyberchef Locally with Docker."
excerpt: "Running Cyberchef Locally with Docker."
header-img: /assets/img/cyberchef/cyberchef.png
featured-image: /assets/img/cyberchef/cyberchef.png
featured-image-alt: "Running Cyberchef Locally with Docker"
tags: [command line, cyberchef, linux, windows, docker, python]
---

![Cyberchef](/assets/img/cyberchef/cyberchef.png)

Cyberchef is a go-to tool for me and it has been for a good reason. It's easily accessible via the web by just simply going to [https://cyberchef.org](https://cyberchef.org). 

Or you can just download the zip file and host locally. Usually the Chrome extension 200OK does the job for me.

Link to Chrome extension 200OK!: [https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb?hl=en](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb?hl=en)

![200ok](/assets/img/cyberchef/200ok.png)

<br>

However, for ease of access, I created a docker image in which it will host the static cyberchef site with Flask. All you need to do is run the following command:

```bash
sudo docker pull humangod/cyberchef
sudo docker run -d -p 8080:8080 humangod/cyberchef
```

`-d` flag will run the docker container in the background. And now you will be able to access the site with http://127.0.0.1:8080 or your local ip as it will be listening on `0.0.0.0` by default. 

![docker-pull](/assets/img/cyberchef/docker-pull.png)

![docker-run](/assets/img/cyberchef/docker-run.png)

<br>

If you don't have docker, you can just install on debian, ubuntu by running:

```bash
sudo apt update
sudo apt install docker.io
```

After installing you can check the docker versoin with `sudo docker -v`.

Here are a few useful commands:

```bash
#show docker images
sudo docker images

#show running containers
sudo docker ps

#stop the container
sudo docker stop <container id>

#remove the container
sudo docker rm <container id>
```

