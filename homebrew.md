#### 切换node版本

- 如果之前使用 brew install node 安装过node, 需要先执行 brew unlink node 来'解绑'node
- 查找可用的node版本 brew search node
- 安装需要的版本, 比如 brew install node@8
- 然后 brew link node@8, 这一步可能会报错, 按照提示执行命令, brew link --overwrite --force node@8

#### 搜索
brew search xxx 例如 brew search mysql

#### 安装
brew install xxx 例如：brew install mysql

#### 查询
brew info xxx 例如：brew info mysql 主要查看具体的信息及依赖关系当前版本注意事项等

#### 更新
如果想要更新到当前最新的版本要先把当前 brew 更新到最新。
brew update 这个时候他会先更新自己到最新 接下来的操作才更有意义

#### 检测新版本
brew outdated 会列出所有有新版本的程序

#### 升级
brew upgrade 升级所有 当然也可以指定升级
brew upgrade xxx指定的升级的程序名

#### 清理
brew cleanup 清理不需要的版本及其安装缓存

#### 删除
brew uninstall xxx删除不需要的程序

#### 更多命令详见
man brew
