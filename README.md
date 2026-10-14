# paper_reproduction

论文复现工作区：记录论文、上游实现、独立复现代码以及可追溯的实验结果。

| 目录 | 用途 |
| --- | --- |
| `paper/` | 论文、补充材料、阅读笔记及引用信息 |
| `original/` | 原开源项目的引用及版本记录 |
| `reproduction/` | 自己编写的复现代码、依赖声明和测试 |
| `experiments/` | 实验配置、运行说明、指标、图表及结果分析 |

多篇论文建议在四个目录中使用相同的论文标识，如 `paper/<paper-id>/`、`reproduction/<paper-id>/`。

## 上游代码组织

采用 Git submodule，在 `original/<paper-id>/` 引用原开源项目，并固定完整 commit SHA。主仓库保存上游地址和提交指针，源码保留在独立仓库中。尚未添加上游仓库，等待提供仓库地址；使用方式见 [original/README.md](original/README.md)。

## 实验记录

每次实验记录论文标识、复现代码 commit、上游 commit、环境和依赖版本、数据版本及划分、随机种子、完整命令、配置、指标和结论。

提交小型配置、指标表和最终图表；数据集、模型权重、运行缓存放在忽略的目录中，通过下载地址和校验值记录来源。不要将依赖环境或原项目源码作为普通文件提交。

## 首次提交与推送

在具有 Git 写权限和 GitHub 网络访问的终端中运行：

```bash
cd /mnt/e/paper_reproduction
git init -b main
# 若未配置提交身份，请先设置自己的 user.name 和 user.email。
git add README.md .gitignore .gitattributes paper/README.md original/README.md reproduction/README.md experiments/README.md
git commit -m "Initialize paper reproduction workspace"
git remote add origin https://github.com/Possibility-X/paper_reproduction.git
git push -u origin main
```

以上命令面向首次初始化；若已有仓库或远端配置，应先检查 `git status` 与 `git remote -v`。若远端已有提交，应先获取并整合历史。
