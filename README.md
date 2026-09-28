# bhpan-cli

北航网盘（bhpan.buaa.edu.cn）命令行工具，支持上传 / 下载 / 管理文件和外链分享，适合在无 GUI 的服务器或终端里使用。

本仓库基于 [xdedss/dist_bhpan](https://github.com/xdedss/dist_bhpan)（MIT 许可）修改，修复了新版网盘 API 的兼容问题：

- 修复 `ls` 等命令报 `HTTP 400: attr: Invalid type. Expected: boolean, given: string` —— 新版 API 要求 `/dir/list` 接口的 `attr` 参数传布尔值，原版传的是字符串

## 安装

pip 一行安装（需要 git 和 Python >= 3.6）：

```bash
pip install git+https://github.com/Tukist/bhpan-cli.git
```

## 使用

第一次运行会提示输入北航网盘账号（学号）和密码，凭据用网盘公钥加密后保存到：

- Windows: `AppData/Roaming/bhpan/config.json`
- Linux: `~/.local/share/bhpan/config.json`

不想保存密码的话，把 `config.json` 里的 `store_password` 改为 `false`，之后每次手动输入。

### 命令一览

```bash
# 列目录（-h 显示可读的文件大小）
bhpan ls home
bhpan ls home -h

# 上传（目录加 -r）
bhpan upload 本地文件 home/目标目录
bhpan upload 本地文件夹 home/目标目录 -r

# 下载（目录加 -r）
bhpan download home/文件 本地目录
bhpan download home/文件夹 本地目录 -r

# 查看文件信息 / 直接读取内容（可接管道）
bhpan ls home/xxx.txt
bhpan cat home/xxx.txt | tail

# 删除（目录加 -r）
bhpan rm home/xxx
bhpan rm home/目录 -r

# 重命名 / 移动 / 复制（-f 覆盖已存在文件）
bhpan mv home/test.png home/test2.png
bhpan mv home/dir1/test.png home/dir2/dir3
bhpan cp home/a home/dir

# 创建多级目录
bhpan mkdir home/test/1/2/3

# 外链分享
bhpan link show home/xxx                      # 查看已有外链
bhpan link create home/xxx -e 7               # 创建外链，7 天后过期（默认 30 天）
bhpan link create home/xxx -p --allow-upload  # 带密码、允许上传
bhpan link delete home/xxx                    # 停止分享
```

说明：

- `home` 是文档根目录的别名，也可以用完整路径。
- 用 `-u 用户名` 可以临时以另一个账号登录：`bhpan -u 学号 ls home`。

## 许可

MIT，见 [LICENSE](LICENSE)。原项目版权归 [xdedss](https://github.com/xdedss) 所有。
