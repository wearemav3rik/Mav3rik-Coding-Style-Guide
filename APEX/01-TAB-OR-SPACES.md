# Tabs or Spaces
## 4 Spaces
We will be using `4 spaces` to maximise readability of our code. Ideally, we want to configure our tab keybinding to type 4 spaces.

<br>

## Max Spaces per Line
For each line of our code the maximum is `120 spaces`.

This will prevent too much horizontal scrolling when reading through each other's code.

<br>

## Editor Config
We can manually override the `.editorconfig` file for VSCode and IntelliJ.
To do so, create the following file in your project's root directory. As of writing, IlluminatedCloud is preventing this override.
```
[{*.apex,*.cls,*.soql,*.trigger}]
charset = utf-8
end_of_line = lf
indent_size = 4
indent_style = space
insert_final_newline = false
max_line_length = 120
tab_width = 4
```

<br>

## Hotkeys
### VS Code + Apex PMD Plugin

For VS Code users, the hotkey is `Ctrl + Shift + F` while on the Apex class file.

Another method is to `Cmd + Shift + P` and choose `Format Document`.

<br>

### IntelliJ + IlluminatedCloud

For IntelliJ, the hotkey is `Cmd + Option + L` while on the Apex class file.

Another method is to go to `Menu Bar > Code > Reformat Code`.