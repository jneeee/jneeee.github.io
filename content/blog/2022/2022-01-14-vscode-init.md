---
date: 2022-01-14
title: 我的 Vscode 配置
author: Jn
categories: 工作
tags: 
- Tools
---
记录使用vscode时的一些插件和配置文件。

F1打开全局搜索，输入setting，打开setting.json
```json
{
    "window.zoomLevel": 3,
    "editor.renderWhitespace": "all",
    // 特殊字体显示变量
    "editor.semanticHighlighting.enabled": true,
    // "workbench.colorTheme": "Dracula Soft",
    "workbench.editorAssociations": {
        "*.ipynb": "jupyter-notebook"
    },
    // 行号和右边进度条显示当前修改的内容，非常重要。需要从绝对路径打开代码
    // WSL 下的代码推荐放在 unix 存储下（/home/xxx/ 而不是 c://xxx /mnt/c/xx）否则影响性能
    "scm.diffDecorations": "all",

    "explorer.confirmDelete": false,
    "explorer.confirmDragAndDrop": false,

    "git.ignoreLegacyWarning": true,
    "gitlens.currentLine.pullRequests.enabled": false,
    "gitlens.codeLens.enabled": false,
    "gitlens.hovers.avatars": false,
    "gitlens.currentLine.enabled": false,
    "gitlens.advanced.messages": {
        "suppressGitVersionWarning": true,
        "suppressLineUncommittedWarning": true
    },
    "python.analysis.autoImportCompletions": false,
    // "python.linting.flake8Path": "/home/xxx/miniconda3/envs/37/bin/flake8",
    "python.linting.flake8Enabled": true,
    "python.linting.flake8Args": ["--ignore=I100,H404,H405,F811,H903"],
    //I100: line too long, H404 H405: docstring, F811:unused func,
    // H903: Windows style line endings
    // 自定义导包路径
    // "python.analysis.extraPaths": ["/path"],
    "python.analysis.diagnosticSeverityOverrides": {
        "reportMissingModuleSource": "none"
    },

    "terminal.integrated.defaultProfile.windows": "Command Prompt",
    // make Ctrl+e work fine
    "terminal.integrated.allowChords": false,
    "terminal.integrated.fontSize": 12,

    "leetcode.endpoint": "leetcode-cn",
    "leetcode.workspaceFolder": "/home/xxx/code2/leetcode",
    "leetcode.hint.configWebviewMarkdown": false,
    "leetcode.defaultLanguage": "python3",
    "leetcode.showDescription": "In File Comment",
    "leetcode.enableStatusBar": false,

    "leetcode.hint.commentDescription": false,
    "leetcode.hint.commandShortcut": false,
    "remote.SSH.remotePlatform": {
        "addr.com": "linux"
    },
    "workbench.colorCustomizations": {
        "terminal.background": "#181818",
        "terminal.foreground": "#D8D8D8",
        "terminalCursor.background": "#D8D8D8",
        "terminalCursor.foreground": "#D8D8D8",
        "terminal.ansiBlack": "#181818",
        "terminal.ansiBlue": "#7CAFC2",
        "terminal.ansiBrightBlack": "#585858",
        "terminal.ansiBrightBlue": "#7CAFC2",
        "terminal.ansiBrightCyan": "#86C1B9",
        "terminal.ansiBrightGreen": "#A1B56C",
        "terminal.ansiBrightMagenta": "#BA8BAF",
        "terminal.ansiBrightRed": "#AB4642",
        "terminal.ansiBrightWhite": "#F8F8F8",
        "terminal.ansiBrightYellow": "#F7CA88",
        "terminal.ansiCyan": "#86C1B9",
        "terminal.ansiGreen": "#A1B56C",
        "terminal.ansiMagenta": "#BA8BAF",
        "terminal.ansiRed": "#AB4642",
        "terminal.ansiWhite": "#D8D8D8",
        "terminal.ansiYellow": "#F7CA88"
    },

    // 右侧大纲显示的信息
    "outline.showPackages": false,
    "outline.showVariables": false,
    "security.workspace.trust.untrustedFiles": "open",
    // terminal 光标样式
    "terminal.integrated.cursorStyle": "line",
    "terminal.integrated.cursorBlinking": true,
    "terminal.integrated.enableMultiLinePasteWarning": false,
    // 选中复制和右键粘贴
    "terminal.integrated.copyOnSelection": true,
    "python.languageServer": "Pylance",
    "python.testing.unittestEnabled": true,
    // Diff folders 对比工具，忽略pyc
    "l13Diff.exclude":[
        "**/.DS_Store",
        "**/.git",
        "**/.hg",
        "**/.svn",
        "**/CVS",
        "**/node_modules",
        ".stestr",
        "**/__pycache__",
        ".tox",
        "*egg-info",
    ]
}

```

其中python和flake8的其他配置可以在 https://code.visualstudio.com/docs/python/linting#_flake8 找到。flake8的静态代码检查在保存文件时会自动触发，执行日志在OUTPUT里，结果在PROBLEMS里。
