## git配置
### git配置文件位置

1. ```/etc/gitconfig```文件. 包含系统上每一个用户及他们仓库的通用配置. 如果在执行 git config 时带上 ```--system``` 选项，那么它就会读写该文件中的配置变量
2. ```~/.gitconfig``` 或 ```~/.config/git/config``` 文件. 只针对当前用户。 你可以传递 ```--global``` 选项让 Git 读写此文件，这会对你系统上 所有 的仓库生效
3. 当前使用仓库的 Git 目录中的 config 文件(即 .git/config). 针对该仓库。 你可以传递 --local 选项让 Git 强制读写此文件, 虽然默认情况下用的就是它. (当然, 你需要进入某个 Git 仓库中才能让该选项生效)

每一个级别会覆盖上一级别的配置，所以 .git/config 的配置变量会覆盖 /etc/gitconfig 中的配置变量

### git配置常用命令

1. 可以通过以下命令查看所有的配置以及它们所在的文件
```
git config --list --show-origin
```
2. 通过 git config [key] 查看某一项配置
``` 
git config user.name
```
3. 通过 git config [key] [value]配置

```
git config --global user.name "Joy"
git config --global user.email joy@example.com
```

## git帮助

```
git help <verb>
git <verb> --help
man git-<verb>
```
如果你不需要全面的手册，只需要可用选项的快速参考，那么可以用 -h 选项获得更简明的 “help” 输出

```
git add -h
git push --help
```





1. 关联远程分支：git push --set-upstream origin xxx
2. 删除远程分支：git push origin --delete xxx
3. 从远程分分支co：git checkout -b xxx origin/xxx
