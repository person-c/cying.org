---
title: Environment Setting
date: '2022-08-17'
slug: Environment Setting
---

每次配置环境总是Google一大堆，为了方便以后使用，将用到的的环境配置都记录在这里。首先怎么定义环境，按我现在浅薄的认知就是一些参数配置，一个预先定义的值 key = value, 应用或环境在启动时从这里获取变量值，指定应用在执行时其需要的值。环境变量包括全局环境变量以及局部环境变量。全局环境变量，所有都可以获得的变量。 局部环境变量，某些可以获得这些值。在写代码时将一些敏感的变量值，独立出来作为预先定义的环境变量文件.env，并将其放入.gitignore，然后将代码上传到gitHub，以防止一些隐私信息泄露。另外初次接触bash脚本的人总是对各种命令感觉奇怪，其实这些命令只是函数的特殊形式，其后面可能会接一些其它东西，这些东西就是函数的参数。



## Windows 下环境配置

#### R 环境配置
搜索环境变量 -> 将R路径写入环境变量

#### Vscode配置
Vscode 提供了json文件（一种配置文件），可以直接修改该文件进行设置

1. 快捷键设置
左下脚设置 -> Keyboard Shortcuts -> 右上脚打开Json配置文件
```json
// begin of R language shortcuts
[{
    "key": "ctrl+shift+m",
    "command": "editor.action.insertSnippet",
    "when": "editorTextFocus && editorLangId == 'r'",
    "args": {
        "snippet": " %>% "
    }
},

{
    "key": "ctrl+enter",
    //"command": [ "python.execSelectionInTerminal", "cursorDown" ],
    "command": "macros.pythonExecSelectionAndCursorDown",
    "when": "editorTextFocus && editorLangId == 'python'"
}
]
```

2. 其它配置
R：
- install.package("languageserver"): 代码补全，自动提示 
- install.package("httpgd"): vscode中实时查看R绘图结果，数据。
- vscode 插件 R
- vscode 插件 R markdown all in one
- vscode 插件 Remote -SSH 


左下脚设置 -> Setting -> 右上脚打开Json配置文件
```Json
{
    "workbench.editor.untitled.hint": "hidden",
    "security.workspace.trust.untrustedFiles": "open",
    "editor.bracketPairColorization.enabled": false,
    "workbench.colorTheme": "Default Dark+",
    
    "r.rterm.windows": "D:/R/R latest/R-4.1.3/bin/R.exe",
    "r.rpath.windows": "D:/R/R latest/R-4.1.3/bin/R.exe",
    "r.rterm.option":  "D:/R/R latest/R-4.1.3/bin/R.exe",

    "r.rterm.linux": "/its1/GB_BT1/zhuangzhenhua/soft/R-4.1.3/bin/R",
    "r.rpath.linux": "/its1/GB_BT1/zhuangzhenhua/soft/R-4.1.3/bin/R",
    "remote.SSH.remotePlatform": {
        "218.6.173.101": "linux"
    },
    "git.ignoreLegacyWarning": true,
    "terminal.integrated.autoReplies": {
    
    },
    "r.plot.useHttpgd": true,
    "r.sessionWatcher": true,
    "git.enableSmartCommit": true,
    "http.proxy": "http://127.0.0.1:10808",
    "window.zoomLevel": 1
    
}
```

## Linux 下环境配置
#### R环境配置
终端中输入 `vim /home/user/.bashrc`，打开配置文件输入下面的命令。
```bash
# User specific aliases and functions
alias Rscript="/its1/GB_BT1/zhuangzhenhua/soft/R-4.1.3/bin/Rscript"
alias R="/its1/GB_BT1/zhuangzhenhua/soft/R-4.1.3/bin/R"
```

终端中输入 `vim /home/user/.Rprofile`,打开R配置文件输入下面R代码，该R代码在每次R启动时，都会执行。
```r
.libPaths(c("/its1/GB_BT1/cuiwei/R_Library"))
library("languageserver")
```

## Git 配置
1 git bash下配置git与github账号的连接。
```bash
git config --global user.name "myname"
git config --global user.email  "test@gmail.com"
```
2 为了实现自己账号下本地与远程仓库的同步，先在github建立一个空的仓库，然后使用git克隆到本地即可。





参考：  
[linux下怎么运行R脚本](http://www.cureffi.org/2014/01/15/running-r-batch-mode-linux/)  
[环境变量介绍以及使用](https://medium.com/chingu/an-introduction-to-environment-variables-and-how-to-use-them-f602f66d15fa)     
[Linux下怎么设置环境变量](https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-set-environment-variables-in-linux/)

