# 必备配置

## 整体风格

Ctrl+Shirt+P =>Preferences:Open User Settings(JSON)

```
{
"workbench.colorTheme": "Dark+",
	"files.exclude": {
		"**/*.meta": true
	},
	"workbench.secondarySideBar.defaultVisibility": "visible",
	"security.workspace.trust.untrustedFiles": "open",
	"workbench.editorAssociations": {
		"*.md": "vscode.markdown.preview.editor"
	},
	"editor.formatOnSave": true
}
```

## 快捷键

### 修复代码

Ctrl+K    Ctrl+S 打开快捷设置界面，右上角打开json配置(keybindings.json)

```
// 将键绑定放在此文件中以覆盖默认值
[
    {
        "key": "alt+enter",
        "command": "editor.action.quickFix",
        "when": "editorHasCodeActionsProvider && !editorReadonly"
    }
]
```

# 插件

## 一键切换VSCode多个设置

1.安装插件 Commands (Author : Alexander) 和 Toggle (Author : Peng Lv)

添加字段到------Ctrl+Shirt+P =>Preferences:Open User Settings(JSON)

### 显示每行的git信息

```
{
    "git.blame.editorDecoration.disableHover": false,
    "git.blame.editorDecoration.enabled": false,
}
```

### 切换代码风格C#

```
{
    "typescript.format.placeOpenBraceOnNewLineForFunctions": false,
    "typescript.format.placeOpenBraceOnNewLineForControlBlocks": false,
    "javascript.format.placeOpenBraceOnNewLineForFunctions": false,
    "javascript.format.placeOpenBraceOnNewLineForControlBlocks": false
}
```

完成配置实例

```
{
  "commands.commands": {
    "切换大括号换行": {
      "command": "toggle",
      "args": {
        "id": "braceNewLine",
        "value": [
          {
            "typescript.format.placeOpenBraceOnNewLineForFunctions": true,
            "typescript.format.placeOpenBraceOnNewLineForControlBlocks": true,
            "javascript.format.placeOpenBraceOnNewLineForFunctions": true,
            "javascript.format.placeOpenBraceOnNewLineForControlBlocks": true
          },
          {
            "typescript.format.placeOpenBraceOnNewLineForFunctions": false,
            "typescript.format.placeOpenBraceOnNewLineForControlBlocks": false,
            "javascript.format.placeOpenBraceOnNewLineForFunctions": false,
            "javascript.format.placeOpenBraceOnNewLineForControlBlocks": false
          }
        ]
      },
      "statusBar": {
        "alignment": "right",
        "text": "$(symbol-method) 大括号换行",
        "tooltip": "切换 TypeScript / JavaScript 大括号换行",
        "priority": 100
      }
    }
  }
}
```

