---
title: "Running Cyberchef Locally with Docker"
description: "Running Cyberchef Locally with Docker"
date: "2022-12-26"
categories: [Lab]
tags: [cyberchef, linux, windows, docker, python]
pin: false
math: true
mermaid: true
image:
  path: "/assets/img/cyberchef/cyberchef.png"
  alt: "Cyberchef"
---

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

