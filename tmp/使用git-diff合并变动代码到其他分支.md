

**疑问：**我在一个分支上开发了代码，我想知道这个分支上的某个目录在之前的某个提交到现在最新的提交 这个过程之间，有哪些改动，如何做？



**回答：**

在 Git 中，要查看某个分支上特定目录从过去某个提交到最新提交之间的所有改动，您可以使用 `git diff` 命令。这是一个非常强大的命令，可以清晰地展示两个不同提交点（commits）之间的差异。

以下是具体的操作步骤和命令解释。

## 核心命令

您需要使用的核心命令格式如下：

Bash

```
git diff <start_commit> <end_commit> -- <directory_path>
```

**命令解析:**

- **`<start_commit>`**: 比较的起始点。这通常是较旧的那个提交的哈希值（Commit Hash）。

- **`<end_commit>`**: 比较的结束点。在您的情况下，这是该分支的最新提交。您可以使用分支名（例如 `main` 或 `feature/my-branch`）或 `HEAD` 来代表当前分支的最新提交。

- **`--`**: 这是一个分隔符，用来明确地告诉 Git 后面跟的是文件或目录路径，而不是分支名或提交哈希。虽然在没有歧义的情况下可以省略，但加上它是一个好习惯。

- **`<directory_path>`**: 您想要查看改动的目录的相对路径。

---

## 操作步骤

### 1. 找到起始提交的哈希值 (Commit Hash)

首先，您需要确定作为改动起点的那个旧提交的哈希值。您可以使用 `git log` 命令来查看提交历史。

为了方便浏览，推荐使用 `--oneline` 选项，它会将每个提交信息压缩到一行显示。

Bash

```
# 查看当前分支的提交历史
git log --oneline

# 您会看到类似下面的输出
# 1a2b3c4 (HEAD -> feature/my-branch) feat: Add new feature X
# d5e6f7g fix: Correct a bug in module Y
# 8h9i0j1 refactor: Improve code structure  <- 比如您想从这里开始看
# ...
```

从列表中找到您想作为起点的那个提交，并复制它的哈希值（例如 `8h9i0j1`）。

### 2. 执行 `git diff` 命令查看改动

现在您有了起始提交的哈希值，可以执行 `git diff` 命令了。假设您想查看的目录是 `src/components`，起始提交是 `8h9i0j1`，并且您当前就在这个开发分支上。

Bash

```
git diff 8h9i0j1 HEAD -- src/components
```

- `8h9i0j1` 是您从 `git log` 中找到的起始提交哈希。

- `HEAD` 代表当前分支的最新提交。您也可以用您的分支名替换它，效果相同。

- `src/components` 是您关心的目录。

执行该命令后，终端会输出从 `8h9i0j1` 提交之后到 `HEAD` 提交为止，所有在 `src/components` 目录下的文件改动详情。

**输出结果解读:**

- 以 `-` 开头的行表示从文件中删除的内容。

- 以 `+` 开头的行表示向文件中添加的内容。

- `diff --git a/path/to/file b/path/to/file` 表示正在比较哪个文件。

## 更多有用的 `git diff` 选项

根据您的需求，还可以添加一些选项来改变输出的格式：

- 只看文件名，不看具体内容:
  
  如果您只想知道哪些文件被改动了，而不关心具体的代码变化，可以使用 --name-only 选项。
  
  Bash
  
  ```
  git diff 8h9i0j1 HEAD --name-only -- src/components
  ```

- 查看简略的统计信息:
  
  如果您想看一个改动的摘要（哪些文件被修改，以及增删了多少行），可以使用 --stat 选项。
  
  Bash
  
  ```
  git diff 8h9i0j1 HEAD --stat -- src/components
  
  # 输出可能如下所示:
  #  src/components/Button.js | 10 +++++-----
  #  src/components/Card.vue  |  5 +++++
  #  2 files changed, 10 insertions(+), 5 deletions(-)
  ```

通过以上步骤，您就可以精确地追踪到在特定时间范围内，某个目录下的所有代码演变历史了。
