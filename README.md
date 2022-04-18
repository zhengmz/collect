# 仓库收集

用来收集仓库，避免过多的 Fork

而且可以进行分类汇总

## 使用说明：

- 注释使用 #，可单行，也可以放行尾
- 暂时只支持 git 仓库

## 配置项介绍

配置文件为 `feeds.conf`，每行一个同步

### repo

源仓库 `URL`，如 `https://github.com/coolsnowwolf/lede.git`

### branch

需要同步的分支名，默认为 `master`

### srcdir

需要同步的子目录列表，留空默认为整个仓库，包括隐藏目录如 `.github`

配置多个子目录时，需要用 `:` 隔开，如 `rom1:rom2`

- 可以使用通配符 `*`，指仓库主目录下的所有非隐藏子目录，而 `*:.*` 相当于整个仓库
- 隐藏目录需要单独配置，如 `.github` 指同步 `.github` 目录
- 目录名支持多级，如 `application/luci-app-acceesscontrol`
- 多子目录中，如果最后目录名相同，会合并内容，相同文件以最后一个为主

### destdir

目标目录名称，留空保存目录名不变

可以使用两个变量名称

- `%u` 源仓库的所有者
- `%n` 原仓库名称

如 `OpenWrt-%u`，会解析为 `OpenWrt-coolsnowwolf`

支持多级目录名，如 `packages/%n`

目标目录不存在，会自动创建

### srcdir 与 destdir 组合使用

- srcdir 未配置，destdir 未配置：指直接把源仓库当做一个目录同步到当前仓库的根目录下
- srcdir 配置，destdir 未配置：指把源仓库中的 srcdir (包括多目录) 同步到当前仓库的根目录下
- srcdir 未配置，destdir 配置：指把源仓库当作一个目录
  - 当 destdir 不存在，则源仓库改名，同步到当前仓库的根目录下
  - 若 destdir 已存在，则会把源仓库当子目录放在 destdir 目录下
- srcdir 配置，destdir 配置: 
  - 当 srcdir 为单目录时
    - 若 destdir 不存在，则将 srcdir 改名，同步到当前仓库中的根目录下
    - 若 destdir 已存在，则将 srcdir 放在 destdir 目录下
  - 当 srcdir 为多目录时，则会放到 destdir 目录下，destdir 不存在会自动创建

同步使用 `cp -a`

当 destdir 已存在，又有同名目录文件，后面会覆盖前面的文件内容

