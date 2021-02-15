# 1. 从kaggle获取数据

kaggle网址：https://www.kaggle.com

进入网址后，使用邮箱进行注册。

kaggle-api的github地址：https://github.com/Kaggle/kaggle-api

参考github中的指引，pip install kaggle

【可能出现的问题】OSError: Could not find kaggle.json. Make sure it‘s located in /home/user/.kaggle.

【解决方案】网页kaggle --> My Account --> API --> Create New API Token，下载得到kaggle.json文件，放在/home/usr/.kaggle隐藏文件夹下

进入kaggle网页[giveMeSomeCredit](https://www.kaggle.com/c/GiveMeSomeCredit)/data的页面，复制命令kaggle competitions download -c GiveMeSomeCredit到命令行中下载，完成下载数据库。



