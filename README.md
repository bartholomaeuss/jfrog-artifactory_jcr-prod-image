# JFROG/ARTIFACTORY-JCR PROD IMAGE

## Overview

### 🚀 Local Artifactory for Container Images
This Artifactory instance will serve as the central repository for all my container images.
By hosting everything locally,
I’ll no longer need an internet connection for future deployments — making the setup faster ⚡,
more reliable ✅, and fully self-contained 🔒.

### 🛠️ Initialization Phase
The initial setup of the environment requires:

* 🌐 a one-time internet connection, and

* 💻 access to a terminal.

This is necessary to install the core components — including 
the JFrog Artifactory instance 🏗️ and the base container images 📦.
Once everything is up and running, no further internet access will be required for container deployments.

### 🤖 CI/CD Automation with Tekton
All future images — including the Artifactory image itself —
will be built and pushed automatically by a Tekton CI/CD pipeline 🔁.
This pipeline ensures that images are:

* 🏷️ versioned
* 🧪 tested
* 📤 published to the local registry.

🎯 The goal is a self-sufficient container ecosystem where all dependencies are managed in-house 🏡.

## Prerequisite

### ✅ Quick Terminal Check

Before we dive in, let’s verify that your terminal is working correctly and can execute basic scripts.
Run the following test with your name as input:
First we need to check your terminal with a tiny test.

```bash
./hello_world.sh
```

You should see a message like:

`Nice job <Your Name>, this seems to work!`

If you see the message, you’re good to go! 🎉


#### 🗄️ Data Persistence

To ensure that JFrog Artifactory retains its configuration and data across container restarts or system reboots, 
we need to set up a persistent directory structure on the host system. 
The following steps create and configure the necessary folders with appropriate ownership and permissions:

```
mkdir -p ~/jfrog/artifactory/var/etc/
cd ~/jfrog/artifactory/var/etc/
touch ./system.yaml

sudo chown -R 1030:1030 ~/jfrog/artifactory/var
sudo chmod -R 777 ~/jfrog/artifactory/var
```

##### ⚠️ Note:

The UID 1030 corresponds to the Artifactory user inside the container.
chmod 777 is generous for development or testing, but in production, you should apply more restrictive permissions.

#### 🛠️ Initial Configuration

```
cat <<EOF >> system.yaml
shared:
  database:
    driver: org.postgresql.Driver
    type: postgresql
    url: jdbc:postgresql://<HOST>:5432/artifactorydb
    username: artifactory
    password: password
EOF
```

#### 🔒 Default Credentials

* 👤 User: `admin` 
* 🔑 Password: `password`

### 🪟 Windows

```bash
./provide_container.sh
```

### 🐳 More

```
sudo docker run -d --net=host -v ~/jfrog/artifactory/var/:/var/opt/jfrog/artifactory --restart=unless-stopped releases-docker.jfrog.io/jfrog/artifactory-jcr:latest
```

#### 🔧 Disable ssl within docker cli

📝 Create settings file

```
cd /etc/docker
sudo touch ./daemon.json
```

✍️ Write settings

```
cat <<EOF >> daemon.json
{
  "insecure-registries": ["<host>:8082"]
}
EOF
```

🔁 You will need to restart the service

```
sudo systemctl restart docker

```

#### 🔐 Login into Docker artifactory using docker cli

```
echo "<password>" | docker login -u <user> --password-stdin <host>:8082/docker
```

#### 📦 Push given docker image manually into local artifactory

🐳 Pull image

```
docker pull --platform linux/arm64 prom/node-exporter:latest
```

🏷 Tag image

```
docker tag prom/node-exporter:latest <host>:8082/docker-dev/prom/node-exporter/arm64:latest
```

🚀 Push image

```
docker push <host>:8082/docker-dev/prom/node-exporter/arm64:latest
```

#### 🤖 Login into Docker artifactory using kubernetes

```

```


See the official
[jfrog artifactory](https://jfrog.com/help/r/jfrog-installation-setup-documentation/installation-configuration)
documentation.
See also the official
[installation manual](https://jfrog.com/help/r/jfrog-installation-setup-documentation/install-artifactory-single-node-with-docker).
See also the official
[artifacts](https://releases-docker.jfrog.io/ui/repos/tree/General/docker/jfrog/artifactory-jcr/latest)