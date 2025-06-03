# JFROG/ARTIFACTORY-JCR PROD IMAGE

### Prerequisite

```bash
./hello_world.sh
```

#### Data Persistence

```
mkdir -p ~/jfrog/artifactory/var/etc/
cd ~/jfrog/artifactory/var/etc/
touch ./system.yaml
sudo chown -R 1030:1030 ~/jfrog/artifactory/var
sudo chmod -R 777 ~/jfrog/artifactory/var
```

#### Initial Configuration

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

#### Default Credentials

User: `admin`, Password: `password`

### Windows

```bash
./provide_container.sh
```

### More

```
sudo docker run -d --net=host -v ~/jfrog/artifactory/var/:/var/opt/jfrog/artifactory --restart=unless-stopped releases-docker.jfrog.io/jfrog/artifactory-jcr:latest
```

#### Disable ssl within docker cli

Create settings file

```
cd /etc/docker
sudo touch ./daemon.json
```

Write settings

```
cat <<EOF >> daemon.json
{
  "insecure-registries": ["<host>:8082"]
}
EOF
```

You will need to restart the service

```
sudo systemctl restart docker

```

#### Login into Docker artifactory using docker cli

```
echo "<password>" | docker login -u <user> --password-stdin <host>:8082/docker
```

#### Push given docker image manually into local artifactory

```
docker pull --platform linux/arm64 prom/node-exporter:latest
```
```
docker tag prom/node-exporter:latest <host>:8082/docker-dev/prom/node-exporter/arm64:latest
```
```
docker push <host>:8082/docker-dev/prom/node-exporter/arm64:latest
```

#### Login into Docker artifactory using kubernetes

```

```


See the official
[jfrog artifactory](https://jfrog.com/help/r/jfrog-installation-setup-documentation/installation-configuration)
documentation.
See also the official
[installation manual](https://jfrog.com/help/r/jfrog-installation-setup-documentation/install-artifactory-single-node-with-docker).
See also the official
[artifacts](https://releases-docker.jfrog.io/ui/repos/tree/General/docker/jfrog/artifactory-jcr/latest)