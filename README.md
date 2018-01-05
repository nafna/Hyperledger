# Hyperledger composer 多节点部署

  呃呃呃，看起来肯定觉得忒费劲吧，我这里纯属快开发使用，如果你没有了解过的话，还是跳过吧
  
## 准备工作

要运行Hyperledger Composer和Hyperledger Fabric，建议您至少拥有4Gb的内存。

### 以下是安装所需开发工具的先决条件：

操作系统：Centos7.1</br>
Docker：版本17.03或更高</br>
Docker-Compose：版本1.8或更高</br>
node：8.9或更高（注意版本9不支持） </br>
npm：v5.x</br>
git：2.9.x或更高版本</br>
Python：2.7.x</br>

### 开发工具的具体安装方式

#### 操作系统

centos7的操作系统我就不说了，这个是基础

#### docker 环境搭建

添加docker资源库的配置文件 <br/>
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo <br/>
通过docker资源文件下载docker <br/>
yum install docker-ce <br/>
查看dockers运行状态 <br/>
systemctl status docker <br/>
启动docker <br/>
systemctl start docker <br/>
查看安装生成的文件 <br/>
rpm -ql docker-ce <br/>
修改用户权限，把需要操作docker的用户加入docker用户组 <br/>
usermod -G docker,wheel username <br/>

#### docker-compose 环境搭建

curl -L https://get.daocloud.io/docker/compose/releases/download/1.9.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose <br/>
chmod +x /usr/local/bin/docker-compose <br/>

#### node 环境搭建

cd /usr/local/src/ <br/>
sudo wget https://nodejs.org/dist/latest-v8.x/node-v8.9.3-linux-x64.tar.gz   <br/>
sudo tar zxvf node-v8.9.3-linux-x64.tar.gz  <br/>
cd node-v8.9.3-linux-x64/bin <br/>
sudo ln -s /usr/local/src/node-v8.9.3-linux-x64/bin/node /usr/local/bin/node <br/>
sudo ln -s /usr/local/src/node-v8.9.3-linux-x64/bin/npm /usr/local/bin/npm <br/>

#### cmpm 环境搭建（用于国内替代npm）

cnpm环境搭建 <br/>
npm install -g cnpm --registry=https://registry.npm.taobao.org <br/>

#### git 环境搭建

sudo yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker <br/>
wget https://github.com/git/git/archive/v2.9.5.zip <br/>
unzip v2.9.5.zip <br/>
cd git-2.9.5 <br/>
make prefix=/usr/local/git all <br/>
sudo make prefix=/usr/local/git install <br/>
sudo vim /etc/profile <br/>
export PATH=/usr/local/git/bin:$PATH <br/>
source /etc/profile <br/>

#### python 环境搭建

yum groupinstall 'Development Tools' <br/>
yum install zlib-devel bzip2-devel  openssl-devel ncurses-devel <br/>
wget  https://www.python.org/ftp/python/3.5.0/Python-3.5.0.tar.xz <br/>
tar Jxvf  Python-3.5.0.tar.xz <br/>
cd Python-3.5.0 <br/>
./configure --prefix=/usr/local/python3 <br/>
make && make install <br/>
ln -s /usr/local/python3/bin/python3.5 /usr/local/bin/python3 <br/>
如果提示：Ignoring ensurepip failure: pip 7.1.2 requires SSL/TLS <br/>
这是原因没有安装或升级oenssl: <br/>
yum install openssl-devel <br/>
再次重复编译方案python3.5 <br/>

### Hyperledger Composer development tools

npm install -g composer-cli <br/>
npm install -g generator-hyperledger-composer <br/>
npm install -g composer-rest-server <br/>
npm install -g yo <br/>
npm install -g composer-playground <br/>
composer-playground <br/>
此时已经可以尝试http://localhost:8080/login <br/>
mkdir ~/fabric-tools && cd ~/fabric-tools <br/>
curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.zip <br/>
unzip fabric-dev-servers.zip <br/>
mkdir ~/fabric-tools && cd ~/fabric-tools <br/>
curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.tar.gz <br/>
tar xvzf fabric-dev-servers.tar.gz <br/>
cd ~/fabric-tools <br/>
./downloadFabric.sh <br/>
./startFabric.sh <br/>
./createPeerAdminCard.sh <br/>
cd ~/fabric-tools <br/>
./stopFabric.sh <br/>
./teardownFabric.sh <br/>

## 项目部署

### 单节点部署

#### yoeman 快速生成架构

yo hyperledger-composer:businessnetwork <br/>
your-network <br/>
your project discription <br/>
yourname <br/>
your e-mail <br/>
Apache-2.0 <br/>
your project namespace <br/>

#### 替换架构内容

替换mode/*.cto 文件 （可参考 org.acme.biznet.cto） <br/>
替换lib/*.js文件 （可参考 logic.js） <br/>
增加permissions.acl文件 （可参考 permissions.acl） <br/>

#### 部署

composer archive create -t dir -n . <br/>
composer runtime install --card PeerAdmin@hlfv1 --businessNetworkName tutorial-network <br/>
composer network start --card PeerAdmin@hlfv1 --networkAdmin admin --networkAdminEnrollSecret adminpw --archiveFile tutorial-network@0.0.1.bna --file networkadmin.card <br/>
composer card import --file networkadmin.card <br/>
composer network ping --card admin@tutorial-network <br/>
composer-rest-server <br/>
admin@tutorial-network <br/>
never use namespaces <br/>
No  <br/>
Yes <br/>
No <br/>
http://localhostL3000











