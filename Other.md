# 杂谈

## 2017-01-05

### Git 把远程分支 clone 到本地

```bash
git checkout 远程分支名 -b 本地分支名
```

### Git log 相关

普通查看日志：

```bash
gg # alias to git lg
```

加上文件名查看文件的改动历史，常用参数 `--author`、`--since/until`、`-p`(打印详细改动内容)

## 2016-11-30

### Git 提交 no verify

使用 precommit hook 的时候，有时候发生误报，此时使用 `--no-verify` 选项即可强制提交：

```bash
gcm "x" --no-verify
```

## 2016-11-24

### git branch

如果当前的 head 不在某个分支上，可以调用 `git branch -f branch_name` 将分支强制指向当前 head 的位置 
