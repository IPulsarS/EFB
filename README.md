# 部署EFB实现QQ和微信的无后台推送

## 0.参考链接
* [ehForwarderBot](https://github.com/ehForwarderBot/ehForwarderBot)  
* [efb-telegram-master](https://github.com/ehForwarderBot/efb-telegram-master)  
* [efb-wechat-slave](https://github.com/ehForwarderBot/efb-wechat-slave)  
* [efb-qq-slave](https://github.com/milkice233/efb-qq-slave)  
* [efb-qq-plugin-go-cqhttp](https://github.com/XYenon/efb-qq-plugin-go-cqhttp)  
* [QQ EFB的部署与配置](https://www.notion.so/QQ-EFB-debian-1be7966a9de9459b9467bc08a92c3eff)  
* [旧手机搭建电报转发](https://note.youdao.com/ynoteshare1/index.html?id=3ed5aa24c3072ef6e0cc4c95fa56aba7&type=note#/)  
* [在Telegram上使用EFB同时推送QQ与微信消息](https://www.shawnleetttt.xyz/posts/f1bc687a/)  
* [Tg收发微信——EFB2](https://specialhua.top/20210402/cid=71.html)  
* [ehForwarderBot遇到的那些坑](https://blog.shzxm.com/2020/12/31/efb/#)  

## 1.申请Telegram Bot
在Telegram向@BotFather 发起会话，发送命令`/newbot`以创建Bot  
<img src="https://github.com/IPulsarS/EFB/blob/main/Picture/1.jpg" width="300px">  
如图所示分别提交bot的名称与用户名（用户名须以Bot为结尾）  
设置好后还须对bot进行权限设置  
发送`/setprivacy`，选择刚刚创建好的 Bot 用户名，然后选择 `Disable`.  
发送`/setjoingroups`，选择刚刚创建好的 Bot 用户名，然后选择 `Enable`.  
发送`/setcommands`，选择刚刚创建好的 Bot 用户名，然后发送如下内容：  
```
help - 显示命令列表.
link - 将聊天链接到群组.
unlink_all - 取消所有聊天与群组的链接.
info - 显示当前Telegram聊天的信息.
chat - 生成聊天对话框.
extra - 查看从端提供的附加功能.
update_info - 更新组名称和资料图片.
react - 向一条消息作出回应，或列出回应者列表.
rm - 从远端会话中删除消息.
```
记住上图中的`HTTP API`，这是后来要用到的`token`  
用同样的方法，创建另一个Bot，一个用于QQ，一个用于微信  

在Telegram向@BotFather 发起会话，发送命令`/start`获取`Userid`，这是后来要用到的`admins`  
<img src="https://github.com/IPulsarS/EFB/blob/main/Picture/2.jpg" width="300px">  

## 2.开始部署
使用ssh以root用户连接到VPS，这里用的是Xshell 7软件  
<img src="https://github.com/IPulsarS/EFB/blob/main/Picture/3.png" width="600px">  

#### 2.1安装依赖
##### **逐行输入之后回车，下同**  
```
apt full-upgrade -y
apt install python3 python3-pip python3-pil python3-setuptools python3-numpy python3-yaml python3-requests python3-dev ffmpeg libmagic-dev libwebp-dev vim -y
apt install libopus0 libmagic1 libcairo2 libcairo2-dev git nano screen docker.io libssl-dev build-essential -y
python3 -m pip install --upgrade pip
python3 -m pip install --upgrade Pillow
```
安装相关服务  
```
pip3 install git+https://github.com/blueset/ehforwarderbot.git
pip3 install git+https://github.com/ehForwarderBot/efb-telegram-master
pip3 install git+https://github.com/ehForwarderBot/efb-wechat-slave
pip3 install -U git+https://github.com/milkice233/efb-qq-slave
pip3 install "efb-telegram-master[tgs]"
```
配置微信的EFB文件  
```
mkdir -p ~/.ehforwarderbot/profiles/wx/
```
```
vim ~/.ehforwarderbot/profiles/wx/config.yaml
```
输入以下内容  
```
master_channel: blueset.telegram
slave_channels:
- blueset.wechat
```
配置微信的ETM文件  
```
mkdir -p ~/.ehforwarderbot/profiles/wx/blueset.telegram
```
```
vim ~/.ehforwarderbot/profiles/wx/blueset.telegram/config.yaml 
```
输入以下内容
```
token: "123456" #值为你在 @BotFather 处获得的token
admins:
- 123456 #值为你在 @get_id_bot 处获得的admins
```
配置微信的EWS文件（可选）（参考`https://github.com/ehForwarderBot/efb-wechat-slave`）  
```
mkdir -p ~/.ehforwarderbot/profiles/wx/blueset.wechat
```
```
vim ~/.ehforwarderbot/profiles/wx/blueset.wechat/config.yaml
```
运行微信转发  
```
ehforwarderbot --profile wx
```
扫码登录即可收发消息  

下面开始配置QQ转发  


配置QQ的EFB文件  
```
mkdir -p ~/.ehforwarderbot/profiles/qq/
```
```
vim ~/.ehforwarderbot/profiles/qq/config.yaml
```
输入以下内容  
```
 Client: GoCQHttp
 GoCQHttp:
     type: HTTP
     access_token:                    #自己随便编一个字母加数字
     api_root: http://127.0.0.1:5700/
     host: 127.0.0.1
     port: 8000              
```
配置QQ的ETM文件  
```
mkdir -p ~/.ehforwarderbot/profiles/qq/blueset.telegram
```
```
vim ~/.ehforwarderbot/profiles/qq/blueset.telegram/config.yaml
```
输入以下内容
```
token: "123456" #值为你在 @BotFather 处获得的token
admins:
- 123456 #值为你在 @get_id_bot 处获得的admins
```
下载  
`https://github.com/Mrs4s/go-cqhttp/releases/download/v1.0.0-beta8-fix2/go-cqhttp_linux_amd64.tar.gz`  
到你电脑的桌面  
为了方便，将解压好的文件夹命名为`go-cqhttp`  
最新的下载链接可以在`https://github.com/Mrs4s/go-cqhttp/releases`找到  

使用Winscp连接你的vps，使用`Ctrl` + `Alt` + `H`显示隐藏的文件  
将`go-cqhttp`文件夹上传到`/root/.ehforwarderbot/profiles/qq`
配置go-cqhttp  
```
cd /root/.ehforwarderbot/profiles/qq/go-cqhttp
sudo chmod +x go-cqhttp
./go-cqhttp
```
输入`0`，`Enter`，即选择HTTP  

这时`/root/.ehforwarderbot/profiles/qq/go-cqhttp`目录出现了文件`config.yml`  
<img src="https://github.com/IPulsarS/EFB/blob/main/Picture/4.png" width="600px">  
