/ [mohawk](/) / [install](/install)

##### Running Mohawk:

![Mohawk](/images/mohawk-help.gif?raw=true "Mohawk help")

```bash
mohawk --version
mohawk --help
mohawk --options help
```

##### Run Mohawk container:

![Mohawk](/images/install-docker.gif?raw=true "Mohawk run docker")

```bash
sudo docker run --name mohawk -v $(readlink -f ./):/root/ssh:Z yaacov/mohawk:latest
```

##### Run Mohawk on Fedora/CentOS:

![Mohawk](/images/install-copr.gif?raw=true "Mohawk install rpm")

```bash
sudo dnf copr enable yaacov/mohawk
sudo dnf install mohawk
```
