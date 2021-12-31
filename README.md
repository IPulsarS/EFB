# 部署EFB实现QQ和微信的无后台推送

## 0.参考链接
https://github.com/ehForwarderBot/ehForwarderBot  
https://github.com/ehForwarderBot/efb-telegram-master  
https://github.com/ehForwarderBot/efb-wechat-slave  
https://github.com/milkice233/efb-qq-slave  
https://github.com/XYenon/efb-qq-plugin-go-cqhttp  
https://www.notion.so/QQ-EFB-debian-1be7966a9de9459b9467bc08a92c3eff  
https://note.youdao.com/ynoteshare1/index.html?id=3ed5aa24c3072ef6e0cc4c95fa56aba7&type=note#/  
https://www.shawnleetttt.xyz/posts/f1bc687a/  
https://specialhua.top/20210402/cid=71.html  
https://blog.shzxm.com/2020/12/31/efb/#  

## 1.申请Telegram Bot
在Telegram向@BotFather 发起会话，发送命令`/newbot`以创建Bot  
<img src="https://github.com/IPulsarS/EFB/blob/main/Picture/1.jpg" width="300px">  
如图所示分别提交bot的名称与用户名（用户名须以Bot为结尾）  
设置好后还须对bot进行权限设置  
发送`/setprivacy`，选择刚刚创建好的 Bot 用户名，然后选择 `Disable`.  
发送`/setjoingroups`，选择刚刚创建好的 Bot 用户名，然后选择 `Enable`.  
发送`/setcommands`，选择刚刚创建好的 Bot 用户名，然后发送如下内容：  
    
        link - 将聊天链接到群组.
        unlink_all - 取消所有聊天与群组的链接. 
        info - 显示当前Telegram聊天的信息. 
        chat - 生成聊天对话框.
        extra - 查看从端提供的附加功能. 
        update_info - 更新组名称和资料图片.  
        react - 向一条消息作出回应，或列出回应者列表.
        rm - 从远端会话中删除消息.
        help - 显示命令列表.
