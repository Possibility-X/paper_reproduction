# 原项目引用

已选定 Git submodule，按 `original/<paper-id>/` 组织原项目，并固定具体提交。当前尚未添加子模块，等待上游仓库 URL；可同时指定论文对应的 tag 或 commit，未指定时记录添加时默认分支的提交。

根目录 `.gitignore` 忽略未登记的原项目目录；添加子模块时仅放行对应路径，防止把其他源码作为普通文件提交。

## 方案比较

| 方案 | 主仓库保存什么 | 版本追踪 | 适用情况 |
| --- | --- | --- | --- |
| Git submodule（推荐） | 仓库 URL、路径和固定 commit 指针；不保存上游源码文件 | Git 原生固定提交 | 希望代码在工作区中可运行，并准确复现实验版本 |
| URL + commit 清单，手动或脚本 clone | 来源和版本清单、可选下载脚本 | 需自行 checkout 并核验 SHA | 希望主仓库简单，不使用子模块命令 |
| 仅 README 超链接 | 仓库链接 | 不固定版本时无法还原当时的代码 | 只需导航、阅读参考 |
| Git subtree | 上游代码文件，可同时引入历史 | 在主仓库中管理 | 希望直接修改并统一提交；不符合仅引用的需求 |

Fork 是在 GitHub 上维护独立仓库的方式，不是目录引用机制。若需要修改上游代码，可以 fork 后用 submodule 引用自己的 fork；修改在 fork 提交，主仓库再更新指针。

## 添加子模块

先在根 `.gitignore` 末尾添加 `!/original/<paper-id>`，将占位符替换为实际论文标识，然后执行：

```bash
git submodule add <upstream-url> original/<paper-id>
git -C original/<paper-id> checkout <full-commit-sha>
git -C original/<paper-id> submodule update --init --recursive
git add .gitignore .gitmodules original/<paper-id>
git commit -m "Reference upstream implementation at a fixed commit"
```

首次克隆完整工作区：

```bash
git clone --recurse-submodules https://github.com/Possibility-X/paper_reproduction.git
```

已克隆主仓库时获取固定版本：

```bash
git submodule update --init --recursive
```

主仓库只记录子模块提交指针。子模块工作区中的未提交修改不会随主仓库提交上传；更新上游版本应单独提交指针变更，并记录对实验的影响。引用的远端和提交必须持续可访问。

参考：[Git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules)、[GitHub subtree merges](https://docs.github.com/en/get-started/using-git/about-git-subtree-merges)。
