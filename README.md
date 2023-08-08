# VSCode LaTeX configuration

愉快地在VScode中编写TeX文档

## Table of Contents

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=2 orderedList=false} -->

<!-- code_chunk_output -->

- [Background](#background)
- [Installation](#installation)
- [Usage](#usage)
- [Related Efforts](#related-efforts)
- [Maintainer](#maintainer)
- [Contributing](#contributing)
- [LICENSE](#license)

<!-- /code_chunk_output -->

## Background

本文档是各种LaTeX插件/工具的汇总，记录VScode配置以达到愉快地编写LaTeX文档，实现以下功能：

- LaTeX语法补全
- tex文件格式化
- 保存时编译
- 快速输入公式
- 公式和图片的实时预览
- 其他
  - 英文单词联想

## Installation

### Clone this repository

克隆本项目：

```Shell
cd \path\to\repo
git clone https://github.com/hilinxinhui/VScode_LaTeX_configuration.git
```

### Install Tex Live

安装Tex Live请看这个[文档](https://mirrors.tuna.tsinghua.edu.cn/CTAN/info/install-latex-guide-zh-cn/install-latex-guide-zh-cn.pdf)。

检查tex是否已经成功安装：

```Shell
tex -v
```

### Install VSCode plugins

在VSCode插件商店中搜索下载即可。

插件列表：

- LaTeX Workshop：LaTeX编译、预览和语法检查等
- English Word Hint：单词拼写检查
- Path Auto Complete：路径自动补全
- indent rainbow：为不同缩进层级的代码块添加颜色
- 根据需要自行增加

## Usage

在`settings.json`文件中修改，按下`ctrl` + `shift` + `p`（或单击VSCode界面左下角齿轮图标，点击Command Palette/命令面板）输入settings选择`Preferences: Open User Settings (JSON)`即可打开配置文件。

### 基本配置

基本配置主要包括：

- 开启IntelliSense以自动补全
- 设置编译后文件的输出目录
- 设置错误信息和警告信息的弹窗显示
- 设置PDF预览样式等

```JSON5
// ========================================
    // latex-workshop其他配置
    // ========================================

    // 鼠标悬停，预览公式时，支持 boldsymbol 宏
    "latex-workshop.hover.preview.mathjax.extensions": [
        "boldsymbol"
    ],
    // 是否启用 IntelliSense，自动补全引用的包中的环境和命令
    "latex-workshop.intellisense.package.enabled": true,
    // 编译后的文件输出目录
    "latex-workshop.latex.outDir": "./tmp",
    // 默认编译引擎为上次使用的
    "latex-workshop.latex.recipe.default": "lastUsed",
    // 预览复杂公式，使用时需要通过 command palette (命令面板) 打开
    "latex-workshop.mathpreviewpanel.cursor.enabled": true,
    // 不允许弹窗显示错误信息
    "latex-workshop.message.error.show": false,
    // 不允许弹窗显示警告信息
    "latex-workshop.message.warning.show": false,
    // 预览 PDF 时，反转颜色
    "latex-workshop.view.pdf.invert": 1,
    // 预览 PDF 时，自动检测是否需要反转颜色
    "latex-workshop.view.pdf.invertMode.enabled": "auto",
```

### 编译工具链配置

编译工具通常为pdfLaTeX（英文文档）、XeLaTeX（中文文档）和bibTeX（参考文献）。使用latexmk编译可以避免使用上述工具处理交叉引用、参考文献和各种索引表时需要的多次编译过程，latexmk支持多种编译引擎（包括XeLaTeX和pdfLaTeX），可以根据文档内容自动编译。

```JSON5
// ========================================
    // 编译工具链配置，使用latexmk（注释中是不使用latexmk的方法）
    // 配置latex-workshop.latex.recipes和latex-workshop.latex.tools
    // ========================================

    "latex-workshop.latex.recipes": [
        // 第一个recipe是默认的编译工具
        {
            "name": "xelatex",
            "tools": [
                "xelatexmk"
            ]
        },
        {
            "name": "pdflatex",
            "tools": [
                "pdflatexmk"
            ]
        },
    ],

    "latex-workshop.latex.tools": [
        // 编译工具和命令
        {
            "name": "xelatexmk",
            "command": "latexmk",
            // 或"command": "xelatex",
            "env": {},
            "args": [
                "-synctex=1",
                "-pdfxe",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-outdir=%OUTDIR%",
                // "%DOC%"
                "DOCFILE" // 将%DOC%替换为%DOCFILE%即可支持编译中文路径下的文件
            ]
        },
        {
            "name": "pdflatexmk",
            "command": "latexmk",
            // 或"command": "pdflatex",
            "env": {},
            "args": [
                "-synctex=1",
                "-pdf",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-outdir=%OUTDIR%",
                // "%DOC%"
                "DOCFILE" // 将%DOC%替换为%DOCFILE%即可支持编译中文路径下的文件
            ]
        },
    ],
```

### 代码格式化配置

使用latexindent格式化LaTeX代码。需要事先安装latexindent，详细的安装和使用方法参考[官方文档](https://latexindentpl.readthedocs.io/en/latest/sec-how-to-use.html)，一个简短的在Ubuntu安装latexindent的记录可以参考[我的博客](https://hilinxinhui.github.io/2023/07/24/Install-latexindent-pl/)。

```JSON5
// ========================================
    // 使用latexindent格式化LaTeX代码
    // 将latexindent的输出信息保存为log（保存到输出目录下）便于管理
    // ========================================
    
    "latex-workshop.latexindent.args": [
        "-g",
        "./%OUTDIR%/indent.log",
        "%TMPFILE%",
        "-y=defaultIndent: '%INDENT%'"
    ],
```

### 配置文件

完整的配置示例文件在[这里](./latex_workshop_settings.json)，将此文件中的内容粘贴到上述VSCode的配置文件`settings.json`中即可。

### 其他

未完待续...

预期的功能包括：

- 快速输入公式（通过HyperSnips for Math插件实现）
- TiKZ Externalize 加速编译

## Related Efforts

- [latex-vscode-config](https://github.com/shinyypig/latex-vscode-config)
- [使用VSCode编写LaTeX](https://zhuanlan.zhihu.com/p/38178015)

## Maintainer

[@hilinxinhui](https://github.com/hilinxinhui)

## Contributing

欢迎Star、Fork，欢迎提交Issues和PRs！

### Contributors

<!-- readme: collaborators,contributors -start -->
<!-- readme: collaborators,contributors -end -->

## LICENSE

[MIT](./LICENSE)
