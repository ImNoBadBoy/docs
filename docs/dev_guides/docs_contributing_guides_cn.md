# 文档贡献指南


PaddlePaddle 的文档存储于 [PaddlePaddle/docs](https://github.com/PaddlePaddle/docs) 中，之后通过技术手段转为 HTML文件后呈现至[官网文档](https://www.paddlepaddle.org.cn/documentation/docs/zh/guides/index_cn.html) 。官网文档和 `docs` 的对应关系如下：

| 官网 |  docs |
| -- | -- |
|[文档/安装说明](https://www.paddlepaddle.org.cn/documentation/docs/zh/install/index_cn.html) | [docs/docs/install](https://github.com/PaddlePaddle/docs/tree/develop/docs/install) |
| [文档/使用教程](https://www.paddlepaddle.org.cn/documentation/docs/zh/guides/index_cn.html) | [docs/docs/guides](https://github.com/PaddlePaddle/docs/tree/develop/docs/guides)  |
| [文档/应用实践](https://www.paddlepaddle.org.cn/documentation/docs/zh/tutorial/index_cn.html) | [docs/docs/tutorial](https://github.com/PaddlePaddle/docs/tree/develop/docs/tutorial) |
| [文档/API文档](https://www.paddlepaddle.org.cn/documentation/docs/zh/api/index_cn.html) | [docs/docs/api](https://github.com/PaddlePaddle/docs/tree/develop/docs/api) |
| [文档/常见问题与解答](https://www.paddlepaddle.org.cn/documentation/docs/zh/faq/index_cn.html) | [docs/docs/faq](https://github.com/PaddlePaddle/docs/tree/develop/docs/faq) |
| [文档/Release Note](https://www.paddlepaddle.org.cn/documentation/docs/zh/release_note_cn.html) | [docs/docs/release_note_cn.md](https://github.com/PaddlePaddle/docs/blob/develop/docs/release_note_cn.md) |

## 一、修改前的准备工作

### 1.1 Fork
先跳转到  [PaddlePaddle/docs](https://github.com/PaddlePaddle/docs) GitHub 首页，然后单击 Fork 按钮，生成自己仓库下的目录，比如你的 GitHub 用户名为 USERNAME，则生成： https://github.com/USERNAME/docs。
![图片](http://bos.bj.bce-internal.sdns.baidu.com/agroup-bos-bj/bj-adbd631fa37a3bb3a313e723b17fe634764eebfe)

### 1.2 Clone
将你目录下的远程仓库clone到本地。
```
➜ git clone https://github.com/USERNAME/docs
➜ cd docs
```

### 1.3 创建本地分支

docs 目前使用 [Git流分支模型](https://nvie.com/posts/a-successful-git-branching-model/)进行开发，测试，发行和维护。

所有的 feature 和 bug fix 的开发工作都应该在一个新的分支上完成，一般从 develop 分支上创建新分支。

使用 ``git checkout -b`` 创建并切换到新分支。

```
➜  git checkout -b my-cool-stuff
```
值得注意的是，在 ``checkout`` 之前，需要保持当前分支目录 clean，否则会把 untracked 的文件也带到新分支上，这可以通过 ``git status`` 查看。

### 1.4 下载 pre-commit 钩子工具（若有的话，可以跳过此步骤）

Paddle 开发人员使用 [pre-commit](https://pre-commit.com/) 工具来管理 Git 预提交钩子。 它可以帮助你格式化源代码（C++，Python），在提交（commit）前自动检查一些基本事宜（如每个文件只有一个 EOL，Git 中不要添加大文件等）。

pre-commit测试是 Travis-CI 中单元测试的一部分，不满足钩子的 PR 不能被提交到 Paddle，首先安装并在当前目录运行它：

```
➜  pip install pre-commit
➜  pre-commit install
```

Paddle 使用 clang-format 来调整 C/C++ 源代码格式，请确保 clang-format 版本在 3.8 以上。

**注**：通过``pip install pre-commit``和 ``conda install -c conda-forge pre-commit``安装的yapf稍有不同，Paddle 开发人员使用的是 ``pip install pre-commit``。

## 二、正式修改文档

根据官网文档和 `docs` 的对应关系，确定要修改/新增的文档路径，然后修改或者新增。

### 2.1 新增文档

当你要新增文档时，需要参考上述的对应关系，找到合适的目录，新建 Markdown或reStructuredText文件。中英文文档存储在同一路径下，其中，中文文档的后缀为 `_cn.md/rst`，英文文档的后缀为 `_en.md/rst`。

在新增文件后，还需要在目录文件中添加该文件的索引。目录文件一般是 index_cn.rst/ index_en.rst，需要在文件的 `.. toctree::` 部分添加该文件的索引。

如在 文档 -> 使用教程 -> 动态图转静态图 中新增 《调试方法》，首先需要在 [docs/guides/04_dygraph_to_static](https://github.com/PaddlePaddle/docs/tree/develop/docs/guides/04_dygraph_to_static) 中 新建 `debugging_cn.md`，`debugging_en.md` 文件。之后，在  [docs/guides/04_dygraph_to_static/index_cn.rst](https://github.com/PaddlePaddle/docs/tree/develop/docs/guides/04_dygraph_to_static/index_cn.rst)  的 `toctree` 部分，新增 `debugging_cn.md` 的索引，合入后即可展示到官网。
```
..  toctree::
    :hidden:

    basic_usage_cn.rst
    program_translator_cn.rst
    grammar_list_cn.rst
    input_spec_cn.rst
    error_handling_cn.md
    debugging_cn.md   # 新增索引
```



### 2.2 修改文档

修改文档，可以通过文档的URL，确定文档的源文件。 如 文档 -> 使用教程 -> 动态图转静态图 中 《调试方法》的文档URL为：[https://www.paddlepaddle.org.cn/documentation/docs/zh/guides/04_dygraph_to_static/debugging_cn.html](https://www.paddlepaddle.org.cn/documentation/docs/zh/guides/04_dygraph_to_static/debugging_cn.html)，URL路径中，`guides/04_dygraph_to_static/debugging_cn.html` 即对应 `(docs/docs/)guides/04_dygraph_to_static/debugging_cn.md` , 因此，可以很快的确定文档的源文件，然后直接修改即可。


## 三、提交&push


### 3.1 提交&触发CI单测

- 修改 ``guides/04_dygraph_to_static/debugging_cn.md`` 这个文件，并提交这个文件

```
➜  git status
On branch my-cool-stuff
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git restore <file>..." to discard changes in working directory)
modified:   paddle/tensor/math/all_cn.rst

no changes added to commit (use "git add" and/or "git commit -a")

➜  git add  guides/04_dygraph_to_static/debugging_cn.md
```

  **如果你不想提交本次修改**，使用 ``git checkout -- <file>`` 取消上面对``guides/04_dygraph_to_static/debugging_cn.md``文件的提交，可以将它恢复至上一次提交的状态:
   
```
➜  git checkout  -- guides/04_dygraph_to_static/debugging_cn.md
```
   恢复后重新进行修改并提交文件即可。
   
- pre-commit：提交修改说明前，需要对本次修改做一些格式化检查：
   
```
➜  pre-commit
CRLF end-lines remover...............................(no files to check)Skipped
yapf.....................................................................Passed
Check for added large files..............................................Passed
Check for merge conflicts................................................Passed
Check for broken symlinks................................................Passed
Detect Private Key...................................(no files to check)Skipped
Fix End of Files.........................................................Passed
clang-format.........................................(no files to check)Skipped
cpplint..............................................(no files to check)Skipped
pylint...................................................................Passed
copyright_checker........................................................Passed
```
  全部Passed 或 Skipped后，即可进入下一步。如果有 Failed 文件，则需要按照规范，修改出现Failed 的文件后，重新 ``git add -> pre-commit`` ，直至没有 Failed 文件。
```
➜  pre-commit
CRLF end-lines remover...............................(no files to check)Skipped
yapf.....................................................................Failed
- hook id: yapf
- files were modified by this hook
Check for added large files..............................................Passed
Check for merge conflicts................................................Passed
Check for broken symlinks................................................Passed
Detect Private Key...................................(no files to check)Skipped
Fix End of Files.........................................................Passed
clang-format.........................................(no files to check)Skipped
cpplint..............................................(no files to check)Skipped
pylint...................................................................Failed
- hook id: pylint-doc-string
- exit code: 127

./tools/codestyle/pylint_pre_commit.hook: line 11: pylint: command not found

copyright_checker........................................................Passed
```
- 填写提交说明：Git 每次提交代码，都需要写提交说明，让其他人知道这次提交做了哪些改变，可以通过 ``git commit`` 完成：

```
➜  git commit -m "fix docs bugs" 
```

### 3.2 确保本地仓库是最新的

在准备发起 Pull Request 之前，需要同步原仓库（https://github.com/PaddlePaddle/docs）最新的代码。

首先通过 ``git remote`` 查看当前远程仓库的名字。

```
➜  git remote
origin
➜  git remote -v
origin	https://github.com/USERNAME/docs (fetch)
origin	https://github.com/USERNAME/docs (push)
```

这里 origin 是你 clone 的远程仓库的名字，也就是自己用户名下的 Paddle，接下来创建一个原始 Paddle 仓库的远程主机，命名为 upstream。

```
➜  git remote add upstream https://github.com/PaddlePaddle/docs
➜  git remote
origin
upstream
```

获取 upstream 的最新代码并更新当前分支。
```
➜  git fetch upstream
➜  git pull upstream develop
```


### 3.3 Push 到远程仓库

将本地的修改推送到 GitHub 上，也就是 https://github.com/USERNAME/docs。

```
# 推送到远程仓库 origin 的 my-cool-stuff 分支上
➜  git push origin my-cool-stuff
```

## 四、提交PR

在你push后在对应仓库会提醒你进行PR操作，点击后，按格式填写PR内容，即可。


## 五、review&merge

提交PR后，可以指定 Paddle 的同学进行 Review。目前 Paddle 负责文档的同学是 @TCChenLong、@jzhang533、@saxon-zh、@Heeenrrry、@dingjiaweiww等 。


## CI

Paddle 中与文档相关的CI 流水线是 `FluidDoc1`等，主要对以下几个方面进行检查:

- 检查PR CLA
- 检查增量修改的API是否需要相关人员审核
- 若需要执行示例代码则执行看能否正常运行

如果无法通过该CI，请点击对应CI的details，查看CI运行的的log，并根据log修改你的PR，直至通过CI。
未选择任何文件