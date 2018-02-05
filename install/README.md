/ [mohawk](/) / [install](/install)

## Building from source

##### Create a directory for sources

```
mkdir -p ${GOPATH}/src/github.com/MohawkTSDB && cd ${GOPATH}/src/github.com/MohawkTSDB
```

##### Clone the sources from the git repository

```
git clone https://github.com/MohawkTSDB/mohawk.git
cd mohawk
```

##### Update vedor sources

```
make vendor
```

##### Build, test and install

```
make clean
make
make test
make install
```

##### Creating Mock Certifications for testing

The server requires certification to serve https requests. Users can use self signed credentials files for testing.

To create a self signed credentials use this bash commands:

```
openssl ecparam -genkey -name secp384r1 -out server.key
openssl req -new -x509 -sha256 -key server.key -out server.pem -days 3650
```

If running from source, the Makefile has a utility for generating secrets:

```
make secret
```

##### Running the server with tls enabled

Using TLS server requires certification files, default file names are server.key and server.pem .

```
mohawk --tls --gzip --port 8443
```

## Running Mohawk

```bash
mohawk
```

![Mohawk](/images/mohawk-help.gif?raw=true "Mohawk help")

```bash
mohawk --version
mohawk --help
mohawk --options help
```

## Run Mohawk container

![Mohawk](/images/install-docker.gif?raw=true "Mohawk run docker")

```bash
sudo docker run --name mohawk -v $(readlink -f ./):/root/ssh:Z yaacov/mohawk:latest
```

## Run Mohawk on Fedora/CentOS

![Mohawk](/images/install-copr.gif?raw=true "Mohawk install rpm")

```bash
sudo dnf copr enable yaacov/mohawk
sudo dnf install mohawk
```
