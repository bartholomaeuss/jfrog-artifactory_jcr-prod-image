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

See the official
[jfrog artifactory](https://jfrog.com/help/r/jfrog-installation-setup-documentation/installation-configuration)
documentation.
See also the official
[installation manual](https://jfrog.com/help/r/jfrog-installation-setup-documentation/install-artifactory-single-node-with-docker).
See also the official
[artifacts](https://releases-docker.jfrog.io/ui/repos/tree/General/docker/jfrog/artifactory-jcr/latest)