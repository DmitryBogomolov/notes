# ESLint in VSCode

1. `ctrl+shift+p` - "Install Extensions" - install and enable "Eslint"

2. reboot VSCode

3. `ctrl+shift+p` - "Open User Settings" - update
```json
{
    "files.trimTrailingWhitespace": true,
    "javascript.validate.enable": true
}
```

4. `ctrl+shift+p` - "Open Keyboard Shortcuts" - update
```json
[
    {
    "key": "shift+alt+f",
    "command": "eslint.executeAutofix",
    "when": "editorTextFocus && !editorReadonly"
    }
]
```
