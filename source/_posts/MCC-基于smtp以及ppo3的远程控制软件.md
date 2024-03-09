---
title: MCC_基于smtp以及ppo3的远程控制软件
date: 2023-12-01 15:06:31
categories:
    - nodejs
tags:
    - pop3
    - smtp
    - electron
---
# Mcc
基于POP3以及smtp协议的远程控制软件，其前端有electron构建。
你可以在任何地方远程操控你的电脑
## 界面展示
![sgyrpn.png](https://files.catbox.moe/sgyrpn.png)
![n1ov8b.png](https://files.catbox.moe/n1ov8b.png)

你可以可以使用任何可以发送邮件的设备，使用编辑邮件标题的形式远程操纵电脑。
软件检测到你的邮件将会执行相应的指令，包括运行shell、python脚本、或者nodejs脚本，这将给你的操控带来无限的可能  
比如你可以编辑一封标题为shutdown的邮件来使您的电脑远程关机，或者编辑一封标题为train_model的邮件来式你的电脑训练深度学习模型。

具体移步[github连接](https://github.com/showarp/Mcc)