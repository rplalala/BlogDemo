---
title: VSCODE 配置GitBash终端，并始终以管理员身份运行
date: 2022-01-03 15:43:02
tags: [vscode,hexo]
categories: vscode
---

# settings.json 配置
```json
    "terminal.integrated.profiles.windows": {   //配置终端选项
        "PowerShell": {
          "source": "PowerShell",
          "icon": "terminal-powershell"
        },
        "Command Prompt": {
          "path": [
            "${env:windir}\\Sysnative\\cmd.exe",
            "${env:windir}\\System32\\cmd.exe"
          ],
          "args": [],
          "icon": "terminal-cmd"
        },
        "GitBash": {
            "path": [
                "D:\\Program Files\\Git\\bin\\bash.exe" //gitbash路径
            ],
            "icon": "terminal-bash"
        }
    },
    "terminal.integrated.defaultProfile.windows": "GitBash",    //设置默认终端
```

然后就可以愉快的写GitBash啦^^
![vscode配置gitbash终端.png](https://s2.loli.net/2022/01/03/dDhSwFMn7QqfgRu.png)

# 以管理员身份运行
我们知道，如果不以管理员身份运行GitBash，再执行一些命令时可能会因为权限不够而报错
1. 将 "D:\\Program Files\\Git\\bin\\bash.exe" 设为始终以管理员方式运行
2. 将 VSCODE执行文件 Code.exe 设为始终以管理员方式运行
   
设置方法： **右键属性 -> 兼容性 -> 始终以管理员方式运行 勾上**