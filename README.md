# DOCKER NGROK IMAGE

## BUILD IMAGE

```linux
git clone https://github.com/hteen/docker-ngrok.git
cd docker-ngrok
docker build -t hteen/ngrok .
```

## RUN
* you must mount your folder (E.g `/data/ngrok`) to container `/myfiles`
* if it is the first run, it will generate the binaries file and CA in your floder `/data/ngrok`

```linux
docker run -idt --name ngrok-server \
-v /data/ngrok:/myfiles \
-e DOMAIN='tunnel.hteen.cn' hteen/ngrok /bin/sh /server.sh
```



## Build for CentOS 7.2
```
yum install gcc golang wget -y
git clone https://github.com/tutumcloud/ngrok.git
cd ngrok/
set -e
DOMAIN="xx.xx.xx"
openssl genrsa -out base.key 2048
openssl req -new -x509 -nodes -key base.key -days 10000 -subj "/CN=${DOMAIN}" -out base.pem
openssl genrsa -out device.key 2048
openssl req -new -key device.key -subj "/CN=${DOMAIN}" -out device.csr
openssl x509 -req -in device.csr -CA base.pem -CAkey base.key -CAcreateserial -days 10000 -out device.crt
cp base.pem assets/client/tls/ngrokroot.crt
GOOS=linux GOARCH=amd64
make release-server
make release-client
```
