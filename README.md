# hello-world
study a test
## 


https://www.jianshu.com/p/f0daa2d9eae6
https://github.com/Dreamacro/clash-dashboard
https://www.codenong.com/cs106602785/


局域网上利用树莓派挂clash，
实现至少在家里各个设备可以自动走代理上网。

查看树莓派系统版本
uname -a

clash核心 下载官网
https://github.com/Dreamacro/clash/releases

方法一：
下载对应版本
wget https://github.com/Dreamacro/clash/releases/download/premium/clash-linux-armv7-v1.5.0.gz 

解压
gzip -d clash-linux-armv7-v1.5.0.gz 

移动并改名
mv clash-linux-armv7-v1.5.0 /usr/local/bin/clash

增加运行权限
sudo chmod +x /usr/local/bin/clash 

# 首次运行，初始化一下config目录
clash

# 下载配置文件
（如果是root用户安装clash的,那么配置文件应放在/root/.config/clash目录下）
（如果是pi用户安装clash的,那么配置文件应放在/home/pi/.config/clash目录下）
wget -O /home/pi/.config/clash/config.yaml https://xxxx.xx/xxx

开启网页ui
下载网页ui代码：
	git clone https://github.com/Dreamacro/clash-dashboard.git

	cd clash-dashboard

切换到已经制作好产出的分支
	git checkout -b gh-pages origin/gh-pages

配置config.yaml：
Clash 的 RESTful API
	external-controller: '0.0.0.0:8989'
RESTful API 的口令
	secret: '网页访问密码'

您可以将静态网页资源（如 clash-dashboard）放置在一个目录中，clash 将会服务于 RESTful API/ui
参数应填写配置目录的相对路径或绝对路径。
external-ui: /home/pi/.config/clash/clash-dashboard

访问ip:8989/ui可见下面的网页：




方法二：
mkdir /home/pi/clash-bin
cd /home/pi/clash-bin 

下载对应版本
wget https://github.com/Dreamacro/clash/releases/download/premium/clash-linux-armv7-v1.5.0.gz 

解压
gzip -d clash-linux-armv7-2020.05.08.gz 

改名
mv clash-linux-armv7-v1.5.0 clash

给与可执行权限
sudo chmod +x clash

建立软链接
sudo ln -s clash /bin/clash

# 首次运行，初始化一下config目录
clash

# 下载配置文件
（如果是root用户安装clash的,那么配置文件应放在/root/.config/clash目录下）
（如果是pi用户安装clash的,那么配置文件应放在/home/pi/.config/clash目录下）
wget -O /home/pi/.config/clash/config.yaml https://xxxx.xx/xxx

config.yaml 配置文件请于代理提供商控制面板处获取，
并将其移动至 /home/pi/clash-bin 目录下。

配置系统代理
执行 sudo vim /etc/environment 编辑 environment 配置文件，
按 i 进入输入模式，在文末另起一行增添以下内容：

export http_proxy="http://127.0.0.1:7890"
export https_proxy="http://127.0.0.1:7890"
export no_proxy="http://localhost, http://127.0.0.1"
依次按下 Esc，:，w，q，!，Enter 以保存文件。

执行 sudo vim /etc/sudoers  编辑 /etc/sudoers 配置文件，
在 Defaults 语句下方另起一行增添以下内容：

Defaults env_keep+="http_proxy https_proxy no_proxy"
依次按下 Ctrl + x，y，Enter 以保存文件。

以上配置文件需手动重启以生效。

添加进程服务（开机自启项）
执行 sudo vim /systemd/system/clash.service 创建并编辑 clash.service 配置文件，
添加以下内容：

# This is about to control clash's start|stop|restart|status|enable
[Unit]
Description=Clash Service
After=network.target

[Service]
ExecStart=/bin/clash -d /home/pi/clash-bin
Restart=on-abort
LimitNOFILE=1048576

[Install]
WantedBy=multi-user.target

依次按下 Esc，:，w，q，!，Enter 以保存文件。

执行 sudo systemctl start clash && sudo systemctl enable clash 运行服务进程并建立自启项。

Clash 的使用
进入 Clash 面板 绑定本机 IP 地址以及端口（默认为 127.0.0.1:90），打开代理开关、选择节点之后，就可以通过代理服务器上网了。


开启网页ui
下载网页ui代码：
git clone https://github.com/Dreamacro/clash-dashboard.git

cd clash-dashboard

切换到已经制作好产出的分支
git checkout -b gh-pages origin/gh-pages
配置config.yaml：

Clash 的 RESTful API
external-controller: '0.0.0.0:8989'

RESTful API 的口令
secret: '网页访问密码'

您可以将静态网页资源（如 clash-dashboard）放置在一个目录中，clash 将会服务于 RESTful API/ui
参数应填写配置目录的相对路径或绝对路径。
external-ui: /home/pi/.config/clash/clash-dashboard

访问ip:8989/ui可见下面的网页：








