## DEMO
http://demo.idcos.com:8081/osinstall/#/dashboard/main (建议使用最新版的Google Chrome浏览器访问，以获得最佳体验)

## Online communication
QQ群:120556005

Download:http://www.idcos.com/download.html

## Prerequisites

You will need the following things properly installed on your computer.

* [Git](http://git-scm.com/)
* [Node.js](http://nodejs.org/) (with NPM)
* [Bower](http://bower.io/)
* [Ember CLI](http://www.ember-cli.com/)
* [PhantomJS](http://phantomjs.org/)
* [Go](https://storage.googleapis.com/golang/go1.4.2.src.tar.gz)

## 1.Initialization DB
* Install Mysql
```
yum install mysql-server
service mysqld start
chkconfig mysqld on
```

* Import Data
```
mkdir -p /usr/yunji/
cd /usr/yunji
git clone https://github.com/idcos/osinstall-server.git
mysql -uroot < /usr/yunji/osinstall-server/doc/db/idcos-osinstall.sql
```

## 2.Deploy Server
* Install Go

```
#Download the latest stable version source code
wget https://storage.googleapis.com/golang/go1.4.2.src.tar.gz

#Copied to the user's work directory
cp go1.4.2.src.tar.gz /home/<USER>/

#Unzip
tar xf go1.4.2.src.tar.gz

#Compile
cd go/src/ && ./make.bash
```

* Configuration environment variable

```
vim ~/.bashrc
#Append
export GOROOT=~/go
export GOPATH=~/golibs
export PATH=$GOROOT/bin:$GOPATH/bin:$PATH
```
Restart terminal


* Install gb tool

`go get github.com/constabulary/gb/...`



* Deploy Server
```
cd /usr/yunji
git clone https://github.com/idcos/osinstall-server.git
cd /usr/yunji/osinstall-server/
```


Configuring database connection:
```
vim bin/idcos-os-install.json
"repo": {
    "connection": "user:password@tcp(localhost:3306)/idcos-osinstall?charset=utf8&parseTime=True&loc=Local"
},
```


Configuring PXE config directory:
```
"osInstall":{
    "pxeConfigDir":"/var/lib/tftpboot/pxelinux.cfg"
}
```


Build and run
```
gb build
cd ./bin/
./os-install-server
```

Test
```
#Then visit the test url:
http://localhost:8083/api/osinstall/v1/device/getNetworkBySn?sn=xxx&type=json

#Output:
{
  "Content": "",
  "Message": "record not found",
  "Status": "error"
}
```

## 3.Deploy UI
```
cd /usr/yunji
git clone https://github.com/idcos/osinstall-ui.git
cd /usr/yunji/osinstall-ui/
npm install
npm install -g ember-cli
npm install -g bower
bower install
ember server --proxy=http://localhost:8083/
```

## 4.Visit your app at
`http://localhost:4200/#/dashboard/main`
