# Tabs or Spaces
Everything you need to know about how we format our code and the tools we use to ensure Code Readability.

## 1 Tab
We will be using `1 Tab` to maximise readability of our Apex code. Readability-wise 1 tab looks a lot like 4 spaces, but it will save us some character space and prevent us from reaching the `100,000 character limit` for Apex classes. Please note that this is strictly for Apex classes.

<br>

## Max Spaces per Line
For each line of code, the maximum we want to set is `120 characters`. but as we are using tabs, tabs will look like 4 spaces and we therefore need to adjust that perspective such that we seemingly only reach 120 characters per line. The benefit of this is that it will help us prevent too much horizontal scrolling when reading through other people's code.
<br><br>
There are plugins that help us enforce this through providing visible vertical lines in which we will stop typing, if not already a feature of our IDE.

<br>

## Editor Config
We can manually override the `.editorconfig` file for VSCode and IntelliJ.
To do so, create the following file in your project's root directory. As of writing, IlluminatedCloud is preventing this override.
```
[{*.apex,*.cls,*.soql,*.trigger}]
charset = utf-8
end_of_line = lf
indent_size = 4
indent_style = tab
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