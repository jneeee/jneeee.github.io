---
published: true
title: Vscode init
layout: post
author: Jn
category: work
tags: 
- vscode
---
记录使用vscode时的一些插件和配置文件。

F1打开全局搜索，输入setting，打开setting.json
```json
{
    "workbench.editorAssociations": {
        "*.ipynb": "jupyter.notebook.ipynb"
    },
    "window.zoomLevel": 2,
    "explorer.confirmDelete": false,
    "explorer.confirmDragAndDrop": false,
    "gitlens.currentLine.pullRequests.enabled": false,
    "gitlens.codeLens.enabled": false,
    "gitlens.hovers.avatars": false,
    "editor.renderWhitespace": "all",
    "python.linting.flake8Enabled": true,
    "python.linting.flake8Path": "PATHTO python/python310/Scripts/flake8.exe",
    "python.linting.flake8Args": [],
    "terminal.integrated.defaultProfile.windows": "Command Prompt",
    "python.languageServer": "None"
}
```

其中python和flake8的其他配置可以在 https://code.visualstudio.com/docs/python/linting#_flake8 找到。flake8的静态代码检查在保存文件时会自动触发，执行日志在OUTPUT里，结果在PROBLEMS里。
