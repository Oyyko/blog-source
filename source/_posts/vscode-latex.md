---
title: VsCode Latex 插件配置
date: 2022-02-10
tag: 
- VSCODE
- LATEX
category: VSCODE
mathjax: false
---
更改vscode的 setting.json文件
加入
```json
"latex-workshop.latex.tools": [
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "-outdir=%OUTDIR%",
                "%DOC%",
                "-xelatex"
            ],
            "env": {}
        },
```
即可

即在命令参数的最后增加 `-xelatex` 参数

备注：之前在文件头部添加`% !TEX program = xelatex`是管用的，现在不知道为啥又不行了。