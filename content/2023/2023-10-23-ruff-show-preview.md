---
date: 2023-10-23
title: Python lint - ruff 的设置问题
author: Jn
categories: 工作
tags:
  - python
---
ruff 实现了 flake8 的 lint 规则，并且得益于 Rust 语言，速度比 flake8 等快多了。
记录几个设置问题

### 1 不显示 E231
ruff 明明配了 select=["E"] 却不检测：
```
Missing whitespace after ','
```

需要配置 `preview = true`，因为它实现的 E 规则现在大多是预览版
```
# cat ~/.config/ruff/ruff.toml
# Enable flake8-bugbear (`B`) rules.
# https://beta.ruff.rs/docs/rules/
# select = ["E", "F", "B", "UP", "SIM", "BLE", "PL", "ISC", "D300", "PIE", "PERF", "RET", "ARG",]
select = ["ALL"]

preview = true

# D300 PEP 257 recommends the use of """triple double quotes""" for docstrings, to ensure consistency.
# BLE001 Checks for except clauses that catch all exceptions.
# Never enforce `E501` (line length violations).
ignore = ["E501", "Q", "ANN", "D", "ERA", "PLR"]

extend-exclude = [
    "envs",
]
cache-dir = "/code/.cache/ruff"
```
select 后面直接写明 `E231` 也行。

### 2 不检查其他路径的代码
使用 vscode ruff 插件时候，默认会检查所有打开过的文件的lint ，如果只想检查工作区的代码，在vscode 设置里加上：

```
# setting.json
    "ruff.lint.args": [
        "${workspaceFolder}",
        // "--exclude=envs",
    ],
```

