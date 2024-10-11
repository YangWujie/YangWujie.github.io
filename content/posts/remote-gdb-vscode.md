+++
title = '远程调试'
date = 2024-10-11T21:36:01+08:00
draft = false
+++

## 在 VSCode 里用 gdb 进行远程调试

1. 在 `VSCode` 里安装 [Native Debug](https://marketplace.visualstudio.com/items?itemName=webfreak.debug) 插件
2. 在 `launch.json` 里添加配置 `Attach to gdbserver`
3. 对配置做些调整：
```
        {
            "type": "gdb",
            "request": "attach",
            "name": "Attach to gdbserver",
            "executable": "${command:cmake.launchTargetPath}",
            "target": "localhost:2345",
            "remote": true,
            "cwd": "${workspaceRoot}",
            "valuesFormatting": "parseText",
            "autorun": [
            ]    
        },
```