## reprepro

<p align="center"><a href="README.md">English</a> | <a href="README-zh.md">中文</a></p>

本项目为 [reprepro](https://salsa.debian.org/debian/reprepro) 构建多架构 Docker 镜像，reprepro 是用于管理 Debian 软件包仓库的工具，可处理 `.deb` 包、软件包池及生成仓库元数据。

镜像基于 **Anolis OS**，支持以下架构：

| 架构         | 平台            |
|------------|---------------|
| 🐉 loong64 | linux/loong64 |
| 💻 amd64   | linux/amd64   |
| 📱 arm64   | linux/arm64   |

主要目标是将 reprepro 带到 **LoongArch (loong64)** 平台，同时也提供 amd64 和 arm64 版本以便使用。

## 分支与版本

| 分支      | reprepro 版本 | 状态    |
|---------|-------------|-------|
| `1.2.4` | 1.2.4       | 活跃    |
| `main`  | —           | 仓库根目录 |

分支名对应上游 [reprepro](https://salsa.debian.org/debian/reprepro) 的发行版本。标签格式为 `<reprepro-版本>+<构建编号>`（例如 `1.2.4+2`）。

## Docker 镜像

镜像发布在 Docker Hub 的 [`kubernetesloong64/reprepro`](https://hub.docker.com/r/kubernetesloong64/reprepro) 命名空间下。

### 多架构清单

多架构清单会自动选择适合您平台的架构：

| 镜像标签                                      | 平台                                    |
|-------------------------------------------|---------------------------------------|
| `kubernetesloong64/reprepro:1.2.4-anolis` | linux/amd64、linux/arm64、linux/loong64 |

### 架构特定镜像

| 镜像标签                                              | 平台            |
|---------------------------------------------------|---------------|
| `kubernetesloong64/reprepro:1.2.4-anolis-amd64`   | linux/amd64   |
| `kubernetesloong64/reprepro:1.2.4-anolis-arm64`   | linux/arm64   |
| `kubernetesloong64/reprepro:1.2.4-anolis-loong64` | linux/loong64 |

### 使用方式

将 `.deb` 包添加到 Debian 仓库：

```shell
docker run --rm -v $(pwd)/debs:/debs -v $(pwd)/repo:/repo kubernetesloong64/reprepro:1.2.4-anolis reprepro -b /repo includedeb stable /debs/*.deb
```

更新已有仓库（从上游拉取更新的包）：

```shell
docker run --rm -v $(pwd)/repo:/repo kubernetesloong64/reprepro:1.2.4-anolis reprepro -b /repo update stable
```

## 验证发布

- 发布文件使用 GPG 签名。
- 从 [keys.openpgp.org](https://keys.openpgp.org) 下载公钥。
- 指纹：[FCF8724722CCBF9F51B1FBE376532BE7E3013105](https://keys.openpgp.org/debug?q=FCF8724722CCBF9F51B1FBE376532BE7E3013105)
- [手动下载](https://keys.openpgp.org/vks/v1/by-fingerprint/FCF8724722CCBF9F51B1FBE376532BE7E3013105)

```shell
gpg --keyserver keys.openpgp.org --recv-keys FCF8724722CCBF9F51B1FBE376532BE7E3013105
echo "FCF8724722CCBF9F51B1FBE376532BE7E3013105:6:" | gpg --import-ownertrust
```

或者，手动下载公钥文件后导入：

```shell
gpg --import /tmp/xxx
```

## 许可证

[Apache License 2.0](LICENSE)
