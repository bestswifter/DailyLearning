# 杂谈

## 2016-11-30

### Git 提交 no verify

使用 precommit hook 的时候，有时候发生误报，此时使用 `--no-verify` 选项即可强制提交：

```bash
gcm "x" --no-verify
```

## 2016-11-24

### git branch

如果当前的 head 不在某个分支上，可以调用 `git branch -f branch_name` 讲分支强制指向当前 head 的位置 


