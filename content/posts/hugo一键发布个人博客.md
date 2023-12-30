+++
title = 'hugo一键发布个人博客'
date = 2023-12-21T22:20:19+08:00
draft = false
author = "Deequoique"
categories = ["码农日记"]
tags = []
+++
 以vscode为例，在项目根文件夹建立.vscode文件夹，文件夹内创建`tasks.json`文件，内容如下

``` json
{
    "label": "Publish Blog",
    "type": "shell",
    "command": "hugo --theme=你的主题 --baseURL=\"你的站点\" && cd public && git add . && git commit -m \"Commit message\" && git push && cd ..",
    "group": {
        "kind": "build",
        "isDefault": true
    },
    "presentation": {
        "reveal": "always",
        "panel": "shared",
        "showReuseMessage": false,
        "clear": false
    },
    "problemMatcher": []
}
```
### 配置解释

- **label**: 任务名称。
- **type**: 任务类型，这里是 "shell"，意味着它将在 shell 环境下执行。
- **command**: 要执行的命令序列。
    - `hugo --theme=Loveit --baseURL="https://onlane.github.io/"`：运行 Hugo 命令来生成站点，使用 Loveit 主题，设置基础 URL 为你的远端仓库地址。
    - `cd public`：改变目录到 `public`。
    - `git add . && git commit -m "Commit message" && git push`：将更改添加到 Git 暂存区，提交这些更改，并推送到远程仓库。
    - `cd ..`：返回到先前的目录。
- **group**: 将此任务分组到 "build" 类别。
- **presentation**: 控制任务运行时的终端展示方式。
    - **reveal**: 设置为 "always"，在任务运行时总是显示终端。
    - **panel**: 使用共享的面板。
- **problemMatcher**: 用于匹配输出中的问题，这里设置为空。

Shift+ctrl+P运行命令，选择该任务即可。