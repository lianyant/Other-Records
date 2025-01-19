在Git中，如果你需要修改已经添加的远程仓库的URL，可以使用命令`git remote set-url`来进行更新。这里是具体的步骤：

1. **查看现有的远程仓库**:
   
   ```bash
   git remote -v
   ```
   
   这个命令会显示所有配置的远程仓库及其对应的URL。

2. **修改远程仓库的URL**:
   
   ```bash
   git remote set-url [remote-name] [new-url]
   ```
   
   其中 `[remote-name]` 是远程仓库的名称（通常默认为`origin`），`[new-url]` 是新的仓库URL。
   
   例如，如果你想要将名为`origin`的远程仓库的URL改为新的URL，你可以这样做：
   
   ```bash
   git remote set-url origin https://github.com/user/repo.git
   ```

3. **再次检查更新后的远程仓库URL**:
   
   ```bash
   git remote -v
   ```
   
   这将显示更新后的远程仓库的URL，确认修改已正确应用。

这样就可以重新设置你的远程仓库的对应内容了。如果有其他需要帮助的地方，随时告诉我！
